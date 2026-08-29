![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Графики | Пред.: [AudioPlayer](../13-audio/01_audioplayer_ru.md) | След.: [RadarChart](02_radarchart_ru.md) | [English](01_chart_en.md)

---

# Chart — общая база графиков

Абстрактный базовый класс для всех видов графиков (`RadarChart`, `PolarChart`, `PieChart`,
`LineChart`, `ColumnChart`, `HeatmapChart` — см. их отдельные страницы). Собирает общую
инфраструктуру: серии данных, легенду, подсказку при наведении, авто-масштаб шкалы, загрузку из
плоского JSON. Данные читаются пассивно каждый кадр — «реактивность» (живые, обновляющиеся
графики) достигается через `ChartSeries.Provider`, а не через события.

`RimUI.Components.Chart : UiElement` (abstract)

## Пример

```csharp
public sealed class ChartSeries
{
    public ChartSeries();
    public ChartSeries(string name, params float[] values);
    public ChartSeries(string name, ColorRGBA color, params float[] values);
}

var live = new ChartSeries("Боец Б");
var vals = new List<float> { 45f, 75f, 50f, 90f, 35f };
live.Provider = () => { vals[4] = 35f + 25f * (0.5f + 0.5f * Mathf.Sin(Time.realtimeSinceStartup)); return vals; };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Series` | `List<ChartSeries>` | пусто | Серии данных. |
| `Labels` | `List<string>` | пусто | Подписи категорий/осей. |
| `ShowLegend` | `bool` | `true` | Показывать легенду. |
| `ShowTooltips` | `bool` | `true` | Показывать подсказку при наведении. |
| `static Palette` | `ColorRGBA[]` | 8 цветов | Цвета серий по кругу, если цвет серии не задан. |

`ChartSeries`: `string Name`, `ColorRGBA Color` (альфа `0` = не задан — цвет берётся из
`Palette` по индексу серии), `List<float> Values`, `Func<IList<float>> Provider` («живой»
источник — приоритетнее `Values`, вызывается каждый кадр).

## Стили

Общий фон/бордюр графика — обычный `Style` элемента. Легенда и подсказки стилизуются внутренней
логикой базового класса, не имеют отдельных полей `Style`.

### Точный алгоритм `NiceMax`

```
m = max / 10^floor(log10(max))        // мантисса, всегда в диапазоне [1, 10)
nice = 1, если m <= 1
       2, если m <= 2
       5, если m <= 5
       10 иначе
результат = nice × 10^floor(log10(max))
```
Примеры: `NiceMax(73) = 100` (мантисса 7.3 → округляется до 10), `NiceMax(45) = 50` (мантисса 4.5
→ округляется до 5), `NiceMax(15) = 20`, `NiceMax(6) = 10`. Общая система осей/шкалы всегда
строится по этому «красивому» верхнему пределу, а не по точному максимуму данных — деления
шкалы поэтому обычно приходятся на круглые числа.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `SeriesColor(int i)` | `ColorRGBA` | Эффективный цвет серии (заданный либо из `Palette`). |
| `LoadJson(string json, out string error)` | `bool` | Загрузить `{ "labels": [...], "series": [ { "name", "color", "values" } ] }`; `false` при ошибке. |
| `static NiceMax(float max)` | `float` | «Красивый» верхний предел шкалы (1/2/5 × 10^k). |
| `static Fmt(float v)` | `string` | Короткое форматирование числа. |
| `SetStyle(string path, string value)` | — | Установить поле стиля строкой (`"text.color"`, `"background"`, `"radius"` и т.д.). Ни у `Chart`, ни у наследников (`RadarChart`, `PolarChart`, `PieChart`, `LineChart`, `ColumnChart`, `HeatmapChart`) нет именованных частей стиля (`StylePart`) — путь применяется напрямую к стилю графика в целом; легенда и подсказки при наведении своих полей `Style` не имеют и `SetStyle` не затрагиваются. Неизвестный путь/значение — тихо игнорируется. |

Метод общий для всех наследников — там отдельно не переописывается.

## События

Собственных событий/колбэков нет — «реактивность» через `ChartSeries.Provider`, не через
события. Универсальные события `UiElement.Events` (см. `Source/Core/UiEvents.cs`), общие для
всех графиков-наследников, всё же доступны:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы графика (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над графиком — КАЖДЫЙ КАДР (~60 раз/сек). Обработчик должен быть лёгким: без аллокаций, поиска, IO — тяжёлая логика здесь просадит FPS. Для собственной интерактивности (подсветка сектора/точки при наведении) — используйте это событие, но держите обработчик минимальным; сама подсказка при наведении (`ShowTooltips`) реализована внутри `Chart` отдельно и `Events.Hover` не использует. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по графику в целом (клики по конкретным секторам/столбцам/точкам отдельно не различаются — хот-споты используются только для подсказки, не для кликов). |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над графиком. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

Эти события общие для всех наследников `Chart` — на их страницах отдельно не переописываются.

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Фон/бордюр графика — обычный `Style`. |
| Спрайт-рамка | Да | Через `Style.Sprite`, как у любого элемента. |
| Анимация | Да | Общий механизм `Style.Animation`; «живая» подача данных — через `Provider`, не через анимации. |
| Disabled | Нет | Своего понятия «отключён» нет. |
| Hover-fade | Нет | Подсказка при наведении появляется/исчезает мгновенно, не через `HoverFade`. |


---

[Оглавление](../index_ru.md) - Графики | Пред.: [AudioPlayer](../13-audio/01_audioplayer_ru.md) | След.: [RadarChart](02_radarchart_ru.md) | [English](01_chart_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
