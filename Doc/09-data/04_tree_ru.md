![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — ядро `0.6.7` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Данные | Пред.: [PickList&lt;T&gt;](03_picklist_ru.md) | След.: [DataTable](05_datatable_ru.md) | [English](04_tree_en.md)

---

# Tree

Дерево с раскрывающимися ветками: клик раскрывает/сворачивает ветвь, опционально — мультивыбор
строк с поиском по листьям и/или чекбоксы, каскадно распространяющиеся на дочерние узлы.

`RimUI.Components.Tree : UiElement`

## Пример

```csharp
var tr = new Tree { MultiSelect = true, Searchable = true, Style = { Height = 200f } };
tr.Nodes.Add(new TreeItem("База")
    .Sub(new TreeItem("Склад")
        .Sub(new TreeItem("Еда") { Icon = Icons.Dot })
        .Sub(new TreeItem("Лекарства"))));
tr.OnSelect = n => { /* выбранный узел */ };

// вариант с чекбоксами (тристейт)
var trc = new Tree { Checkable = true };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Nodes` | `List<TreeItem>` (readonly) | пусто | Корневые узлы. |
| `Checkable` | `bool` | `false` | Включает тристейт-чекбоксы (ничего / часть «−» / всё). |
| `MultiSelect` | `bool` | `false` | Множественный выбор строк. |
| `RowHeight` | `float` | `24` | Высота строки. |
| `Indent` | `float` | `18` | Отступ уровня вложенности. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `Searchable` | `bool` | `false` | Поле поиска (ищет только по листьям; совпавшие показываются вместе с родителями-контекстом, ветви форсированно раскрываются). |
| `CaseSensitive` | `bool` | `false` | Регистрозависимый поиск. |
| `SearchPlaceholder` | `string` | `"Поиск..."` | Подсказка в поле поиска. |

`TreeItem(string text, object value = null)`: `string Key`, `Text`, `int Icon = -1`, `object
Value`, `List<TreeItem> Children`; метод `Sub(TreeItem child)` — fluent, добавляет дочерний
узел; свойство `bool HasChildren`.

### Точная механика поиска

Матчатся ТОЛЬКО листья (`!n.HasChildren`) — текст узла-ветви никогда не сравнивается с фильтром
напрямую. Ветвь остаётся в списке только если хотя бы один лист в её поддереве совпал; иначе
убирается целиком. При активном поиске состояние раскрытия (`Expanded`) не меняется — оно лишь
временно игнорируется визуально (все показанные ветви рисуются раскрытыми), а не перезаписывается
навсегда: очистка поля поиска возвращает дерево ровно к тому раскрытию, что было до/во время
поиска, без авто-сворачивания или авто-разворачивания. Поиск — подстрока (`LIKE %filter%`, не
`StartsWith`), по умолчанию без учёта регистра.

## Стили

Слоты: `tree/frame`, `tree/row`, `tree/row_hover`, `tree/row_selected`, `tree/scrollbar`,
`tree/chevron`, `tree/check`, `tree/check_on`, `tree/check_mark`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `FocusSearch()` | `void` | Сфокусировать поле поиска на следующем кадре (используется, например, `TreeSelect`). |
| `St(UiState s)` | `TreeState` | Доступ к внутреннему состоянию (`Expanded`, `Checked`, `SelectedKeys`, `Scroll`, `Filter`). |
| `GetSelected()` | `TreeItem` | Последний выбранный узел (single) либо последний тоггл-нутый (`MultiSelect`); до первого выбора — `null`. Только чтение — отдельного `SetSelected` нет (мутация требует стабильного ключа узла, а не только объекта). |
| `SetStyle(string path, string value)` | `void` | Установить поле стиля строкой; именованных частей у `Tree` нет — путь применяется к корневому `Style`. Неизвестный путь/значение — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnSelect` | `Action<TreeItem>` | выбранный узел | Клик по строке. |
| `OnSelectKeyed` | `Action<TreeItem, string>` | узел + стабильный ключ | Выбор узла (используется, например, `MultiTreeSelect`). |
| `OnToggle` | `Action<TreeItem, bool>` | узел, новое состояние раскрытия | Раскрытие/сворачивание ветви. |
| `OnCheck` | `Action<TreeItem, bool>` | узел, новое значение | Клик по чекбоксу (только `Checkable`); каскадно применяется ко всему поддереву. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР (~60 раз/сек). Тяжёлый обработчик просадит FPS — только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex` (всегда `-1`), `data.SelectedValue` (выбранный `TreeItem`) | Клик по строке (выбор листа или ветви) — после `OnSelect`/`OnSelectKeyed`. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `tree/*`. |
| Спрайт-рамка | Да | `tree/frame`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Блокирует клики/чекбоксы. |
| Hover-fade | Да | Id по стабильному ключу узла — устойчив к сортировке/скроллу. |


---

[Оглавление](../index_ru.md) - Данные | Пред.: [PickList&lt;T&gt;](03_picklist_ru.md) | След.: [DataTable](05_datatable_ru.md) | [English](04_tree_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
