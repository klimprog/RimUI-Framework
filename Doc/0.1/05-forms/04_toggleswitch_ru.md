![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [RadioButton](03_radiobutton_ru.md) | След.: [ToggleButton](05_togglebutton_ru.md) | [English](04_toggleswitch_en.md)

---

# ToggleSwitch

Переключатель-«пилюля»: клик по нему переключает состояние вкл/выкл, ползунок плавно едет к
нужному краю. Есть отключённое состояние, вид можно задать спрайтом из темы.

`RimUI.Components.ToggleSwitch : UiElement`

## Пример

```csharp
new ToggleSwitch(null, "Переключить") { Key = "sw1" };
new ToggleSwitch(null, "Отключено") { Disabled = true };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `LabelText` | `string` | — | Текст подписи. |
| `LabelElement` | `Text` | `null` | Готовый элемент подписи. |
| `On` | `Func<bool>` | `null` | Источник значения. |
| `OnChange` | `Action<bool>` | `null` | Колбэк изменения. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `TrackWidth` | `float` | `0` (из темы, деф. 38) | Ширина трека. |
| `TrackHeight` | `float` | `0` (из темы, деф. 20) | Высота трека. |
| `Gap` | `float` | `0` (из темы, деф. 8) | Зазор между треком и подписью. |
| `TrackStyle`, `TrackHoverStyle`, `TrackOnStyle`, `TrackDisabledStyle`, `KnobStyle`, `LabelStyle` | `Style` (readonly) | — | Явные стили частей. |

**Зона клика — вся строка целиком** (трек + зазор + подпись), а не только сам трек — клик
засчитывается по общему `Bounds` компонента, без дополнительного расширения/сужения хитбокса.

## Стили

Слоты темы: `switch/track`, `switch/track_hover`, `switch/track_on`, `switch/knob`,
`switch/label`. Спрайт: `switch/track_on`/`track_off` — 9-slice; `knob` — обычный спрайт.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ToggleSwitch(Action<bool> onChange = null, string label = null)` (конструктор) | — | ПЕРВЫМ идёт колбэк, ВТОРЫМ — текст подписи. |
| `GetValue()` | `bool` | Текущее значение: источник `On`, если задан, иначе кэш последнего отрисованного кадра (до первого показа — `false`, либо значение из `SetValue`). |
| `SetValue(bool v)` | — | Программно установить значение (как клик): пишет во внутреннее состояние (если нет `On`-источника) и вызывает `OnChange`. Эффект — на ближайшем кадре, работает и до первого показа окна. |
| `SetStyle(string path, string value)` | — | Части: `track`, `track_hover`, `track_on`, `track_disabled`, `knob`, `label` — например `SetStyle("track.borderColor", "#FF5050")`. Неизвестный путь/значение — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<bool>` | новое значение | Клик по переключателю, если он не `Disabled`. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `ToggleSwitch` не переопределяет эффективную активность (`EffectiveDisabled`) — событие не отражает поле `Disabled` и не сработает при его изменении. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `TrackStyle`/`KnobStyle`/`LabelStyle` + слоты темы. |
| Спрайт-рамка | Да | `switch/track_on`/`track_off` (9-slice), `knob`. |
| Анимация | Да | Общий механизм `Style.Animation`; сам ползунок едет плавно независимо (не через реестр анимаций). |
| Disabled | Да | Свой стиль `TrackDisabledStyle`. |
| Hover-fade | Да | Плавный переход рамки трека при наведении. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [RadioButton](03_radiobutton_ru.md) | След.: [ToggleButton](05_togglebutton_ru.md) | [English](04_toggleswitch_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
