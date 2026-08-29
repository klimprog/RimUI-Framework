![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Данные | Пред.: [OrderList&lt;T&gt;](02_orderlist_ru.md) | След.: [Tree](04_tree_ru.md) | [English](03_picklist_en.md)

---

# PickList&lt;T&gt;

Перенос элементов между двумя списками: кнопками между колонками или двойным кликом по
элементу; Ctrl/Shift позволяют выделить сразу несколько элементов и перенести их одним
действием.

`RimUI.Components.PickList<T> : UiElement` (generic)

## Пример

```csharp
var pick = new PickList<string>(null) { Style = { Height = 200f } };
pick.Source.Items.AddRange(new[] { "Винтовка", "Пистолет", "Дубина" });
pick.Target.Items.Add("Нож");
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Source` | `ListBox<T>` (readonly) | новый `ListBox<T>()` | Список «доступные». |
| `Target` | `ListBox<T>` (readonly) | новый `ListBox<T>()` | Список «выбранные». |
| `OnChange` | `Action<List<T>, List<T>>` | `null` | Вызывается после каждого переноса, передаёт `(Source.Items, Target.Items)`. |
| `SourceTitle` | `string` | `"Доступные"` | Заголовок левой колонки. |
| `TargetTitle` | `string` | `"Выбранные"` | Заголовок правой колонки. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `ButtonsWidth` | `float` | `26` | Ширина колонки кнопок. |
| `TitleHeight` | `float` | `20` | Высота заголовков колонок. |
| `Rows` | `int` | `6` | Фиксированная высота списков в строках (иначе пустой список схлопывается); `Style.Height` компонента приоритетнее. |

## Стили

Внутренние `ListBox` (слоты `list/*`) + слот `list/title` для заголовков колонок + `list/button*`
для кнопок.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `PickList(Action<List<T>, List<T>> onChange = null)` (конструктор) | — | Создать перенос с колбэком. |
| `SetStyle(string path, string value)` | `void` | Установить поле стиля строкой; именованных частей у `PickList` нет — путь применяется к корневому `Style`. Неизвестный путь/значение — молча игнорируется. |

Двойной клик по строке в `Source`/`Target` тоже переносит элемент (назначен внутри конструктора
через `OnActivate`). Кнопки → ⇉ ⇇ ← переносят выбранное/всё. **Перетаскивания мышью между
списками нет** — как и у `OrderList`, весь перенос идёт только кнопками/двойным кликом.

У самого `PickList` нет своих `GetSelected`/`SetSelected` — выбор строки живёт в двух внутренних
`ListBox`, доступных через свойства `Source`/`Target`: `pickList.Source.GetSelected()` /
`pickList.Source.SetSelected(index)`, аналогично для `Target`.

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<List<T>, List<T>>` | `(Source.Items, Target.Items)` | После каждого переноса элемента. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР (~60 раз/сек). Тяжёлый обработчик просадит FPS — только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

`SelectionChanged` сам `PickList` не вызывает (`FireSelection` в его коде нет). Чтобы слушать
выбор строки, подписывайтесь на `Events.SelectionChanged` внутренних `Source`/`Target`.

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Внутренние `ListBox` + `list/title` + `list/button*`. |
| Спрайт-рамка | Да | Аналогично `OrderList`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Прокидывается в оба списка и кнопки. |
| Hover-fade | Нет | Обычный `HoverOn`, без плавного перехода. |


---

[Оглавление](../index_ru.md) - Данные | Пред.: [OrderList&lt;T&gt;](02_orderlist_ru.md) | След.: [Tree](04_tree_ru.md) | [English](03_picklist_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
