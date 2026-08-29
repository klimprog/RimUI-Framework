![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [CircularProgress](08_circularprogress_ru.md) | След.: [Divider](10_divider_ru.md) | [English](09_segmentprogress_en.md)

---

# SegmentProgress

Сегментный прогресс-бар: полоса делится на сегменты, каждому можно задать свой цвет; заполняются
сегменты целиком, а не плавной полоской. Есть горизонтальный, вертикальный и круговой варианты.

`RimUI.Components.SegmentProgress : UiElement`

## Пример

```csharp
var hSeg = new SegmentProgress(() => 0.6f) { Segments = 10 };
hSeg.SegmentColors.AddRange(rampColors);

var vSeg = new SegmentProgress(() => 0.6f) { Segments = 8, Vertical = true, Style = { Height = 90f } };
var cSeg = new SegmentProgress(() => 0.6f) { Segments = 12, Circular = true, Style = { Width = 72f } };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Value` | `Func<float>` | `null` | Значение 0..1. |
| `Segments` | `int` | `10` | Число сегментов. |
| `Gap` | `float` | `3` | Зазор между сегментами (линейный режим). |
| `Vertical` | `bool` | `false` | Вертикальный режим. |
| `Circular` | `bool` | `false` | Круговой режим. |
| `BarHeight` | `float` | `16` | Толщина полосы (линейный режим). |
| `Thickness` | `float` | `8` | Толщина кольца (круговой режим). |
| `GapDegrees` | `float` | `8` | Зазор между сегментами кольца, градусы. |
| `SegmentColors` | `List<ColorRGBA>` | пусто | Цвет по индексу сегмента; не хватает записей — берётся акцент темы. |
| `ShowText` | `bool` | `true` | Показывать текст значения. |
| `Format` | `Func<float,string>` | `null` | Форматирование текста. |

Заполненность — `filled = (int)(value * Segments + 0.5)`, т.е. **округление вверх на границе
.5** (`floor(x+0.5)`, а не банковское округление): например `value = 0.55, Segments = 10` даёт
`5.5 + 0.5 = 6.0` → закрашено **6** сегментов, не 5. При `value = 0.5` ровно — `5.0 + 0.5 = 5.5` →
усечение до `5` (граница `.5` срабатывает только когда сама доля уже `x.5`, а не когда `x.0`
попадает на середину сегмента).

В круговом режиме зазор `GapDegrees` **вычитается из равной доли каждого сегмента**, а не
добавляется поверх: каждый из `Segments` слотов занимает ровно `360° / Segments`, из которых
`GapDegrees` уходит под зазор, а `segSpan = 360°/Segments − GapDegrees` остаётся под закрашенную
дугу (минимум `segSpan` — 0.5°, если `GapDegrees` слишком велик относительно доли слота). Сумма
всех слотов (`segSpan + GapDegrees`) всегда даёт полный круг 360°, пока не сработал этот пол.

## Стили

Размер через `Style.Width`/`Height`. Сегменты — `Fill.Solid`; круговые — `ArcDraw.Arc`, без
спрайтов.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `SegmentProgress(Func<float> value = null)` (конструктор) | — | Создать сегментный индикатор с источником значения. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути (например, `"width"`); применяется к корневому `Style` элемента — именованных частей у `SegmentProgress` нет. |

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
| Стиль | Да | Размер через `Style.Width`/`Height`. |
| Спрайт-рамка | Нет | Сегменты — заливка цветом/дуга, без спрайтов. |
| Анимация | Да | Общий механизм `Style.Animation`; своей time-анимации нет. |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [CircularProgress](08_circularprogress_ru.md) | След.: [Divider](10_divider_ru.md) | [English](09_segmentprogress_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
