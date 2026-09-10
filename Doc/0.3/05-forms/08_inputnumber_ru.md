![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [InputText](07_inputtext_ru.md) | След.: [Textarea](09_textarea_ru.md) | [English](08_inputnumber_en.md)

---

# InputNumber

Числовое поле с кнопками +/− по краям: поддерживает целые и дробные значения, свой шаг изменения
и границы допустимого диапазона.

`RimUI.Components.InputNumber : UiElement`

## Пример

```csharp
new InputNumber(0, 100) { };
new InputNumber(-10, 10) { Integer = false, Step = 0.5 };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Min` | `double` | — | Нижняя граница. |
| `Max` | `double` | `100` | Верхняя граница. |
| `Step` | `double` | `1` | Шаг изменения по кнопкам. |
| `Integer` | `bool` | `true` | Только целые; `false` — допускается дробная точка/запятая. |
| `OnChangeNum` | `Action<double>` | `null` | Вызывается по коммиту (Enter/потеря фокуса) и по кнопкам +/−. |
| `Initial` | `double` | `= Min` | Стартовое значение буфера. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `ButtonsWidth` | `float` | `20` | Ширина колонки кнопок +/− справа. |

## Стили

Слоты темы: `inputnumber/button`, `inputnumber/button_hover`, `inputnumber/button_disabled`,
`inputnumber/button_icon`; поле ввода внутри использует слоты `InputBase` (`input/frame`).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `InputNumber(double min, double max, Action<double> onChange = null)` (конструктор) | — | Создать поле с границами и колбэком. |
| `GetValue()` | `double` | Текущее значение буфера (до первого показа — `Initial`, кламп применён). |
| `SetValue(double v)` | — | Программно установить значение (как коммит): пишет ПРЯМО в буфер поля, минуя `Filter` (как кнопки +/−), и вызывает `OnChangeNum`. |
| `SetStyle(string path, string value)` | — | Часть: `input` — стиль рамки внутреннего поля ввода (`InputBase.FrameStyle`), например `SetStyle("input.borderColor", "#FF5050")`. Отдельных частей для кнопок +/− нет. Неизвестный путь/значение — молча игнорируется. |

### Точные правила посимвольной фильтрации

Каждое изменение буфера прогоняется через фильтр целиком (не «блокировка нажатия», а
перезапись строки постфактум):

- Цифры `0-9` — всегда разрешены.
- Минус `-` — разрешён, только если он оказался бы САМЫМ ПЕРВЫМ символом уже отфильтрованного
  результата (не исходной строки) И `Min < 0`; если `Min >= 0`, минус вырезается всегда.
- Точка ИЛИ запятая — разрешена, только если `Integer == false` и точка ещё не встречалась в
  этом же проходе; запятая при этом **нормализуется в точку** в буфере. При `Integer == true`
  любые `.`/`,` вырезаются целиком.
- Любые другие символы (буквы, `+`, экспонента `e`/`E`, пробелы) молча отбрасываются.

Парсинг при коммите — `double.TryParse(..., NumberStyles.Float, CultureInfo.InvariantCulture)`;
пустая или невалидная строка (например одинокий `"-"`) трактуется как `0`, дальше — обычный
`Clamp(Min, Max)`. Форматирование результата: `Integer` — `((long)Math.Round(v)).ToString()`;
дробное — `v.ToString("0.##", InvariantCulture)` (до двух знаков после точки).

Кнопки +/− работают в обход фильтра: парсят текущий буфер, прибавляют `±Step`, клампят и
записывают результат напрямую в состояние поля.

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChangeNum` | `Action<double>` | новое значение | Коммит (Enter/потеря фокуса) или клик по кнопке +/−. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `InputNumber` не переопределяет эффективную активность (`EffectiveDisabled`) — событие не отражает поле `Disabled` и не сработает при его изменении. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты кнопок +/− и поля ввода. |
| Спрайт-рамка | Да | Фон кнопок +/− может быть спрайтом слота. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Блокирует поле и кнопки. |
| Hover-fade | Нет | Наведение на кнопки +/− переключается жёстко (`OverlayState`), без плавного перехода. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [InputText](07_inputtext_ru.md) | След.: [Textarea](09_textarea_ru.md) | [English](08_inputnumber_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
