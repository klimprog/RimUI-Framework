![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.3.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Menu](04_menu_ru.md) | След.: [TooltipBox](06_tooltipbox_ru.md) | [English](05_contextmenu_en.md)

---

# ContextMenu

Контекстное меню по правому клику на произвольном элементе.

`RimUI.Components.ContextMenu : UiElement` (sealed)

## Пример

```csharp
var target = new Field();
target.Add(new Text("Кликни ПРАВОЙ кнопкой сюда"));
var cm = new ContextMenu(target);
cm.AddItem(new MenuItem("Действие", () => { }));
cm.AddItem(new MenuItem("Подменю").Sub(new MenuItem("Вложенное", () => { })));
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Target` | `UiElement` | из конструктора | Элемент, по ПКМ на котором открывается меню. |
| `Panel` | `DropdownMenu` (readonly) | новый `DropdownMenu()` | Сама панель пунктов (полноценное tiered-меню). |

## Стили

Наследует стили `DropdownMenu` (`Panel`) — см. страницу «DropdownMenu».

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ContextMenu(UiElement target = null)` (конструктор) | — | Создать меню для элемента-цели. |
| `AddItem(MenuItem item)` | `ContextMenu` | Сахар над `Panel.AddItem`, возвращает себя. |
| `SetStyle(string path, string value)` | — | Изменить стиль строкой (применяется к корневому `Style` самого `ContextMenu` — не к `Panel`; у `ContextMenu` именованных частей нет). Опечатка в пути — молча игнорируется. |

Механика: по ПКМ на `Target` (точнее — по границам самого `ContextMenu`, которые совпадают с
`Target` после `Arrange`) вызывается `Panel.OpenAt(s, s.MousePosition)` — панель открывается
у курсора.

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `MenuItem.OnClick` | `Action` | — | Клик по пункту меню (через `Panel`). |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы `ContextMenu` (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` | `UiEventHandler` | `data.MousePosition` | ЛКМ по границам `ContextMenu`. |
| `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ПКМ по границам `ContextMenu` — **важно**: срабатывает даже когда к элементу подвешено контекстное меню, и вызывается ПОСЛЕ собственной логики `ContextMenu` (после `Panel.OpenAt(...)`, которая уже открыла панель у курсора за этот же клик). Универсальные события — это наблюдатели поверх готовой логики компонента, а не замена ей: оба сработают на один и тот же ПКМ. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У самого `ContextMenu` нет своего disabled — не сработает (см. `MenuItem.Disabled` на уровне пункта панели). |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Унаследовано от внутреннего `DropdownMenu` (`Panel`). |
| Спрайт-рамка | Да | Как у `DropdownMenu`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | На уровне пункта (`MenuItem.Disabled`). |
| Hover-fade | Да | Как у `DropdownMenu` — на строку пункта. |


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Menu](04_menu_ru.md) | След.: [TooltipBox](06_tooltipbox_ru.md) | [English](05_contextmenu_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
