![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [ToggleButton](05_togglebutton_ru.md) | След.: [InputText](07_inputtext_ru.md) | [English](06_slider_en.md)

---

# Slider

Ползунок: перетаскивайте ручку мышью, чтобы изменить значение. Поддерживает шаг, единицы
измерения, деления шкалы, подписи по краям, диапазон значений (две ручки одновременно) и
отключённое состояние.

`RimUI.Components.Slider : UiElement`

## Пример

```csharp
new Slider(0f, 100f) { Step = 5f, Unit = "%", ShowTicks = true, ShowEndLabels = true };
new Slider(0f, 20f) { Range = true, Step = 1f, ShowEndLabels = true };   // диапазон, 2 ручки
new Slider(0f, 100f) { Vertical = true, Style = { Height = 120f } };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Min` | `float` | — | Нижняя граница. |
| `Max` | `float` | `100` | Верхняя граница. |
| `Step` | `float` | `1` | Шаг изменения. |
| `Value` | `Func<float>` | `null` | Источник значения (одинарный режим; нижняя ручка при `Range`). |
| `OnChange` | `Action<float>` | `null` | Колбэк изменения (нижняя/единственная ручка). |
| `Range` | `bool` | `false` | Режим диапазона (две ручки). |
| `ValueHi` | `Func<float>` | `null` | Источник значения верхней ручки (только при `Range`). |
| `OnChangeHi` | `Action<float>` | `null` | Колбэк изменения верхней ручки. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `ShowValue` | `bool` | `true` | Подпись значения над ручкой. |
| `ShowTicks` | `bool` | `false` | Деления под треком. |
| `ShowEndLabels` | `bool` | `false` | Подписи min/max по краям. |
| `Vertical` | `bool` | `false` | Вертикальный режим (min внизу). |
| `Unit` | `string` | `""` | Единица измерения в подписи значения. |
| `Format` | `Func<float,string>` | `null` | Свой форматтер значения. |
| `TrackHeight` | `float` | `0` (из темы `slider/track`, деф. 8) | Толщина трека. |
| `HandleSize` | `float` | `0` (из темы `slider/handle`, деф. 16) | Размер ручки. |
| `TrackStyle`, `FillStyle`, `HandleStyle`, `ValueStyle`, `EndsStyle`, `TickStyle`, `TickMajorStyle` | `Style` (readonly) | — | Явные стили частей. |

## Точная механика перетаскивания

Зона захвата — это ВСЯ полоса трека по толщине ручки (`_hs`, а не только сама ручка): клик в
любом месте трека сразу переводит значение под курсор (ручка «прыгает» к точке клика, а не
только начинает драг с текущей позиции). Формула перевода позиции мыши в долю 0..1 (с инсетом на
половину размера ручки `i = HandleSize/2`, чтобы ручка не вылезала за торцы трека):

```
Горизонтально: f = (mouseX - (track.X + i)) / (track.Width  - 2i)
Вертикально:   f = (track.Bottom - i - mouseY) / (track.Height - 2i)
f = clamp(f, 0, 1)
```

Округление к шагу — обычный `Math.Round` от смещения относительно `Min`, с повторным клампом:

```
value = clamp(Min + Round((Min + f*(Max-Min) - Min) / Step) * Step, Min, Max)
```

В режиме `Range` при захвате ручки определяется, какая из двух ближе к курсору по X (`|mouseX -
xLow| < |mouseX - xHigh|` → тащится верхняя). Пересечение ручек **не имеет отдельного зазора** —
код клампит только `nv < lo` (для верхней) / `nv > hi` (для нижней) строгим неравенством, то есть
`lo == hi` — легальное состояние, ручки могут сойтись вплотную без ограничения снизу.

## Стили

Слоты темы: `slider/track` (9-slice), `slider/handle`, `slider/handle_active`,
`slider/handle_disabled`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Slider(float min, float max, Action<float> onChange = null)` (конструктор) | — | Создать ползунок с границами и колбэком. |
| `GetValue()` | `float` | Текущее значение нижней (единственной) ручки: источник `Value`, если задан, иначе кэш последнего отрисованного кадра. |
| `SetValue(float v)` | — | Программно установить нижнюю (единственную) ручку: пишет во внутреннее состояние (если нет `Value`-источника) и вызывает `OnChange`. Эффект — на ближайшем кадре. |
| `GetValueHi()` | `float` | Текущее значение верхней ручки (только `Range = true`): источник `ValueHi`, если задан, иначе кэш последнего кадра. |
| `SetValueHi(float v)` | — | Программно установить верхнюю ручку (только `Range = true`); пишет во внутреннее состояние (если нет `ValueHi`-источника) и вызывает `OnChangeHi`. |
| `SetStyle(string path, string value)` | — | Части: `track`, `fill`, `handle`, `value`, `ends`, `tick`, `tick_major` — например `SetStyle("handle.borderColor", "#FF5050")`. Неизвестный путь/значение — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<float>` | новое значение | Перетаскивание нижней/единственной ручки. |
| `OnChangeHi` | `Action<float>` | новое значение | Перетаскивание верхней ручки (только `Range = true`). |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Slider` не переопределяет эффективную активность (`EffectiveDisabled`) — событие не отражает поле `Disabled` и не сработает при его изменении. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Все стили частей + слоты темы. |
| Спрайт-рамка | Да | `slider/track` (9-slice), `handle` (обычный спрайт). |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Свой слот `handle_disabled`, перетаскивание блокируется. |
| Hover-fade | Нет | Состояние ручки переключается жёстко (`handle_active`/`handle_disabled`), без плавного перехода. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [ToggleButton](05_togglebutton_ru.md) | След.: [InputText](07_inputtext_ru.md) | [English](06_slider_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
