![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Данные | Пред.: [Tree](04_tree_ru.md) | След.: [TreeTable](06_treetable_ru.md) | [English](05_datatable_en.md)

---

# DataTable

Таблица: чередующаяся заливка строк (зебра), подсветка при наведении, мультивыбор строк,
сортировка кликом по заголовку колонки, фильтры по колонкам (совмещаются по И), а в ячейках —
живые элементы интерфейса, а не просто текст.

`RimUI.Components.DataTable : UiElement`

## Пример

```csharp
var lvls = new List<int>();
var table = new DataTable(
    new TableColumn("Колонист", 0f, 2f) { FilterBy = r => names[r] },
    new TableColumn("Навык", 0f, 1.4f) { FilterBy = r => skills[r % skills.Length] },
    new TableColumn("Уровень", 46f) { SortBy = r => lvls[r] },
    new TableColumn("Прогресс", 0f, 1.6f),
    new TableColumn("Вкл", 50f))
{ MultiSelect = true, GridLines = true, Style = { Height = 240f } };

for (int i = 0; i < names.Length; i++)
{
    int lvl = Rand(1, 15); lvls.Add(lvl);
    table.AddRow(
        Cell(names[i]), Cell(skills[i % skills.Length]), Cell(lvl.ToString()),
        new ProgressBar(() => progress[i]) { BarHeight = 12f, ShowText = false },
        new Checkbox(null) { Key = "chk" + i, BoxSize = 16f });
}
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Columns` | `List<TableColumn>` (readonly) | из конструктора | Колонки таблицы. |
| `Rows` | `List<List<UiElement>>` (readonly) | пусто | Строки: каждая строка — список ячеек-элементов. |
| `Selectable` | `bool` | `true` | Разрешён ли выбор строк. |
| `MultiSelect` | `bool` | `false` | Множественный выбор. |
| `Zebra` | `bool` | `true` | Чередование фона строк. |
| `GridLines` | `bool` | `false` | Линии сетки между ячейками. |
| `RowHeight` | `float` | `28` | Высота строки. |
| `HeaderHeight` | `float` | `28` | Высота заголовка. |
| `CaseSensitive` | `bool` | `false` | Регистрозависимый поиск по фильтрам. |
| `MinWeightColumnWidth` | `float` | `90` | Минимальная ширина weight-колонки; не влезают — включается горизонтальный скролл. |

`TableColumn(string title, float width = 0f, float weight = 1f)`: `width > 0` — фиксированная
ширина в px; `width == 0` — доля остатка по `weight` (аналог `flex-grow` для колонок). Поля:
`Func<int, IComparable> SortBy` (ключ сортировки по индексу строки, `null` = не сортируется),
`Func<int, string> FilterBy` (текст для поля-фильтра под заголовком, `null` = без поля поиска).

## Стили

Слоты: `table/frame`, `header`, `header_text`, `row_odd`, `row_hover`, `row_selected`,
`gridline`, `scrollbar`, `sort_arrow`, `hscroll_track`, `hscroll_handle(+_active)`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `DataTable(params TableColumn[] columns)` (конструктор) | — | Создать таблицу с колонками. |
| `Bind<T>(IEnumerable<T> data, Func<T, UiElement[]> rowBuilder)` | `DataTable` | Перестроить `Rows` из данных (вызывать при изменении данных, НЕ на каждый кадр). |
| `AddRow(params UiElement[] cells)` | `DataTable` | Добавить строку, возвращает себя (цепочка). |
| `St(UiState s)` | `TableState` | Доступ к состоянию (`Selected`, `Scroll`, `SortCol`, `Order`, `Filters`...). |
| `GetSelected()` | `HashSet<int>` | Набор выбранных строк по исходному индексу (`Selectable`/`MultiSelect`); до первого показа — пусто. Только чтение — отдельного `SetSelected` нет. |
| `SetStyle(string path, string value)` | `void` | Установить поле стиля строкой; именованных частей у `DataTable` нет — путь применяется к корневому `Style`. Неизвестный путь/значение — молча игнорируется. |

Клик по заголовку сортируемой колонки циклит: возрастание → убывание → без сортировки.
Несколько заполненных фильтров объединяются по И (проверка идёт по цепочке и останавливается на
первом несовпадении — цикл, а не сразу все сразу). Клики/выбор стабильны по исходному индексу
строки даже после сортировки.

### Точная механика ширины weight-колонок

Для колонок с `Width = 0` итоговая ширина считается так:
```
contentW = max(доступная_ширина, fixedSum + weightCount × MinWeightColumnWidth)
rest     = contentW - fixedSum
colW[i]  = rest × (Weight[i] / weightSum)
```
где `fixedSum` — сумма ширин фиксированных колонок, `weightCount`/`weightSum` — число и сумма
весов weight-колонок. **Важная ловушка**: `MinWeightColumnWidth` (90px по умолчанию) гарантирует
минимум только для СУММЫ весовых колонок вместе, а не для каждой по отдельности — при сильно
разных весах колонка с маленьким `Weight` может получить итоговую ширину заметно МЕНЬШЕ 90px
(например, при весах 1 и 3 и `rest = 180`: колонки получат 45px и 135px соответственно).
Горизонтальный скролл включается строго при `contentW > видимая_ширина + 0.5` (эпсилон на
погрешность округления).

### Точная механика сортировки

Сортировка реализована вручную как **стабильная** (при равенстве ключей строки остаются в
исходном относительном порядке — компаратор в этом случае сравнивает индексы) — обычный
`List.Sort` в .NET сам по себе стабильность не гарантирует, поэтому это сознательное решение, а
не побочный эффект. `null`-ключи трактуются как «меньше» любого не-`null` значения; при
возрастании строки с `null` уходят в начало списка. Несовместимые между собой типы `IComparable`
не отлавливаются — сравнение упадёт исключением, если `SortBy` для разных строк вернёт значения,
которые нельзя сравнить друг с другом.

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnRowClick` | `Action<int>` | индекс исходной строки | Клик по строке. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР (~60 раз/сек). Тяжёлый обработчик просадит FPS — только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность (у `DataTable` своего `Disabled` нет — сработает, только если состояние меняется на уровне базового `UiElement`). |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex` (индекс исходной строки), `data.SelectedValue` (всегда `null`) | Клик по строке — после собственной логики выбора и `OnRowClick`. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `table/*`. |
| Спрайт-рамка | Да | `table/frame`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Своего понятия «отключена» нет. |
| Hover-fade | Да | Id по исходному индексу строки — устойчив к сортировке. |


---

[Оглавление](../index_ru.md) - Данные | Пред.: [Tree](04_tree_ru.md) | След.: [TreeTable](06_treetable_ru.md) | [English](05_datatable_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
