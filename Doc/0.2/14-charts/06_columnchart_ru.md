![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Графики | Пред.: [LineChart (Line / Area)](05_linechart_ru.md) | След.: [HeatmapChart](07_heatmapchart_ru.md) | [English](06_columnchart_en.md)

---

# ColumnChart (Column / Bar)

`Column` показывает данные вертикальными столбцами снизу вверх; `Bar` — те же данные
горизонтальными полосами вбок. Несколько серий группируются рядом внутри слота категории.
Наследует общий API `Chart` (см. соответствующую страницу).

`RimUI.Components.ColumnChart : Chart` (sealed)

## Пример

```csharp
var column = new ColumnChart { Style = { Height = 240f } };
column.Labels.AddRange(cats);
column.Series.Add(new ChartSeries("Смена А", 12f, 19f, 8f, 15f));
column.Series.Add(new ChartSeries("Смена Б", 9f, 14f, 11f, 10f));

var bar = new ColumnChart { Style = { Height = 240f }, Horizontal = true };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Horizontal` | `bool` | `false` | `false` = Column (вертикальные столбцы снизу вверх); `true` = Bar (горизонтальные полосы вбок). |
| `GroupGapFraction` | `float` | `0.28` | Доля слота категории под зазор МЕЖДУ группами. |
| `BarGapFraction` | `float` | `0.10` | Доля ширины группы под зазоры между барами одной группы. |
| `ShowGrid` | `bool` | `true` | Показывать сетку. |
| `GridLines` | `int` | `4` | Число линий сетки. |

Плюс общее API `Chart`. Поддерживает несколько серий одновременно (группируются рядом).

## Стили

См. страницу «Chart — общая база графиков». Прямоугольники столбцов рисуются как обычные
`Background`-команды (не через GL-примитив Poly) — работают и на брусковом фоллбеке без GL.

## Методы

Собственных публичных методов сверх базового `Chart` нет. `SetStyle(string path, string value)` —
см. «Chart».

## События

Собственных событий нет — см. «Chart» (там же — универсальные `Events.Hover`/`Events.Click`/
`Events.Scroll`/`Events.HoverEnter`/`Events.HoverLeave`/`Events.DisabledChanged`, общие для всех
графиков).

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль/Спрайт/Анимация | См. «Chart» | Общий механизм. |
| Многосерийность | Да | Группируются рядом внутри слота категории. |
| Подсказка при наведении | Да | На каждом столбце/баре. |


---

[Оглавление](../index_ru.md) - Графики | Пред.: [LineChart (Line / Area)](05_linechart_ru.md) | След.: [HeatmapChart](07_heatmapchart_ru.md) | [English](06_columnchart_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
