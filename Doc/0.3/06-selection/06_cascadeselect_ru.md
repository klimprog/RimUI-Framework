![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Выбор | Пред.: [TreeSelect](05_treeselect_ru.md) | След.: [ColorPicker](07_colorpicker_ru.md) | [English](06_cascadeselect_en.md)

---

# CascadeSelect

Каскадный выбор через вложенные подменю: наведение на пункт с дочерними вариантами раскрывает
следующий уровень сбоку, клик по конечному пункту завершает выбор. Реализован поверх
`DropdownMenu`.

`RimUI.Components.CascadeSelect : UiElement`

## Пример

```csharp
var cas = new CascadeSelect(null) { Placeholder = "Регион" };
cas.Items.Add(new CascadeItem("Умеренный").Sub(new CascadeItem("Лес")).Sub(new CascadeItem("Холмы")));
cas.Items.Add(new CascadeItem("Суровый").Sub(new CascadeItem("Тундра")));
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Items` | `List<CascadeItem>` | пусто | Корневые пункты каскада. |
| `OnChange` | `Action<object>` | `null` | Колбэк, передаёт `CascadeItem.Value` выбранного листа. |
| `Placeholder` | `string` | `"—"` | Текст, когда ничего не выбрано. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `FieldHeight` | `float` | `28` | Высота поля-триггера. |
| `Clearable` | `bool` | `false` | Добавить первым пунктом «не выбрано», сбрасывающий выбор. |
| `ClearText` | `string` | `"— не выбрано —"` | Текст пункта очистки. |
| `SelectBranches` | `bool` | `false` | Разрешить выбирать **ветку**, а не только лист. Ветка остаётся раскрываемой: подменю раскрывается по наведению, а клик по самой ветке выбирает её. |
| `MultiSelect` | `bool` | `false` | Множественный выбор: клик добавляет или убирает пункт из набора, меню не закрывается, выбранные помечаются галочкой. |
| `MultiSummary` | `Func<int,string>` | `null` | Подпись поля при множественном выборе; по умолчанию «выбрано: N». |
| `CascadeItem.Content` | `UiElement` | `null` | Своё содержимое пункта — уходит в пункт меню как есть. |

`CascadeItem(string text, object value = null)` — `value` по умолчанию = `text`, если не задан
явно; метод `Sub(CascadeItem child)` добавляет дочерний пункт (fluent).

Каждое подменю раскрывается вправо от родительского пункта (зазор 0px, примыкает вплотную) и
**может флипнуться влево**, если справа по краю окна не хватает места, а слева — достаточно
(идентичный алгоритм флипа, что у вложенных подменю `DropdownMenu`, см. соответствующую
страницу) — отдельного отключения флипа здесь нет, `CascadeSelect` получает эту логику «бесплатно»
через реюз `DropdownMenu`.

## Стили

Рамка поля-триггера — слоты `select/field*`; сама панель — слоты меню `DropdownMenu` (см.
страницу «DropdownMenu»).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `CascadeSelect(Action<object> onChange = null)` (конструктор) | — | Создать каскадный выбор с колбэком. |
| `GetSelected()` | `object` | Значение последнего выбранного (до первого выбора — `null`). При множественном выборе — последнее переключённое. |
| `GetSelectedValues()` | `List<object>` | Весь набор выбранных значений при `MultiSelect` (пустой список, если ничего не выбрано). |
| `SetSelected(object value)` | — | Задать выбор из кода: подпись поля берётся у пункта с таким значением. Значение, которого нет в списке, просто сбрасывает выбор. |
| `Clear()` | — | Сбросить выбор: поле возвращается к `Placeholder`. |
| `Rebuild()` | — | Пересобрать меню из текущего списка. Обычно не нужно — состав отслеживается сам; вызывайте, если поменялись **тексты** при том же числе пунктов. |
| `SetStyle(string path, string value)` | — | Точечно задать стиль по пути темы; неизвестный путь или невалидное значение игнорируются. У `CascadeSelect` нет именованных частей стиля — путь применяется к стилю самого элемента. |

## Список, который меняется

Меню пересобирается, когда меняется состав `Items`, поэтому список можно заполнять или
перестраивать (например фильтром) после первого показа — новые пункты будут доступны сразу.
Отслеживается именно **состав**; если поменялись только тексты при том же числе пунктов,
скажите об этом явно через `Rebuild()`.

```csharp
var cs = new CascadeSelect { Key = "region", Clearable = true };
cs.Items.Add(new CascadeItem("Материалы", "materials")
    .Sub(new CascadeItem("Сталь", "steel"))
    .Sub(new CascadeItem("Дерево", "wood")));

cs.SetSelected("steel");              // подпись возьмётся у самого пункта
```

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<object>` | `CascadeItem.Value` листа | Клик по конечному пункту цепочки. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS: только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Выбор изменился (после клика по листу цепочки + `OnChange`). `SelectedIndex` всегда `-1`, `SelectedValue` — `CascadeItem.Value` выбранного листа. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Рамка поля + слоты меню панели. |
| Спрайт-рамка | Да | Рамка поля-триггера. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | При `Disabled` рисуется только поле, меню не показывается. |
| Hover-fade | Нет (на поле-триггере) | Рамка поля переключается жёстко. |


---

[Оглавление](../index_ru.md) - Выбор | Пред.: [TreeSelect](05_treeselect_ru.md) | След.: [ColorPicker](07_colorpicker_ru.md) | [English](06_cascadeselect_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
