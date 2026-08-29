![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [ToggleSwitch](04_toggleswitch_ru.md) | След.: [Slider](06_slider_ru.md) | [English](05_togglebutton_en.md)

---

# ToggleButton

Кнопка-переключатель с двумя подписями — например «Вкл»/«Выкл»: клик переключает состояние, и
подпись на кнопке меняется вместе с ним. Внутри использует обычную `Button`.

`RimUI.Components.ToggleButton : UiElement`

## Пример

```csharp
new ToggleButton("Вкл", "Выкл", null) { Key = "tb1" };
new ToggleButton("Пауза", "Играть", null);   // разные подписи, не пара вкл/выкл
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `OnText` | `string` | — | Подпись во включённом состоянии. |
| `OffText` | `string` | = `OnText`, если не задан | Подпись в выключенном состоянии. |
| `OnIcon` | `int` | `-1` | Индекс `Icons` для включённого состояния. |
| `OffIcon` | `int` | `-1` | Индекс `Icons` для выключенного состояния. |
| `On` | `Func<bool>` | `null` | Источник значения. |
| `OnChange` | `Action<bool>` | `null` | Колбэк изменения. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `OnPreset` | `ButtonPreset` (`Default`\|`Danger`\|`Warning`\|`Success`) | `Success` | Пресет кнопки во включённом состоянии — цвета всех значений см. на странице «Button». |
| `OffPreset` | `ButtonPreset` (те же значения) | `Default` | Пресет кнопки в выключенном состоянии. |

## Стили

Использует стиль и слоты обычной `Button` (см. страницу «Button») — отдельных Style-полей у
`ToggleButton` нет.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ToggleButton(string onText, string offText = null, Action<bool> onChange = null)` (конструктор) | — | Если `offText` не задан — используется тот же `onText`. |
| `GetValue()` | `bool` | Текущее значение: источник `On`, если задан, иначе кэш последнего отрисованного кадра (до первого показа — `false`, либо значение из `SetValue`). |
| `SetValue(bool v)` | — | Программно установить значение (как клик): пишет во внутреннее состояние (если нет `On`-источника) и вызывает `OnChange`. Эффект — на ближайшем кадре, работает и до первого показа окна. |
| `SetStyle(string path, string value)` | — | `ToggleButton` не переопределяет части (`StylePart`) и не использует свой корневой `Style` при отрисовке (рисует через внутреннюю приватную `Button`, недоступную снаружи) — вызов практически не имеет видимого эффекта; для оформления используйте `OnPreset`/`OffPreset`. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<bool>` | новое значение | Клик по кнопке, если она не `Disabled`. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `ToggleButton` не переопределяет эффективную активность (`EffectiveDisabled`) — событие не отражает поле `Disabled` и не сработает при его изменении. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Как у обёрнутой `Button`. |
| Спрайт-рамка | Да | Как у `Button` (зависит от `ButtonPreset`). |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Блокирует клики. |
| Hover-fade | Да (унаследовано) | Как у `Button` — плавный переход фона при наведении. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [ToggleSwitch](04_toggleswitch_ru.md) | След.: [Slider](06_slider_ru.md) | [English](05_togglebutton_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
