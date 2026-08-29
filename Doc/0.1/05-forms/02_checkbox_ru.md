![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [Label](01_label_ru.md) | След.: [RadioButton](03_radiobutton_ru.md) | [English](02_checkbox_en.md)

---

# Checkbox

Флажок: клик по нему переключает значение да/нет. Значение можно хранить в самом элементе или
брать из вашего кода, есть отключённое состояние, вид бокса можно задать спрайтом из темы.

`RimUI.Components.Checkbox : UiElement`

## Пример

```csharp
new Checkbox("Включено", v => myFlag = v) { Key = "chk1", Checked = () => myFlag };
new Checkbox("Отключено") { Disabled = true };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `LabelText` | `string` | `null` | Текст подписи. |
| `LabelElement` | `Text` | `null` | Готовый элемент подписи. |
| `Checked` | `Func<bool>` | `null` | Источник значения; если не задан — своё внутреннее состояние по ID. |
| `OnChange` | `Action<bool>` | `null` | Колбэк изменения. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `BoxSize` | `float` | `0` (из темы `checkbox/box`, деф. 18) | Размер бокса. |
| `Gap` | `float` | `0` (из темы, деф. 8) | Зазор между боксом и подписью. |
| `BoxStyle`, `BoxHoverStyle`, `BoxOnStyle`, `BoxDisabledStyle`, `CheckStyle`, `LabelStyle` | `Style` (readonly) | — | Явные стили частей, перекрывают слоты темы. |

**Зона клика — вся строка целиком** (бокс + зазор + подпись), а не только сам квадратик: клик
засчитывается по общему `Bounds` компонента (`Measure` уже включает `BoxSize + Gap +
LabelWidth`), без дополнительного расширения/сужения хитбокса. Кликать по тексту подписи так же
эффективно, как по самому боксу.

## Стили

Слоты темы: `checkbox/box`, `checkbox/box_on`, `checkbox/box_hover`, `checkbox/check`,
`checkbox/label`. Спрайт — `checkbox/box_on`/`box_off` рисуется целиком (не как 9-slice рамка).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Checkbox(string label = null, Action<bool> onChange = null)` (конструктор) | — | Создать флажок с подписью и колбэком. |
| `GetValue()` | `bool` | Текущее значение: источник-делегат `Checked` напрямую, иначе кэш последнего кадра (до первого показа — `false` либо значение из `SetValue`). |
| `SetValue(bool v)` | — | Программно установить значение (как клик): пишет во внутреннее состояние (если `Checked` не задан) на ближайшем кадре отрисовки и вызывает `OnChange`. При заданном `Checked`-источнике источник истины у мододела — обновите его в `OnChange`. |
| `SetStyle(string path, string value)` | — | Изменить часть стиля строкой (части: `box`, `box_hover`, `box_on`, `box_disabled`, `check`, `label`, напр. `checkbox.SetStyle("box.borderColor", "#FF5050")`; без части — путь применяется к корневому `Style`). Опечатка в пути или невалидное значение — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<bool>` | новое значение | Клик по флажку, если он не `Disabled`, либо программный `SetValue`. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР (~60 раз/сек). Тяжёлый обработчик здесь просадит FPS: только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилось значение `Disabled` между кадрами (у `Checkbox` эффективная активность — просто `Disabled`). |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `BoxStyle`/`CheckStyle`/`LabelStyle` + слоты темы. |
| Спрайт-рамка | Да | `checkbox/box_on`/`box_off`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | `Disabled = true` блокирует клики, свой стиль (`BoxDisabledStyle`). |
| Hover-fade | Да | Плавный переход цвета рамки бокса при наведении (`Theme.HoverFadeDuration`). |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [Label](01_label_ru.md) | След.: [RadioButton](03_radiobutton_ru.md) | [English](02_checkbox_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
