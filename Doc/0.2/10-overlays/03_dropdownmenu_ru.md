![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Popover](02_popover_ru.md) | След.: [Menu](04_menu_ru.md) | [English](03_dropdownmenu_en.md)

---

# DropdownMenu

Выпадающее меню у кнопки: клик по кнопке открывает список пунктов, среди которых могут быть
отключённые и пункты с вложенными подменю, раскрывающимися при наведении.

`RimUI.Elements.DropdownMenu : UiElement` (sealed)

## Пример

```csharp
var menu = new DropdownMenu { Trigger = Button.Make("Меню") };
menu.AddItem(new MenuItem("Создать", () => { }));
menu.AddItem(new MenuItem("Недоступно") { Disabled = true });
menu.AddItem(new MenuItem("Экспорт")
    .Sub(new MenuItem("В PNG", () => { }))
    .Sub(new MenuItem("В JSON", () => { })));
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Trigger` | `UiElement` | `null` | Якорь-кнопка, открывающая меню по клику. |
| `Items` | `List<MenuItem>` (readonly) | пусто | Пункты меню верхнего уровня. |
| `ItemTextStyle` | `TextStyle` | `null` (из темы) | Стиль текста пунктов. |
| `ItemHoverColor` | `ColorRGBA?` | `null` | Цвет подсветки пункта при наведении. |
| `DisabledTextColor` | `ColorRGBA?` | `null` | Цвет текста отключённого пункта. |
| `ItemHeight` | `float` | `0` (из темы) | Высота пункта. |
| `IconSlot` | `float` | `0` (из темы) | Ширина слота под иконку (если хоть у одного пункта есть иконка). |
| `MinWidth` | `float` | `0` (из темы) | Минимальная ширина панели. |

`MenuItem(string text, Action onClick = null, string icon = null)`: `string Text`, `string
Icon`, `Action OnClick`, `bool Disabled`, `List<MenuItem> Items` (подменю); метод `Sub(MenuItem
child)` — добавить подпункт, возвращает РОДИТЕЛЯ (fluent); свойство `bool HasChildren`.

### Точная механика позиционирования

Панель первого уровня открывается стороной `Bottom` с тем же алгоритмом флипа, что у `Popover`
(см. соответствующую страницу), с зазором 4px от триггера. Вложенное подменю раскрывается
стороной `Right` от строки родительского пункта (не от всей панели — якорь именно строки), с
зазором **0px** (примыкает вплотную); может флипнуться в `Left`, если справа по краю окна не
хватает места, а слева достаточно — по той же логике флипа. По вертикали подменю стартует на
уровне верхнего края родительской строки (а не по центру панели), раскрывается по наведению
(hover), а не по клику.

Ширина панели считается по самому широкому пункту (слот под иконку + текст + место под
шеврон вложенности), но не меньше `MinWidth` (0 = из темы, `Theme.MenuMinWidth = 140`). Высота —
сумма высот всех пунктов плюс `Gap` между ними плюс внутренние отступы панели.

## Стили

Слот панели — `Theme.MenuPanel`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `AddItem(MenuItem item)` | `DropdownMenu` | Добавить пункт, возвращает себя. |
| `OpenAt(UiState s, Vec2 pos)` | `void` | Открыть панель у произвольной точки (используется `ContextMenu`), `Trigger` не обязателен. |
| `SetStyle(string path, string value)` | — | Изменить стиль строкой (применяется к корневому `Style` — именованных частей у `DropdownMenu` нет). Опечатка в пути — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `MenuItem.OnClick` | `Action` | — | Клик по пункту без подменю (у пункта с `HasChildren` клик раскрывает подменю вместо вызова). |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы `DropdownMenu` (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по границам `DropdownMenu` (не по отдельным пунктам — для них используйте `MenuItem.OnClick`). |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У самого `DropdownMenu` нет своего disabled — не сработает (см. `MenuItem.Disabled` на уровне пункта). |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `Theme.MenuPanel`. |
| Спрайт-рамка | Да | Панель рисуется через `DrawCommand.Background`, поддерживает спрайт-стиль как обычный элемент. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | На уровне пункта (`MenuItem.Disabled`) — серый текст, клик не срабатывает. |
| Hover-fade | Да | На строку пункта (буфер по `depth*64+i`). |


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Popover](02_popover_ru.md) | След.: [Menu](04_menu_ru.md) | [English](03_dropdownmenu_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
