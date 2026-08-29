![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Графики | Пред.: [Chart — общая база графиков](01_chart_ru.md) | След.: [PolarChart (Polar Area)](03_polarchart_ru.md) | [English](02_radarchart_en.md)

---

# RadarChart

Лепестковая диаграмма: оси — по числу `Labels` (минимум 3), одна общая шкала на все серии.
Наследует общий API `Chart` (см. страницу «Chart — общая база графиков»).

`RimUI.Components.RadarChart : Chart` (sealed)

## Пример

```csharp
var radar = new RadarChart { Style = { Height = 260f } };
radar.Labels.AddRange(new[] { "Скорость", "Броня", "Урон", "Дальность", "Выносливость" });
radar.Series.Add(new ChartSeries("Боец А", 65f, 40f, 80f, 55f, 70f));

var live = new ChartSeries("Боец Б");
var vals = new List<float> { 45f, 75f, 50f, 90f, 35f };
live.Provider = () => { vals[4] = 35f + 25f * (0.5f + 0.5f * Mathf.Sin(Time.realtimeSinceStartup)); return vals; };
radar.Series.Add(live);
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `GridRings` | `int` | `4` | Число колец сетки. |
| `FillAlpha` | `float` | `0.22` | Прозрачность заливки многоугольника серии. |
| `StrokeWidth` | `float` | `2` | Толщина контура. |

Плюс всё общее API `Chart`: `Series`, `Labels`, `ShowLegend`, `ShowTooltips`.

### Точная геометрия осей

Оси распределены равномерно по кругу: угловой шаг между соседними осями — `360° / n` (`n` —
число категорий, минимум 3 — при меньшем количестве радар не рисуется вовсе). Ось с индексом 0
направлена строго вверх (12 часов); следующие оси идут по часовой стрелке в порядке возрастания
индекса. Радиус паутины — `min(ширина, высота) area × 0.5 − 16` (16 единиц зарезервировано под
подписи категорий по краю). Сетка рисуется как `GridRings` концентрических многоугольников,
радиус кольца `k` = `радиус × k / GridRings`.

## Стили

См. страницу «Chart — общая база графиков» — собственных стилевых полей нет.

## Методы

Собственных публичных методов сверх базового `Chart` нет — реализует только защищённый
`EmitChart` (отрисовку рабочей зоны). `SetStyle(string path, string value)` — см. «Chart».

## События

Собственных событий нет — см. «Chart» (там же — универсальные `Events.Hover`/`Events.Click`/
`Events.Scroll`/`Events.HoverEnter`/`Events.HoverLeave`/`Events.DisabledChanged`, общие для всех
графиков).

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль/Спрайт/Анимация | См. «Chart» | Общий механизм, специфики у `RadarChart` нет. |
| Живые данные | Да | Через `ChartSeries.Provider` — обновляются каждый кадр. |
| Подсказка при наведении | Да | Хот-споты — на каждой вершине многоугольника серии, текст вида «категория · серия: значение». |
| Disabled/Hover-fade | Нет | Не интерактивный элемент (кроме подсказки). |


---

[Оглавление](../index_ru.md) - Графики | Пред.: [Chart — общая база графиков](01_chart_ru.md) | След.: [PolarChart (Polar Area)](03_polarchart_ru.md) | [English](02_radarchart_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
