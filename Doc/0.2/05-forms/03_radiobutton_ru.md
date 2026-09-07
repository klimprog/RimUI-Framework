![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [Checkbox](02_checkbox_ru.md) | След.: [ToggleSwitch](04_toggleswitch_ru.md) | [English](03_radiobutton_en.md)

---

# RadioButton

Радиокнопки объединяются в группу общим значением: клик по одной из них снимает выбор с
остальных в той же группе. Вид кружка и точки внутри можно задать спрайтом из темы.

`RimUI.Components.RadioButton : UiElement`

## Пример

```csharp
row.Add(new RadioButton("Лёгкий", 1, v => _mode = (int)v) { Selected = () => _mode });
row.Add(new RadioButton("Обычный", 2, v => _mode = (int)v) { Selected = () => _mode });
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `LabelText` | `string` | — | Текст подписи. |
| `LabelElement` | `Text` | `null` | Готовый элемент подписи. |
| `Value` | `object` | — | Значение ЭТОЙ кнопки. |
| `Selected` | `Func<object>` | `null` | Текущее значение ГРУППЫ — общий делегат для всех кнопок группы. |
| `OnSelect` | `Action<object>` | `null` | Колбэк выбора. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `CircleSize` | `float` | `0` (из темы `radio/circle`, деф. 18) | Размер кружка. |
| `Gap` | `float` | `0` (из темы, деф. 8) | Зазор между кружком и подписью. |
| `CircleStyle`, `CircleHoverStyle`, `CircleOnStyle`, `CircleDisabledStyle`, `DotStyle`, `LabelStyle` | `Style` (readonly) | — | Явные стили частей. |

**Зона клика — вся строка целиком** (кружок + зазор + подпись), а не только сам кружок — клик
засчитывается по общему `Bounds` компонента, без дополнительного расширения/сужения хитбокса.

## Стили

Слоты темы: `radio/circle`, `radio/circle_hover`, `radio/circle_on`, `radio/dot`, `radio/label`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `RadioButton(string label, object value, Action<object> onSelect = null)` (конструктор) | — | `label` и `value` обязательны. |
| `SetStyle(string path, string value)` | — | Части: `circle`, `circle_hover`, `circle_on`, `circle_disabled`, `dot`, `label` — например `SetStyle("circle.borderColor", "#FF5050")`. Неизвестный путь/значение — молча игнорируется. |

Своих `GetValue`/`SetValue`/`GetSelected`/`SetSelected` у `RadioButton` нет — значение выбора
группы читается и пишется только через делегаты `Selected`/`OnSelect`: кнопка сама значение не
хранит, а лишь сравнивает `Selected()` со своим `Value`.

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnSelect` | `Action<object>` | `Value` этой кнопки | Клик по кнопке, если она не `Disabled`. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex` (всегда `-1` — у радиокнопки нет индекса), `data.SelectedValue` (`Value` этой кнопки) | Клик по кнопке, если она не `Disabled` — после срабатывания `OnSelect`. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность (`Disabled`). |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `CircleStyle`/`DotStyle`/`LabelStyle` + слоты темы. |
| Спрайт-рамка | Да | Спрайт кружка из слота `radio/circle*`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Свой стиль `CircleDisabledStyle`, клики блокируются. |
| Hover-fade | Да | Плавный переход рамки кружка при наведении. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [Checkbox](02_checkbox_ru.md) | След.: [ToggleSwitch](04_toggleswitch_ru.md) | [English](03_radiobutton_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
