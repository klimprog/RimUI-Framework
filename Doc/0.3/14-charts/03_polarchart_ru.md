![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Графики | Пред.: [RadarChart](02_radarchart_ru.md) | След.: [PieChart (Pie / Donut)](04_piechart_ru.md) | [English](03_polarchart_en.md)

---

# PolarChart (Polar Area)

Категории делят круг на равные секторы; радиус сектора = значение только из **`Series[0]`**
(остальные серии игнорируются — это однорядный вид). Легенда здесь — категории (`Labels`), а не
серии. Наследует общий API `Chart` (см. соответствующую страницу).

`RimUI.Components.PolarChart : Chart` (sealed)

## Пример

```csharp
var polar = new PolarChart { Style = { Height = 280f } };
string err;
polar.LoadJson(jsonString, out err);
```

```json
{ "labels": ["Сон", "Развлечения", "Работа", "Здоровье", "Настроение"],
  "series": [ { "name": "Колонист", "values": [70, 35, 85, 60, 50] } ] }
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `GridRings` | `int` | `4` | Число колец сетки. |
| `FillAlpha` | `float` | `0.55` | Прозрачность заливки секторов. |
| `ShowScale` | `bool` | `true` | Подписи уровней шкалы по верхней оси. |

Плюс общее API `Chart`. **Данные читаются только из `Series[0]`.**

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
| Легенда | Особая | Показывает категории (`Labels`), а не серии — в отличие от большинства других видов. |


---

[Оглавление](../index_ru.md) - Графики | Пред.: [RadarChart](02_radarchart_ru.md) | След.: [PieChart (Pie / Donut)](04_piechart_ru.md) | [English](03_polarchart_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
