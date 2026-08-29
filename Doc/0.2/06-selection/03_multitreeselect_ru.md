![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Выбор | Пред.: [MultiSelect&lt;T&gt;](02_multiselect_ru.md) | След.: [SelectButton&lt;T&gt;](04_selectbutton_ru.md) | [English](03_multitreeselect_en.md)

---

# MultiTreeSelect

То же самое, что `MultiSelect`, но источник вариантов — дерево, а выбирать можно только листья:
клик по листу переключает его выбор, выбранные показаны чипами в поле; есть поиск по названиям
листьев.

`RimUI.Components.MultiTreeSelect : UiElement`

## Пример

```csharp
var mts = new MultiTreeSelect { Placeholder = "Листья", Searchable = true };
mts.TreePanel.Nodes.Add(new TreeItem("Оружие")
    .Sub(new TreeItem("Дальнее").Sub(new TreeItem("Винтовка")).Sub(new TreeItem("Лук")))
    .Sub(new TreeItem("Ближнее").Sub(new TreeItem("Меч"))));
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `TreePanel` | `Tree` (readonly) | новый `Tree()` | Дерево вариантов; узлы добавляются через `TreePanel.Nodes` (см. страницу «Tree»). |
| `OnChange` | `Action<IList<TreeItem>>` | `null` | Колбэк, получает всю коллекцию выбранных узлов. |
| `Placeholder` | `string` | `"—"` | Текст, когда ничего не выбрано. |
| `LeavesOnly` | `bool` | `true` | Выбирать можно только листья; ветвь — только раскрытие. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `MaxSelected` | `int` | `0` (без лимита) | Максимум выбранных листьев. |
| `FieldHeight` | `float` | `28` | Высота поля-триггера. |
| `PanelHeight` | `float` | `240` | Высота панели с деревом. |
| `Searchable` | `bool` | из `TreePanel` | Поле поиска (проксирует `TreePanel.Searchable`). |
| `CaseSensitive` | `bool` | из `TreePanel` | Регистрозависимый поиск (проксирует `TreePanel.CaseSensitive`). |

Панель раскрывается вниз с флипом вверх при нехватке места — тот же алгоритм, что у `Select`.
Поиск целиком переиспользует `Tree.CollectFiltered` (матчинг только по листьям) — точная
механика описана на странице «Tree».

## Стили

Общие слоты с `Select`-семейством (`select/panel`, `select/field*`, `chip/*`) плюс слоты дерева
`tree/*` (см. страницу «Tree»).

## Методы

Явного публичного конструктора с параметрами нет — только беспараметрический `MultiTreeSelect()`.

| Метод | Возвращает | Описание |
|---|---|---|
| `GetSelected()` | `IList<TreeItem>` | Текущий список выбранных узлов (порядок выбора; до первого показа — пусто). Отдельного `SetSelected` нет — набор мутируется кликом по листу/крестику чипа. |
| `SetStyle(string path, string value)` | — | Точечно задать стиль по пути темы; неизвестный путь или невалидное значение игнорируются. У `MultiTreeSelect` нет именованных частей стиля — путь применяется к стилю самого элемента. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<IList<TreeItem>>` | вся коллекция выбранных узлов | Клик по листу дерева. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS: только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Выбор изменился (после добавления/снятия листа + `OnChange`). `SelectedIndex` всегда `-1`, `SelectedValue` — весь список выбранных узлов (`IList<TreeItem>`) после изменения. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `select/*` + `tree/*` + `chip/*`. |
| Спрайт-рамка | Да | Рамка поля и панель. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Панель не открывается. |
| Hover-fade | Нет (на поле-триггере) | Рамка поля переключается жёстко; строки дерева внутри панели — с плавным переходом (см. «Tree»). |


---

[Оглавление](../index_ru.md) - Выбор | Пред.: [MultiSelect&lt;T&gt;](02_multiselect_ru.md) | След.: [SelectButton&lt;T&gt;](04_selectbutton_ru.md) | [English](03_multitreeselect_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
