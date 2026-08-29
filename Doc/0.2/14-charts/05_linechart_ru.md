![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Графики | Пред.: [PieChart (Pie / Donut)](04_piechart_ru.md) | След.: [ColumnChart (Column / Bar)](06_columnchart_ru.md) | [English](05_linechart_en.md)

---

# LineChart (Line / Area)

`Line` рисует линию-контур без заливки; `Area` — то же самое, но с заливкой под кривой до
нулевой линии. Несколько серий рисуются как отдельные ломаные. Наследует общий API `Chart` (см.
соответствующую страницу).

`RimUI.Components.LineChart : Chart` (sealed)

## Пример

```csharp
var line = new LineChart { Style = { Height = 240f } };
line.Labels.AddRange(months);
line.Series.Add(new ChartSeries("Добыча", 20f, 35f, 30f, 50f, 45f, 60f));
line.Series.Add(new ChartSeries("Торговля", 40f, 28f, 45f, 38f, 55f, 42f));

var area = new LineChart { Style = { Height = 240f }, Area = true };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Area` | `bool` | `false` | `false` = линия-контур без заливки; `true` = заливка под кривой до нулевой линии. |
| `FillAlpha` | `float` | `0.22` | Прозрачность заливки (режим `Area`). |
| `StrokeWidth` | `float` | `2` | Толщина линии. |
| `ShowMarkers` | `bool` | `true` | Показывать точки-маркеры. |
| `MarkerRadius` | `float` | `3.5` | Радиус маркера. |
| `ShowGrid` | `bool` | `true` | Показывать сетку. |
| `GridLines` | `int` | `4` | Число линий сетки. |

Плюс общее API `Chart`. Поддерживает несколько серий одновременно.

## Стили

См. страницу «Chart — общая база графиков».

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
| Многосерийность | Да | Несколько ломаных одновременно. |
| Подсказка при наведении | Да | Точки-маркеры — хот-споты (работают даже при `ShowMarkers = false`, просто невидимые). |


---

[Оглавление](../index_ru.md) - Графики | Пред.: [PieChart (Pie / Donut)](04_piechart_ru.md) | След.: [ColumnChart (Column / Bar)](06_columnchart_ru.md) | [English](05_linechart_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
