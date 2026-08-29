![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [InlineMessage](04_inlinemessage_ru.md) | След.: [ProgressBar](06_progressbar_ru.md) | [English](05_metergroup_en.md)

---

# MeterGroup

Полоса, поделённая на сегменты по долям (например, состав ресурса на складе), с легендой под ней.
Цвет каждого сегмента задаёте вы сами вместе с данными.

`RimUI.Components.MeterGroup : UiElement`

## Пример

```csharp
var m = new MeterGroup().Add(55f, steelColor, "Сталь").Add(25f, woodColor, "Дерево")
                         .Add(10f, goldColor, "Золото");
m.Max = 100f;
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Segments` | `List<MeterSegment>` | пусто | Сегменты полосы. |
| `Max` | `float` | `0` (= сумма всех сегментов) | Верхняя граница шкалы. |
| `ShowLegend` | `bool` | `true` | Показывать легенду под полосой. |
| `BarHeight` | `float` | `10` | Высота полосы. |
| `Format` | `Func<float,string>` | `null` | Форматирование значения в легенде (дефолт — число). |

`MeterSegment(float value, ColorRGBA color, string label = null)` — один сегмент: значение,
цвет, подпись.

## Стили

Слот трека — `"meter/track"` (поддерживает спрайт), легенда — `"meter/legend"`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `MeterGroup()` (конструктор) | — | Пустая группа. |
| `MeterGroup(IEnumerable<MeterSegment> segments)` (конструктор) | — | Из готовых сегментов. |
| `Add(float value, ColorRGBA color, string label = null)` | `MeterGroup` | Добавить сегмент, возвращает себя (цепочка). |
| `SetStyle(string path, string value)` | — | Задать стиль по пути (например, `"width"`); применяется к корневому `Style` элемента — именованных частей у `MeterGroup` нет. |

## События

Собственных `Action`-колбэков нет, но доступны универсальные события `Events.*`:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS, не делайте в нём аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Частично | `Width` через `RS` учитывается для ширины трека; `Radius` трека — из слота. |
| Спрайт-рамка | Да (трек) | `trackSlot.Sprite`. |
| Анимация | Да | Общий механизм `Style.Animation` доступен базово. |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [InlineMessage](04_inlinemessage_ru.md) | След.: [ProgressBar](06_progressbar_ru.md) | [English](05_metergroup_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
