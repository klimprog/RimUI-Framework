![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Выбор | Пред.: [Select&lt;T&gt;](01_select_ru.md) | След.: [MultiTreeSelect](03_multitreeselect_ru.md) | [English](02_multiselect_en.md)

---

# MultiSelect&lt;T&gt;

Выбор нескольких значений из списка: клик по варианту добавляет или убирает его из выбора.
Выбранные показаны чипами с крестиком прямо в поле (не поместившиеся сворачиваются в «+N»);
панель после выбора не закрывается, есть поиск и лимит на число выбранных.

`RimUI.Components.MultiSelect<T> : UiElement` (generic)

## Пример

```csharp
new MultiSelect<string>(skills) { Placeholder = "Навыки", Searchable = true };
new MultiSelect<string>(skills) { Placeholder = "До трёх", MaxSelected = 3 };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Items` | `List<SelectItem<T>>` | пусто | Варианты списка. |
| `Values` | `Func<ICollection<T>>` | `null` | Источник выбранных — контрол мутирует эту коллекцию напрямую, если задана. |
| `OnChange` | `Action<ICollection<T>>` | `null` | Колбэк, получает всю коллекцию выбранных. |
| `Placeholder` | `string` | `"—"` | Текст, когда ничего не выбрано. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `MaxSelected` | `int` | `0` (без лимита) | Максимум выбранных элементов. |
| `MaxListHeight` | `float` | `220` | Максимальная высота панели. |
| `ItemHeight` | `float` | `26` | Высота строки варианта. |
| `FieldHeight` | `float` | `28` | Высота поля-триггера. |
| `Searchable` | `bool` | `false` | Поле поиска вверху панели. |
| `CaseSensitive` | `bool` | `false` | Регистрозависимый поиск. |
| `SearchPlaceholder` | `string` | `"Поиск..."` | Подсказка в поле поиска. |

### Точное поведение `MaxSelected`

При попытке выбрать элемент сверх лимита клик по невыбранному пункту **просто игнорируется** —
без эффекта, панель остаётся открытой и никакого автоматического снятия «самого старого» выбора
не происходит. Чтобы добавить новый элемент при заполненном лимите, пользователь должен сначала
сам снять один из уже выбранных (клик по уже выбранному пункту работает всегда, лимит действует
только на добавление). Панель позиционируется тем же алгоритмом флипа, что и `Select` (см.
соответствующую страницу).

## Стили

Общие слоты с `Select` (`select/field*`, `select/panel`, `select/item*`) плюс `chip`,
`chip/text`, `chip/close` для чипов выбранных значений в поле.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `MultiSelect()` (конструктор) | — | Пустой список. |
| `MultiSelect(IEnumerable<SelectItem<T>> items, Action<ICollection<T>> onChange = null)` (конструктор) | — | Из готовых вариантов. |
| `MultiSelect(IEnumerable<T> values, Action<ICollection<T>> onChange = null)` (конструктор) | — | Из значений. |
| `GetSelected()` | `ICollection<T>` | Текущий набор выбранных: источник `Values`, иначе кэш последнего кадра. Отдельного `SetSelected` нет — набор мутируется кликом по пункту/крестику чипа (коллекция принадлежит мододелу). |
| `SetStyle(string path, string value)` | — | Точечно задать стиль по пути темы; неизвестный путь или невалидное значение игнорируются. У `MultiSelect<T>` нет именованных частей стиля — путь применяется к стилю самого элемента. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<ICollection<T>>` | вся коллекция выбранных | Клик по варианту (добавление/удаление) или клик по крестику чипа. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS: только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Выбор изменился (после переключения пункта + `OnChange`). `SelectedIndex` — индекс переключённого пункта в `Items`, `SelectedValue` — вся коллекция выбранных (`ICollection<T>`) после переключения. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `select/*` + `chip/*`. |
| Спрайт-рамка | Да | Рамка поля и панель. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Панель не открывается. |
| Hover-fade | Да | Рамка поля и строки списка. |


---

[Оглавление](../index_ru.md) - Выбор | Пред.: [Select&lt;T&gt;](01_select_ru.md) | След.: [MultiTreeSelect](03_multitreeselect_ru.md) | [English](02_multiselect_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
