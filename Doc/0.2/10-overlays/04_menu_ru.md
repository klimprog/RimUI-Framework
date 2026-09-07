![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [DropdownMenu](03_dropdownmenu_ru.md) | След.: [ContextMenu](05_contextmenu_ru.md) | [English](04_menu_en.md)

---

# Menu

Меню бывает статичным — встроенным прямо в поток интерфейса, и всплывающим (overlay) у
триггера. Режим переключается флагом `Popup`.

`RimUI.Components.Menu : UiElement` (sealed)

## Пример

```csharp
var m = new Menu { Style = { Width = 220f } };   // статичный режим (Popup = false)
m.AddHeader("Файл");
m.AddItem(new MenuItem("Создать", () => { }));
m.AddSeparator();
m.AddItem(new MenuItem("Недоступно") { Disabled = true });
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Trigger` | `UiElement` | `null` | Якорь-кнопка (используется только при `Popup = true`). |
| `Popup` | `bool` | `false` | `false` — плоский список ПРЯМО В ПОТОКЕ интерфейса (не overlay); `true` — overlay-меню у триггера (внутри реюзается `DropdownMenu`). |
| `ItemHeight` | `float` | `26` | Высота пункта. |
| `HeaderHeight` | `float` | `20` | Высота заголовка группы. |
| `SeparatorHeight` | `float` | `9` | Высота разделителя. |
| `MinWidth` | `float` | `160` | Минимальная ширина. |

Использует тот же `MenuItem`, что и `DropdownMenu` (см. страницу «DropdownMenu»).

## Стили

Слот — `Theme.MenuPanel` (тот же, что у `DropdownMenu`).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `AddItem(MenuItem item)` | `Menu` | Добавить пункт, возвращает себя. |
| `AddHeader(string text)` | `Menu` | Добавить заголовок группы. |
| `AddSeparator()` | `Menu` | Добавить разделитель. |
| `SetStyle(string path, string value)` | — | Изменить стиль строкой (применяется к корневому `Style` — именованных частей у `Menu` нет). Опечатка в пути — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `MenuItem.OnClick` | `Action` | — | Клик по пункту без подменю. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы `Menu` (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по границам `Menu` (не по отдельным пунктам — для них используйте `MenuItem.OnClick`). |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У самого `Menu` нет своего disabled — не сработает (см. `MenuItem.Disabled` на уровне пункта). |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `Theme.MenuPanel`. |
| Спрайт-рамка | Да | Панель рисуется через `DrawCommand.Background`, поддерживает как обычный элемент. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | На уровне пункта (`MenuItem.Disabled`). |
| Hover-fade | Нет (в статичном режиме) | Подсветка строки в статичном режиме мгновенная (`if (hover) list.Add(...)`), без `HoverFade` — в отличие от `DropdownMenu`. |


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [DropdownMenu](03_dropdownmenu_ru.md) | След.: [ContextMenu](05_contextmenu_ru.md) | [English](04_menu_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
