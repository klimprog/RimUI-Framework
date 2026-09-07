![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.3.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Устройство и расширение | Пред.: [Архитектура: как фреймворк устроен внутри](01_architecture_ru.md) | [English](02_custom_component_en.md)

---

# Свой компонент

Готовых компонентов много, но рано или поздно понадобится что-то своё. Эта страница разбирает
целый рабочий компонент — не псевдокод: приведённый ниже класс компилируется и работает как есть.

Для примера напишем **Spoiler** — заголовок, по клику раскрывающий содержимое. Он маленький, но
задействует всё, что нужно знать: три фазы кадра, стиль и тему, внутреннее состояние, ввод,
курсор и колбеки.

Перед чтением полезно понимать модель кадра — она описана на странице
[Архитектура](01_architecture_ru.md).

## Полный код

```csharp
using RimUI.Core;
using RimUI.Elements;
using RimUI.Rendering;
using RimUI.Styling;

namespace MyMod.Ui
{
    public sealed class Spoiler : UiElement
    {
        public string Title { get; set; } = "";
        public UiElement Content { get; set; }
        public float HeaderHeight { get; set; } = 26f;
        public bool Disabled { get; set; }
        public System.Action<bool> OnToggle { get; set; }

        // --- состояние между кадрами (ID-store) ---
        private sealed class SpoilerState : WidgetState
        {
            public bool Open;
        }

        private SpoilerState St(UiState s)
            => s != null ? s.GetOrCreate(s.MakeId(StateKey) + "/spoiler", () => new SpoilerState()) : null;

        public bool IsOpen(LayoutContext ctx)
        {
            SpoilerState st = ctx != null ? St(ctx.State) : null;
            return st != null && st.Open;
        }

        public void SetOpen(LayoutContext ctx, bool open)
        {
            SpoilerState st = ctx != null ? St(ctx.State) : null;
            if (st != null) st.Open = open;
        }

        public override bool EffectiveDisabled => Disabled;

        // Буферы: переиспользуются между кадрами, чтобы не мусорить аллокациями.
        private readonly Style _headerStyle = new Style { Radius = BorderRadius.Small };
        private TextStyle _titleTs = new TextStyle { VAlign = VerticalAlign.Middle, Wrap = false };

        // ---------- фаза 1: сколько нужно места ----------
        public override SizeF Measure(SizeF available, LayoutContext ctx)
        {
            ResolveStyle(ctx);
            Style s = RS;
            Thickness m = EffMargin(s);
            Thickness pad = MeasurePadding(s);

            float innerW = available.Width - pad.Horizontal - m.Horizontal;
            float h = HeaderHeight;

            if (Content != null && IsOpen(ctx))
            {
                SizeF c = Content.Measure(new SizeF(innerW, available.Height), ctx);
                h += s.Gap + c.Height;
            }

            Desired = new SizeF(available.Width, h + pad.Vertical + m.Vertical);
            return Desired;
        }

        // ---------- фаза 2: раздать места ----------
        public override void Arrange(RectF finalRect, LayoutContext ctx)
        {
            Bounds = finalRect;
            RectF inner = ContentRect(finalRect);

            if (Content == null || !IsOpen(ctx)) return;

            float top = inner.Y + HeaderHeight + RS.Gap;
            Content.Arrange(new RectF(inner.X, top, inner.Width, inner.Bottom - top), ctx);
        }

        // ---------- фаза 3: нарисоваться и обработать ввод ----------
        public override void Emit(DrawList list, LayoutContext ctx)
        {
            Theme t = ctx.Theme ?? Theme.Default;
            UiState s = ctx.State;
            SpoilerState st = St(s);

            RectF inner = ContentRect(Bounds);
            var header = new RectF(inner.X, inner.Y, inner.Width, HeaderHeight);

            bool hover = !Disabled && s != null && s.HoverOn(header);
            _headerStyle.Background = Fill.Solid(hover ? t.MenuItemHover : t.SurfaceAlt);
            _headerStyle.BorderColor = t.BorderColor;
            _headerStyle.BorderWidth = BorderWidth.Small;
            list.Add(DrawCommand.Background(header, _headerStyle));
            list.Add(DrawCommand.Border(header, _headerStyle));

            bool open = st != null && st.Open;
            var chev = new RectF(header.X + 6f, header.Y + (HeaderHeight - 12f) * 0.5f, 12f, 12f);
            list.Add(DrawCommand.ImagePart(chev, Icons.AtlasKey,
                Icons.Rect(open ? Icons.ChevronDown : Icons.ChevronRight),
                Disabled ? t.ControlDisabled : t.TextMuted));

            _titleTs.Color = Disabled ? t.ControlDisabled : t.TextColor;
            list.Add(DrawCommand.Label(new RectF(chev.Right + 6f, header.Y,
                                                 header.Right - chev.Right - 10f, header.Height),
                                       Title ?? "", _titleTs));

            if (!Disabled && s != null)
            {
                if (hover) s.RequestCursor(CursorKind.Hand);
                if (s.ClickOn(header) && st != null)
                {
                    st.Open = !st.Open;
                    if (OnToggle != null) OnToggle(st.Open);
                }
            }

            if (open && Content != null) Content.EmitStyled(list, ctx);
        }
    }
}
```

Применение ничем не отличается от встроенных компонентов:

```csharp
var body = new FlexBox(Axis.Column) { Style = { Gap = 6f } };
body.Add(new Text("Первая строка содержимого"));
body.Add(new Text("Вторая строка"));

var sp = new Spoiler
{
    Key = "settings_advanced",       // нужен, если элемент пересоздаётся; см. ниже
    Title = "Дополнительно",
    Content = body,
    Style = { Gap = 6f, Padding = new Thickness(4f) }
};
```

## Разбор по частям

### Наследование и три фазы

Компонент наследует `UiElement` и переопределяет три метода — `Measure`, `Arrange`, `Emit`.
Порядок их вызова обеспечивает фреймворк, звать их вручную не нужно.

`Measure` **обязан** записать `Desired`, `Arrange` — `Bounds`. Родитель полагается на эти поля;
забыть их — значит получить элемент нулевого размера или в углу окна.

Обратите внимание, что размер зависит от состояния: закрытый спойлер занимает только высоту
заголовка и не меряет содержимое вовсе. Это нормально и типично — в immediate-mode «свернуть»
означает буквально «в этом кадре не учитывать».

### Стиль: `RS`, а не `Style`

`Style` — то, что задал пользователь компонента. `RS` — **резолвнутый** стиль: пользовательский,
дополненный дефолтами темы. Внутри компонента всегда работайте с `RS`, а перед первым обращением
к нему в `Measure` вызовите `ResolveStyle(ctx)` — тема может смениться на лету, и резолв
пересчитывается каждый кадр.

Три вспомогательных метода базового класса избавляют от ручной арифметики отступов:

- `EffMargin(s)` — внешний отступ с учётом спрайт-рамки;
- `MeasurePadding(s)` — внутренний отступ для `Measure`;
- `ContentRect(rect)` — прямоугольник содержимого: `rect` минус margin, padding и кромка спрайта.

`Measure` и `ContentRect` обязаны быть согласованы: если посчитать размер без padding, а рисовать
внутри `ContentRect`, содержимое не влезет.

### Тема: берите цвета из палитры

Компонент не задаёт цвета константами — он берёт их из темы: `t.SurfaceAlt`, `t.BorderColor`,
`t.TextColor`, `t.TextMuted`, `t.MenuItemHover`, `t.ControlDisabled`, `t.Accent`. Тогда ваш
компонент автоматически подхватывает любую тему пользователя и выглядит своим.

> Встроенные компоненты используют ещё и **слоты** (`t.Slot("button")`) — именованные наборы
> дефолтов. Для внешнего компонента этот путь менее удобен: слоты объявляются в общем реестре
> до построения тем, и порядок инициализации становится важен. Проще и надёжнее — палитра выше
> плюс свои публичные `Style`-поля для того, что пользователь должен настраивать.

### Состояние: ID-store и `StateKey`

Раскрыт спойлер или нет — это внутренняя механика виджета, и живёт она в ID-store:

```csharp
private sealed class SpoilerState : WidgetState { public bool Open; }

s.GetOrCreate(s.MakeId(StateKey) + "/spoiler", () => new SpoilerState())
```

Суффикс `"/spoiler"` отделяет ваше состояние от чужого под тем же ключом.

**Обращайтесь к состоянию через `StateKey`, а не через `Key`.** `StateKey` — это `Key`, если он
задан, иначе автоключ, привязанный к самому объекту элемента. Разница принципиальна:
`MakeId(null)` выдаёт номер ПО СЧЁТЧИКУ ВЫЗОВОВ, а компонент трогает состояние дважды за кадр —
в `Measure` и в `Emit`. С `Key = null` он прочитал бы состояние под одним номером, а записал под
другим, и это выглядело бы как «компонент без ключа не работает».

**Если пользователь задал `Key`, тот обязан быть стабильным между кадрами.** Ключ вроде `"sp" + i`
в цикле — нормально, если `i` стабилен. Ключ, зависящий от времени, случайного числа или порядка
сортировки — источник трудноуловимых багов: состояние «теряется», спойлер захлопывается сам.

Обратите внимание на пару `IsOpen(ctx)` / `SetOpen(ctx, open)`. Состояние лежит в `UiState`, но
пользователь компонента не должен туда лазить — дайте ему методы. Это же соглашение действует
во всём фреймворке (`GetSelected()`, `SetValue()` и подобные).

### Ввод

Ввод обрабатывается в `Emit`, потому что только там известны окончательные `Bounds`:

```csharp
bool hover = !Disabled && s != null && s.HoverOn(header);
if (s.ClickOn(header)) { ... }
```

Полезное из `UiState`: `HoverOn(rect)`, `ClickOn(rect)`, `RightClicked`, `MouseDown`,
`MousePosition`, `ScrollDelta`, `CtrlDown`, `ShiftDown`, а для перетаскивания —
`Capture(id)` / `IsCaptured(id)` / `ReleaseCapture()`.

Проверяйте `Disabled` **до** реакции на ввод, а не только при отрисовке: неактивный элемент,
который всё равно кликается, — частая ошибка.

### Курсор

Одна строка делает компонент «живым»:

```csharp
if (hover) s.RequestCursor(CursorKind.Hand);
```

Заявку делает элемент под мышью; побеждает самый глубокий, а применяется курсор один раз за кадр.
Подробности — на странице про стили.

### Дети

Ребёнок обходится теми же тремя фазами: `Content.Measure(...)`, `Content.Arrange(...)` и
`Content.EmitStyled(list, ctx)`.

Для детей вызывайте именно **`EmitStyled`**, а не `Emit`: это обёртка, которая дополнительно
применяет анимацию из стиля, пользовательские события и заявку курсора. `Emit` — метод, который
переопределяете вы; `EmitStyled` — который вызываете для чужих элементов.

### События

Колбек `OnToggle` — «своё» событие компонента. Дополнительно любой `UiElement` уже поддерживает
общий набор событий, ничего для этого писать не нужно:

```csharp
sp.Events.HoverEnter = (sender, data) => Log.Message("вошли");
sp.Events.Click = (sender, data) => Log.Message("клик в " + data.MousePosition.X);
```

Блок `Events` создаётся лениво, так что элементы без подписок ничего не стоят.

Если у компонента есть неактивное состояние, переопределите `EffectiveDisabled` — от него
зависит событие `DisabledChanged` и то, будет ли элемент заявлять курсор.

## Чек-лист

- [ ] `Measure` записывает `Desired`, `Arrange` записывает `Bounds`.
- [ ] `ResolveStyle(ctx)` вызван в начале `Measure`, дальше работаем с `RS`.
- [ ] Отступы считаются через `EffMargin` / `MeasurePadding` / `ContentRect`, и `Measure`
      согласован с тем, куда вы рисуете.
- [ ] Цвета — из темы, а не константами.
- [ ] Состояние — в ID-store по `StateKey` (не по `Key`), наружу — методами.
- [ ] Ввод — в `Emit`, с проверкой `Disabled`.
- [ ] Для детей — `EmitStyled`, не `Emit`.
- [ ] Буферы (`Style`, `TextStyle`, списки) созданы один раз в полях, а не в каждом кадре.

## Частые ошибки

**Забыт `Desired` или `Bounds`.** Элемент схлопывается в ноль либо рисуется не там. Самая частая
ошибка первого компонента.

**Новый `Style` в каждом кадре.** Команда отрисовки хранит **ссылку** на стиль, а не копию.
Если в цикле переиспользовать один буфер под разные строки, все они получат стиль последней.
Либо буфер на элемент списка, либо не менять его между командами.

**Состояние по `Key` вместо `StateKey`.** С незаданным ключом `MakeId(null)` выдаёт разные номера
в `Measure` и в `Emit`: компонент читает одно состояние, а пишет другое. Симптом — «работает только
если задать Key».

**Нестабильный `Key`.** Если ключ всё же задан, но меняется между кадрами, состояние теряется:
скролл прыгает, спойлер закрывается, `HoverEnter` стреляет без остановки.

**Работа с данными в `Emit`.** Поиск по карте, чтение файла или `LINQ` по большому списку внутри
построения кадра выполняются 60 раз в секунду. Считайте заранее, а в компонент передавайте готовое
значение или делегат `Func<T>`.

**Unity внутри компонента.** Компоненты живут в ядре, которое не знает про Unity. Если понадобился
`UnityEngine`, скорее всего задача решается командой отрисовки, а не прямым рисованием.


---

[Оглавление](../index_ru.md) - Устройство и расширение | Пред.: [Архитектура: как фреймворк устроен внутри](01_architecture_ru.md) | [English](02_custom_component_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
