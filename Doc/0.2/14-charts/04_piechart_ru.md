![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Графики | Пред.: [PolarChart (Polar Area)](03_polarchart_ru.md) | След.: [LineChart (Line / Area)](05_linechart_ru.md) | [English](04_piechart_en.md)

---

# PieChart (Pie / Donut)

Одна серия данных делит круг на секторы пропорционально значениям категорий; режим Donut — то же
самое, но с вырезом-«бубликом» в центре. Данные — тоже только `Series[0]` (сумма долей = 360°).
Наследует общий API `Chart` (см. соответствующую страницу).

`RimUI.Components.PieChart : Chart` (sealed)

## Пример

```csharp
var pie = new PieChart { Style = { Height = 260f } };
pie.Labels.AddRange(new[] { "Дерево", "Камень", "Металл", "Ткань" });
pie.Series.Add(new ChartSeries("", 35f, 25f, 20f, 20f));

var donut = new PieChart { Style = { Height = 260f }, InnerRadiusFraction = 0.55f };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `InnerRadiusFraction` | `float` | `0` | `0` = обычный пирог; `0..0.9` = Donut — доля внешнего радиуса под вырез в центре. |
| `ShowPercent` | `bool` | `true` | Подписывать долю % прямо на секторе. |
| `MinPercentLabel` | `float` | `0.06` | Минимальная доля, с которой ещё подписывается %. |

Плюс общее API `Chart`. **Данные читаются только из `Series[0]`.**

### Точный расчёт углов

`total` — сумма только ПОЛОЖИТЕЛЬНЫХ значений; нулевые и отрицательные значения пропускаются
целиком (под них не выделяется сектор вообще, даже нулевой ширины). Угол сектора —
`value / total × 360°`, начало отсчёта — `0°` (строго вверх, 12 часов), секторы идут
последовательно по часовой стрелке в порядке следования категорий (каждый следующий сектор
начинается там, где закончился предыдущий). `InnerRadiusFraction` клампится в диапазон `[0,
0.9]` — вырез не может занять больше 90% радиуса.

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
| Многосерийность | Нет | Используется только `Series[0]`. |
| Подсказка при наведении | Да | Хот-споты на секторах. |
| Легенда | Особая | Категории (`Labels`), а не серии. |


---

[Оглавление](../index_ru.md) - Графики | Пред.: [PolarChart (Polar Area)](03_polarchart_ru.md) | След.: [LineChart (Line / Area)](05_linechart_ru.md) | [English](04_piechart_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
