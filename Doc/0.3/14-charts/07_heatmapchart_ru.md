![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Графики | Пред.: [ColumnChart (Column / Bar)](06_columnchart_ru.md) | След.: [NodeCanvas](../15-nodecanvas/01_nodecanvas_ru.md) | [English](07_heatmapchart_en.md)

---

# HeatmapChart

Матрица «серия × категория»: строки матрицы — серии (`Series[r].Name` — подпись строки),
столбцы — категории (`Labels`). Цвет каждой ячейки — интерполяция между `MinColor` и `MaxColor`
в зависимости от значения ячейки, по глобальным мин/макс ВСЕХ ячеек (не по строке). Наследует
общий API `Chart` (см. соответствующую страницу).

`RimUI.Components.HeatmapChart : Chart` (sealed)

## Пример

```csharp
var hm = new HeatmapChart { Style = { Height = 220f } };
hm.Labels.AddRange(days);
hm.Series.Add(new ChartSeries("Колонист А", 2f, 5f, 3f, 6f, 1f));
hm.Series.Add(new ChartSeries("Колонист Б", 4f, 2f, 6f, 3f, 5f));
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `MinColor` | `ColorRGBA` | синий | Цвет минимального значения. |
| `MaxColor` | `ColorRGBA` | красный | Цвет максимального значения. |
| `ShowValues` | `bool` | `true` | Печатать значение прямо в ячейке (цвет текста подстраивается под яркость фона). |

Плюс общее API `Chart`: строки — `Series`, столбцы — `Labels`.

### Точная формула интерполяции цвета

```
t = (значение - globalMin) / (globalMax - globalMin)   // клампится в [0, 1]
цвет = MinColor + (MaxColor - MinColor) × t            // линейно по каждому из 4 каналов R,G,B,A
```
`globalMin`/`globalMax` считаются по ВСЕЙ матрице сразу (по всем строкам-сериям вместе), не по
отдельной строке — то есть цвет ячейки отражает её значение относительно всего датасета, а не
только своей серии. Если все значения матрицы одинаковы (`globalMax == globalMin`) — знаменатель
искусственно защищён от деления на ноль, и в этом вырожденном случае все ячейки закрашиваются
ровно в `MinColor` (а не в промежуточный цвет, как можно было бы интуитивно ожидать). Цвет текста
в ячейке (белый/тёмный) выбирается по яркости получившегося цвета ячейки — порог около 0.6 по
формуле стандартной люминансности (`0.299R + 0.587G + 0.114B`).

## Стили

См. страницу «Chart — общая база графиков». Легенда отключена (`LegendCount => 0`) — шкала
непрерывная, категориальные квадратики были бы бессмысленны.

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
| Легенда | Нет | Отключена намеренно — см. выше. |
| Подсказка при наведении | Да | На каждой ячейке матрицы. |


---

[Оглавление](../index_ru.md) - Графики | Пред.: [ColumnChart (Column / Bar)](06_columnchart_ru.md) | След.: [NodeCanvas](../15-nodecanvas/01_nodecanvas_ru.md) | [English](07_heatmapchart_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
