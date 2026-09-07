![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.3.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Отображение | Пред.: [ImageBox](01_imagebox_ru.md) | След.: [Gallery](03_gallery_ru.md) | [English](02_customdraw_en.md)

---

# CustomDraw

Своя отрисовка: `OnDraw` рисует через команды фреймворка (переносимо между темами и масштабом),
`OnDrawNative` даёт прямой доступ к `Widgets`/GUI RimWorld, если нужен полный контроль.

`RimUI.Components.CustomDraw : UiElement`

## Пример

```csharp
var chart = new CustomDraw
{
    Style = { Width = 220f, Height = 90f },
    OnDraw = (list, rect, style, ctx) =>
    {
        var bar = new RectF(rect.X, rect.Bottom - h, w, h);
        list.Add(DrawCommand.Background(bar, new Style { Background = Fill.Solid(Blue) }));
    }
};
var native = new CustomDraw
{
    Style = { Width = 220f, Height = 60f },
    OnDrawNative = r => Widgets.Label(UnitConv.ToRect(r), "Текст")
};
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `OnDraw` | `Action<DrawList, RectF, Style, LayoutContext>` | `null` | Портируемый путь — рисует нашими `DrawCommand` (переносимо между темами/масштабом). |
| `OnDrawNative` | `Action<RectF>` | `null` | Сырой путь — получает прямоугольник в оконных координатах для вызова `Widgets.*` напрямую. |
| `DefaultWidth` | `float` | `200` | Ширина по умолчанию, если `Style.Width` не задан. |
| `DefaultHeight` | `float` | `120` | Высота по умолчанию, если `Style.Height` не задан. |

## Стили

Обычный `Style` элемента-контейнера; порядок отрисовки: сначала базовый фон/бордюр по `Style`,
потом `OnDraw`, затем `OnDrawNative` (поверх всего).

## Методы

Явного публичного конструктора с параметрами нет — только `new CustomDraw { ... }` через
инициализатор полей.

| Метод | Возвращает | Описание |
|---|---|---|
| `SetStyle(string path, string value)` | — | Установить поле стиля строкой (`"text.color"`, `"background"`, `"radius"` и т.д.). Именованных частей стиля (`StylePart`) у `CustomDraw` нет — путь применяется напрямую к стилю элемента. Неизвестный путь/значение — тихо игнорируется. |

## События

Собственных событий (в смысле колбэков состояния) нет — `OnDraw`/`OnDrawNative` вызываются
каждый кадр как часть отрисовки, а не как реакция на действие пользователя. Однако доступны
универсальные события `UiElement.Events` (есть у любого элемента):

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР (~60 раз/сек). Обработчик должен быть лёгким: без аллокаций, поиска, IO — тяжёлая логика здесь просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Полностью, как у любого элемента-контейнера. |
| Спрайт-рамка | Косвенно | `Style.Sprite` работает через базовый `UiElement.Emit`, спец-слота темы нет. |
| Анимация | Да, с оговоркой | `Style.Animation` доступен базово; при обёртке `Animated` смещение/масштаб применяются и к `OnDrawNative`, а вот **прозрачность — нет** (сырой `Widgets.*`-вызов не проходит через слой адаптера, вплетающий `Opacity`). |
| Disabled | Нет | Своего понятия «отключён» нет. |
| Hover-fade | Вручную | Можно вызвать `HoverFade.K(...)` внутри `OnDraw` самому — так и сделано в демонстрационном примере (`sc_an_hoverfade`). |


---

[Оглавление](../index_ru.md) - Отображение | Пред.: [ImageBox](01_imagebox_ru.md) | След.: [Gallery](03_gallery_ru.md) | [English](02_customdraw_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
