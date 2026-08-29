![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Выбор | Пред.: [Rating](../05-forms/12_rating_ru.md) | След.: [MultiSelect&lt;T&gt;](02_multiselect_ru.md) | [English](01_select_en.md)

---

# Select&lt;T&gt;

Выпадающий список: клик по полю открывает панель с вариантами, клик по варианту выбирает его и
закрывает панель. Поддерживает плейсхолдер, длинные списки со скроллом и поиском, свой спрайт
рамки и отключённое состояние.

`RimUI.Components.Select<T> : UiElement` (generic)

## Пример

```csharp
new Select<string>(new[] { "Колонист", "Пришелец", "Механоид" }, null) { Placeholder = "Выберите тип" };
new Select<int>(many, null) { Placeholder = "40 пунктов", Searchable = true };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Items` | `List<SelectItem<T>>` | пусто | Варианты списка. |
| `Value` | `Func<T>` | `null` | Источник текущего значения. |
| `OnChange` | `Action<T>` | `null` | Колбэк выбора. |
| `Placeholder` | `string` | `"—"` | Текст, когда ничего не выбрано. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `MaxListHeight` | `float` | `220` | Максимальная высота панели. |
| `ItemHeight` | `float` | `26` | Высота строки варианта. |
| `FieldHeight` | `float` | `28` | Высота поля-триггера. |
| `Searchable` | `bool` | `false` | Поле поиска вверху панели (LIKE `%..%`). |
| `CaseSensitive` | `bool` | `false` | Регистрозависимый поиск. |
| `SearchPlaceholder` | `string` | `"Поиск..."` | Подсказка в поле поиска. |

`SelectItem<T>(T value, string text = null)` — вспомогательный класс варианта: `T Value`, `string
Text`, `int Icon`.

### Точная механика позиционирования панели

Панель по умолчанию открывается вниз (`OverlayPlacement.Bottom`) с зазором 2px и **флипает
вверх**, если снизу не влезает, а сверху — влезает (тот же алгоритм флипа, что у `Popover`, см.
страницу «Popover»). Высота панели считается от РЕАЛЬНОГО числа видимых пунктов (уже после
фильтрации поиском, если `Searchable`), а не от общего `Items.Count`: `видимая_высота =
min(видимых_пунктов × ItemHeight, MaxListHeight)`; внутренний скролл появляется, если реальный
список выше `MaxListHeight`.

## Стили

Слоты темы: `select/field`, `select/field_focus`, `select/field_disabled`, `select/panel`,
`select/item`, `select/item_selected`, `select/item_hover`, `select/chevron`,
`select/scrollbar`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Select()` (конструктор) | — | Пустой список — заполнить `Items` вручную. |
| `Select(IEnumerable<T> values, Action<T> onChange = null)` (конструктор) | — | Список из значений. |
| `Select(IEnumerable<SelectItem<T>> items, Action<T> onChange = null)` (конструктор) | — | Список из готовых вариантов. |
| `GetSelected()` | `T` | Текущее выбранное значение: источник `Value`, иначе кэш последнего кадра (до первого показа — `default(T)`, либо значение из `SetSelected`). |
| `SetSelected(T value)` | — | Программно выбрать значение (как клик по пункту): без источника `Value` пишет во внутреннее состояние (эффект — со следующего кадра), вызывает `OnChange`. |
| `SetStyle(string path, string value)` | — | Точечно задать стиль по пути темы (например `"text.color"`, `"background"`); неизвестный путь или невалидное значение игнорируются. У `Select<T>` нет именованных частей стиля (`StylePart` не переопределён) — путь применяется к стилю самого элемента. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<T>` | выбранное значение | Клик по варианту в панели. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS: только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Выбор изменился (после клика по пункту + `OnChange`). `SelectedIndex` — индекс пункта в `Items`, `SelectedValue` — его `Value`. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `select/*`. |
| Спрайт-рамка | Да | Рамка поля и панель могут быть спрайтом слота. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Панель не открывается. |
| Hover-fade | Да | Рамка поля (через общий хелпер отрисовки рамки) и строки списка. |


---

[Оглавление](../index_ru.md) - Выбор | Пред.: [Rating](../05-forms/12_rating_ru.md) | След.: [MultiSelect&lt;T&gt;](02_multiselect_ru.md) | [English](01_select_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
