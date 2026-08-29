![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [ContextMenu](05_contextmenu_ru.md) | След.: [Icon](07_icon_ru.md) | [English](06_tooltipbox_en.md)

---

# TooltipBox

Шаблон подсказки — в отличие от нативного тултипа RimWorld, содержимое может быть ЛЮБЫМ
элементом, не только текстом. Появляется при наведении, следует за курсором, не перехватывает
клики — можно кликать «сквозь» неё по элементу под ней.

`RimUI.Elements.TooltipBox : UiElement` (sealed)

## Пример

```csharp
var body = new Field { Style = { Gap = 6f } };
body.Add(new Text("Заголовок подсказки"));
body.Add(new Divider());
var tip = new TooltipBox { Target = Button.Make("Наведи на меня"), Content = body };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Target` | `UiElement` | `null` | Элемент-носитель подсказки (в потоке). |
| `Content` | `UiElement` | `null` | Шаблон подсказки: любой элемент/контейнер. |
| `CursorOffset` | `Vec2` | `(14, 20)` | Смещение панели от курсора. |

`CursorOffset` смещает именно ВЕРХНИЙ-ЛЕВЫЙ угол панели относительно позиции курсора (панель
появляется на 14px правее и 20px ниже кончика курсора — курсор RimWorld рисуется от своей
«горячей точки», поэтому такой сдвиг нужен, чтобы панель не перекрывала его). Клампинг в границы
окна — тот же алгоритм, что и у `Popover`/`DropdownMenu`: панель поджимается строго впритык к
краю (без запаса в пикселях), если не помещается.

## Стили

Слот панели — `Theme.TooltipPanel`.

## Методы

Явного публичного конструктора с параметрами нет — только `new TooltipBox { ... }` через
инициализатор полей.

| Метод | Возвращает | Описание |
|---|---|---|
| `SetStyle(string path, string value)` | — | Изменить стиль строкой (применяется к корневому `Style` — именованных частей у `TooltipBox` нет). Опечатка в пути — молча игнорируется. |

## События

Собственного колбэка появления/исчезновения нет — управляется наведением курсора на `Target`.
Универсальные события доступны как у любого элемента:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы `TooltipBox` (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У `TooltipBox` нет своего disabled — не сработает. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `Theme.TooltipPanel`. |
| Спрайт-рамка | Да | `ps.Sprite`, `SpriteBox`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Своего понятия «отключена» нет. |
| Hover-fade | Нет | Появление/исчезновение мгновенное по наведению, без плавного перехода. |

Панель зажимается в границах окна, исчезает автоматически при уходе курсора (нет
toggle-состояния, в отличие от `Popover`), НЕ регистрируется как overlay-rect (не перехватывает
клики/hover под собой).


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [ContextMenu](05_contextmenu_ru.md) | След.: [Icon](07_icon_ru.md) | [English](06_tooltipbox_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
