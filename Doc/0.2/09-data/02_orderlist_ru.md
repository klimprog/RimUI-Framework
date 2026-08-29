![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Данные | Пред.: [ListBox&lt;T&gt;](01_listbox_ru.md) | След.: [PickList&lt;T&gt;](03_picklist_ru.md) | [English](02_orderlist_en.md)

---

# OrderList&lt;T&gt;

Список с изменяемым порядком строк: переставляйте их четырьмя кнопками (в начало / вверх на одну
позицию / вниз на одну позицию / в конец). **Перетаскивания мышью нет** — несмотря на
интуитивное ожидание «списка с drag&drop», перестановка реализована исключительно кнопками.

`RimUI.Components.OrderList<T> : UiElement` (generic)

## Пример

```csharp
var ord = new OrderList<string>(null) { Style = { Height = 190f } };
ord.Items.AddRange(new[] { "Готовка", "Стройка", "Растения" });
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `List` | `ListBox<T>` (readonly) | новый `ListBox<T>()` | Внутренний список (см. страницу «ListBox»). |
| `Items` | `List<T>` (свойство `=> List.Items`) | — | Элементы списка. |
| `OnReorder` | `Action<List<T>>` | `null` | Вызывается после каждого перемещения, передаёт актуальный список. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `ButtonsWidth` | `float` | `26` | Ширина колонки кнопок. |
| `Rows` | `int` | `0` (по контенту/`Style.Height`) | Фиксированная высота списка в строках. |

## Стили

Стиль применяется к внутреннему `ListBox` + кнопкам (внутренний хелпер `ListButton`, слоты
`list/button*`).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `OrderList(Action<List<T>> onReorder = null)` (конструктор) | — | Создать список с колбэком перестановки. |
| `SetStyle(string path, string value)` | `void` | Установить поле стиля строкой; именованных частей у `OrderList` нет — путь применяется к корневому `Style`. Неизвестный путь/значение — молча игнорируется. |

Кнопки ⇈ (в начало) / ↑ (на одну позицию вверх) / ↓ (на одну позицию вниз) / ⇊ (в конец)
переставляют ВЫБРАННЫЙ элемент, мутируя `Items` на месте; работают обычным кликом, без порога
перетаскивания — драга в компоненте нет вовсе.

У самого `OrderList` нет своих `GetSelected`/`SetSelected` — выбор строки живёт во внутреннем
`ListBox`, доступном через свойство `List`: `orderList.List.GetSelected()` /
`orderList.List.SetSelected(index)`.

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnReorder` | `Action<List<T>>` | актуальный список | После каждого перемещения элемента. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР (~60 раз/сек). Тяжёлый обработчик просадит FPS — только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

`SelectionChanged` сам `OrderList` не вызывает (`FireSelection` в его коде нет). Чтобы слушать
выбор строки, подписывайтесь на `orderList.List.Events.SelectionChanged` у внутреннего `ListBox`.

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Через внутренний `ListBox` + слоты `list/button*`. |
| Спрайт-рамка | Да | Через слот `list/button`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Прокидывается в `List.Disabled` и в кнопки. |
| Hover-fade | Нет | Кнопки и строки — обычный `HoverOn`, без плавного перехода. |


---

[Оглавление](../index_ru.md) - Данные | Пред.: [ListBox&lt;T&gt;](01_listbox_ru.md) | След.: [PickList&lt;T&gt;](03_picklist_ru.md) | [English](02_orderlist_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
