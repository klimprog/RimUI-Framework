![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Данные | Пред.: [DataTable](05_datatable_ru.md) | След.: [ScrollBox](07_scrollbox_ru.md) | [English](06_treetable_en.md)

---

# TreeTable

Таблица, где первая колонка — раскрывающееся дерево, а остальные — обычные ячейки данных.
Сортировка переставляет только соседние строки внутри одного родителя (структура дерева
сохраняется); фильтр ищет только среди листьев.

`RimUI.Components.TreeTable : UiElement`

## Пример

```csharp
TreeTableItem Node(string text, int qty, int w, int price) =>
    new TreeTableItem(text, Cell(qty.ToString()), Cell(w.ToString()), Cell(price.ToString())) { Value = qty };

var tt = new TreeTable(
    new TableColumn("Категория", 0f, 2f) { SortNode = n => n.Text, FilterNode = n => n.Text },
    new TableColumn("Кол-во", 80f) { SortNode = n => (int)n.Value },
    new TableColumn("Вес", 80f),
    new TableColumn("Цена", 80f))
{ GridLines = true, Style = { Height = 250f } };

tt.Nodes.Add(Node("Ресурсы", 245, 310, 1200)
    .Sub(Node("Сталь", 120, 180, 720))
    .Sub(Node("Дерево", 95, 110, 380)));
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Columns` | `List<TableColumn>` (readonly) | из конструктора | Колонки; ПЕРВАЯ всегда колонка дерева. |
| `Nodes` | `List<TreeTableItem>` (readonly) | пусто | Корневые узлы. |
| `Zebra` | `bool` | `true` | Чередование фона строк. |
| `GridLines` | `bool` | `false` | Линии сетки. |
| `RowHeight` | `float` | `26` | Высота строки. |
| `HeaderHeight` | `float` | `28` | Высота заголовка. |
| `Indent` | `float` | `16` | Отступ уровня вложенности. |
| `CaseSensitive` | `bool` | `false` | Регистрозависимый поиск по фильтрам. |

`TreeTable` использует тот же `TableColumn`, что и `DataTable`, но через `SortNode`/`FilterNode`
(вместо `SortBy`/`FilterBy`): `Func<TreeTableItem, IComparable> SortNode`, `Func<TreeTableItem,
string> FilterNode`.

`TreeTableItem(string text, params UiElement[] cells)`: `string Key`, `Text`, `int Icon = -1`,
`UiElement[] Cells` (остальные колонки), `List<TreeTableItem> Children`, `object Value`; метод
`Sub(TreeTableItem child)` — fluent.

Выбор всегда одиночный (нет `MultiSelect`), горизонтального скролла нет (в отличие от
`DataTable`).

### Точная механика сортировки «внутри родителя»

Сортировка применяется рекурсивно на КАЖДОМ уровне дерева независимо: при раскрытии ветки для её
списка детей заново вызывается та же сортировка (по глобально активной колонке/направлению —
они одни на весь `TreeTable`, а не свои у каждого узла). Дети одного родителя переставляются
между собой, но никогда не смешиваются с детьми другого родителя — структура дерева (кто чей
ребёнок) не меняется, меняется только порядок внутри списка `Children`. В отличие от `DataTable`,
где отсортированный порядок кэшируется и пересобирается только по событию, у `TreeTable` порядок
пересчитывается заново каждый кадр внутри `Measure` — своего кеша нет.

## Стили

Общее семейство слотов `table/*` (см. страницу «DataTable»), плюс `table/chevron` для узлов
дерева.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `TreeTable(params TableColumn[] columns)` (конструктор) | — | Создать таблицу-дерево; первая колонка — дерево. |
| `St(UiState s)` | `TreeTableState` | Доступ к состоянию (`Expanded`, `SelectedKey`, `Scroll`, `SortCol`, `Filters`). |
| `GetSelected()` | `TreeTableItem` | Последний выбранный узел; до первого выбора — `null`. Только чтение — отдельного `SetSelected` нет. |
| `SetStyle(string path, string value)` | `void` | Установить поле стиля строкой; именованных частей у `TreeTable` нет — путь применяется к корневому `Style`. Неизвестный путь/значение — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnSelect` | `Action<TreeTableItem>` | выбранный узел | Клик по строке (в т.ч. клик по ветви — выбор и раскрытие одновременно). |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР (~60 раз/сек). Тяжёлый обработчик просадит FPS — только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность (у `TreeTable` своего `Disabled` нет). |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex` (всегда `-1`), `data.SelectedValue` (выбранный `TreeTableItem`) | Клик по строке — после собственной логики выбора и `OnSelect`. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `table/*` + `tree/row` для колонки-дерева. |
| Спрайт-рамка | Да | `table/frame`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Своего понятия «отключена» нет. |
| Hover-fade | Да | Id по стабильному ключу узла. |


---

[Оглавление](../index_ru.md) - Данные | Пред.: [DataTable](05_datatable_ru.md) | След.: [ScrollBox](07_scrollbox_ru.md) | [English](06_treetable_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
