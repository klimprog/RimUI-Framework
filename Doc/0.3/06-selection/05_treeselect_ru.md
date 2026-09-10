![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Выбор | Пред.: [SelectButton&lt;T&gt;](04_selectbutton_ru.md) | След.: [CascadeSelect](06_cascadeselect_ru.md) | [English](05_treeselect_en.md)

---

# TreeSelect

Поле-селект, но вместо плоского списка в выпадающей панели — дерево: раскрывайте ветки и
кликайте по нужному узлу, чтобы выбрать его.

`RimUI.Components.TreeSelect : UiElement`

## Пример

```csharp
var tsel = new TreeSelect { Placeholder = "Узел", Searchable = true };
tsel.TreePanel.Nodes.Add(new TreeItem("Оружие").Sub(new TreeItem("Дальнее").Sub(new TreeItem("Винтовка"))));
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `TreePanel` | `Tree` (readonly) | новый `Tree()` | Дерево вариантов (см. страницу «Tree»). |
| `OnChange` | `Action<TreeItem>` | `null` | Колбэк, передаёт весь выбранный узел (не только значение). |
| `Placeholder` | `string` | `"—"` | Текст, когда ничего не выбрано. |
| `Placement` | `OverlayPlacement` | `Bottom` | Сторона, с которой раскрывается панель: `Bottom`, `Top`, `Right`, `Left`, `Auto` (вниз, а при нехватке места вверх). |
| `Align` | `OverlayAlign` | `Start` | Выравнивание панели по второй оси: `Start`, `Center`, `End`, `Auto`. `End` прижимает панель правым краем к правому краю поля — она уходит влево. |
| `LeavesOnly` | `bool` | `true` | Выбирать можно только листья. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `FieldHeight` | `float` | `28` | Высота поля-триггера. |
| `PanelHeight` | `float` | `240` | Высота панели с деревом. |
| `Searchable` | `bool` | из `TreePanel` | Проксирует `TreePanel.Searchable`. |
| `CaseSensitive` | `bool` | из `TreePanel` | Проксирует `TreePanel.CaseSensitive`. |

Панель раскрывается вниз с флипом вверх при нехватке места — тот же алгоритм, что у `Select`
(см. соответствующую страницу). Поиск (`Searchable`) целиком переиспользует `Tree.CollectFiltered`
— точная механика (матчинг только по листьям, принудительное раскрытие ветвей-контекста без
изменения сохранённого состояния `Expanded`) описана на странице «Tree».


## Куда раскрывается панель

Направление задаётся двумя независимыми осями: `Placement` — с какой стороны панель встаёт у поля,
`Align` — как она выравнивается по другой оси. Пара покрывает все восемь сочетаний:

| Что нужно | Как задать |
|---|---|
| вниз, левые края вровень | `Placement = Bottom` (по умолчанию) |
| вверх | `Placement = Top` |
| **вверх и влево** | `Placement = Top`, `Align = End` |
| вниз, по центру поля | `Align = Center` |
| вбок (подменю) | `Placement = Right` либо `Left` |

`Auto` по каждой оси означает «выбрать саму»: сторона меняется на противоположную, если места не
хватает.

**Место считается по видимой области, а не только по окну.** Список внутри таблицы или прокрутки
разворачивается туда, где он виден. При этом зажимать панель границами ячейки нельзя — ей негде
было бы раскрыться, — поэтому за них она выходит свободно.

**Выравнивание видно только при разной ширине.** Если панель ровно шириной с поле, `Start`,
`Center` и `End` дают одну и ту же картинку.

## Стили

Общие слоты с `Select`-семейством (`select/panel`, `select/field*`) плюс слоты дерева `tree/*`.

## Методы

Явного публичного конструктора с параметрами нет — только беспараметрический `TreeSelect()`.

| Метод | Возвращает | Описание |
|---|---|---|
| `GetSelected()` | `TreeItem` | Текущий выбранный узел (до первого выбора — `null`, либо значение из `SetSelected`). |
| `SetSelected(TreeItem item)` | — | Программно выбрать узел (как клик по листу): пишет текст в поле и вызывает `OnChange`. |
| `SetStyle(string path, string value)` | — | Точечно задать стиль по пути темы; неизвестный путь или невалидное значение игнорируются. У `TreeSelect` нет именованных частей стиля — путь применяется к стилю самого элемента. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<TreeItem>` | выбранный узел целиком | Клик по узлу дерева (с учётом `LeavesOnly`). |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS: только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Выбор изменился (после выбора листа + `OnChange`). `SelectedIndex` всегда `-1`, `SelectedValue` — выбранный `TreeItem` целиком. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `select/*` + `tree/*`. |
| Спрайт-рамка | Да | Рамка поля и панель. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Панель не открывается. |
| Hover-fade | Нет (на поле-триггере) | Рамка поля переключается жёстко; строки дерева внутри — с плавным переходом. |


---

[Оглавление](../index_ru.md) - Выбор | Пред.: [SelectButton&lt;T&gt;](04_selectbutton_ru.md) | След.: [CascadeSelect](06_cascadeselect_ru.md) | [English](05_treeselect_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
