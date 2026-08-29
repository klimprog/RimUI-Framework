![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Button](01_button_ru.md) | След.: [DropdownMenu](03_dropdownmenu_ru.md) | [English](02_popover_en.md)

---

# Popover

Всплывающая панель у точки-якоря (обычно у кнопки): открывается по клику, внутри — любое
содержимое, клик мимо панели закрывает её.

`RimUI.Elements.Popover : UiElement` (sealed)

## Пример

```csharp
var pop = new Popover { Trigger = Button.Make("Текст ↓") };
pop.Content = new Text("Содержимое попапа") { Style = { Text = new TextStyle { Wrap = true } } };

var pop2 = new Popover { Trigger = Button.Make("Панель"), Placement = OverlayPlacement.Right };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Trigger` | `UiElement` | `null` | Якорь в потоке (кликабельная зона, открывает/закрывает панель). |
| `Content` | `UiElement` | `null` | Содержимое панели (любой элемент). |
| `Placement` | `OverlayPlacement` (`Bottom`\|`Top`\|`Right`\|`Left`) | `Bottom` | Сторона раскрытия относительно якоря. |
| `Gap` | `float` | `4` | Зазор между якорем и панелью. |

## Стили

Слот панели — `Theme.PopoverPanel`; поддерживает бордюр/фон/радиус/отступы/тень через `Style`
самого `Popover`.

## Методы

Явного публичного конструктора с параметрами нет — только `new Popover { ... }` через
инициализатор полей.

| Метод | Возвращает | Описание |
|---|---|---|
| `SetStyle(string path, string value)` | — | Изменить стиль строкой (применяется к корневому `Style` — именованных частей у `Popover` нет). Опечатка в пути — молча игнорируется. |

## События

Собственного колбэка открытия/закрытия нет — состояние open/closed хранится в ID-store и
переключается кликами. Но, как и у любого элемента, доступны универсальные события:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы `Popover` (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по границам `Popover`. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У `Popover` нет своего disabled — не сработает. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `Theme.PopoverPanel`, бордюр/фон/радиус/тень. |
| Спрайт-рамка | Да | `ps.Sprite`, `SpriteBox`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Своего disabled-состояния нет (зависит от `Trigger`). |
| Hover-fade | Нет | Переключение по клику, не по наведению. |

Панель рендерится в overlay-слое, зажимается в границах окна, с авто-флипом стороны, если не
влезает.

## Точный алгоритм позиционирования и флипа

По кросс-оси (X для `Bottom`/`Top`, Y для `Left`/`Right`) панель всегда выровнена по НАЧАЛУ
якоря (`anchor.X`/`anchor.Y`), без центрирования. Флип на противоположную сторону — не
клампинг, а осознанная замена стороны, чтобы панель не оказалась поверх якоря (иначе клики по
якорю попадали бы в панель):

```
Bottom → Top,  если снизу не влезает (anchor.Bottom + gap + h > area.Bottom)
               И сверху уже влезает (anchor.Y - gap - h >= area.Y)
Right  → Left, если справа не влезает И слева уже влезает (симметрично)
```

Порог флипа — строгое неравенство, без запаса в пикселях: как только панель не помещается хотя
бы на условную единицу, происходит флип. Если не помещается НИ на одной из двух сторон — флип не
происходит вовсе, остаётся исходная сторона, и уже потом обычный клампинг поджимает панель к
границе окна (сначала по правому/нижнему краю, затем по левому/верхнему — если панель больше
доступной области, приоритет у выравнивания по левому/верхнему краю).


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Button](01_button_ru.md) | След.: [DropdownMenu](03_dropdownmenu_ru.md) | [English](02_popover_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
