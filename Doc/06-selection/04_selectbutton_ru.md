![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — ядро `0.6.7` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Выбор | Пред.: [MultiTreeSelect](03_multitreeselect_ru.md) | След.: [TreeSelect](05_treeselect_ru.md) | [English](04_selectbutton_en.md)

---

# SelectButton&lt;T&gt;

Слитый ряд сегментов-кнопок для выбора ровно одного значения: клик по сегменту делает его
активным, остальные становятся неактивными.

`RimUI.Components.SelectButton<T> : UiElement` (generic)

## Пример

```csharp
new SelectButton<string>(new[] { "День", "Неделя", "Месяц" }, null);
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Items` | `List<SelectItem<T>>` | пусто | Сегменты. |
| `Value` | `Func<T>` | `null` | Источник текущего значения. |
| `OnChange` | `Action<T>` | `null` | Колбэк выбора. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `SegmentHeight` | `float` | `28` | Высота сегмента. |
| `SegmentPadding` | `float` | `12` | Горизонтальный паддинг внутри сегмента. |

## Стили

Слоты темы: `selectbutton/segment`, `selectbutton/segment_active`, `selectbutton/segment_hover`,
`selectbutton/segment_disabled`, `selectbutton/text`, `selectbutton/text_disabled`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `SelectButton()` (конструктор) | — | Пустой ряд — заполнить `Items` вручную. |
| `SelectButton(IEnumerable<T> values, Action<T> onChange = null)` (конструктор) | — | Ряд из значений. |
| `GetSelected()` | `T` | Текущий выбранный сегмент: источник `Value`, иначе кэш последнего кадра (до первого показа — `default(T)`, либо значение из `SetSelected`). |
| `SetSelected(T value)` | — | Программно выбрать сегмент (как клик): без источника `Value` пишет во внутреннее состояние (эффект — со следующего кадра), вызывает `OnChange`. |
| `SetStyle(string path, string value)` | — | Точечно задать стиль по пути темы; неизвестный путь или невалидное значение игнорируются. У `SelectButton<T>` нет именованных частей стиля — путь применяется к стилю самого элемента. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<T>` | выбранное значение | Клик по сегменту. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS: только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Выбор изменился (после клика по сегменту + `OnChange`). `SelectedIndex` — индекс сегмента в `Items`, `SelectedValue` — его `Value`. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `selectbutton/*`. |
| Спрайт-рамка | Да | Через `Style.Background`/спрайт слота сегмента (общий механизм резолва стиля, как у остальных компонентов). |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Свой слот `segment_disabled`. |
| Hover-fade | Нет | Подсветка сегмента при наведении — жёсткая (слот `segment_hover`), без плавного перехода. |


---

[Оглавление](../index_ru.md) - Выбор | Пред.: [MultiTreeSelect](03_multitreeselect_ru.md) | След.: [TreeSelect](05_treeselect_ru.md) | [English](04_selectbutton_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
