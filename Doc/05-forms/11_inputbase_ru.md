![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — ядро `0.6.7` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [Field](10_field_ru.md) | След.: [Select&lt;T&gt;](../06-selection/01_select_ru.md) | [English](11_inputbase_en.md)

---

# InputBase

Общий базовый класс для `InputText`/`Textarea` и внутреннего поля `InputNumber` — не
предназначен для использования напрямую (у него нет удобных конструкторов-фабрик, как у
потомков), но полезен, если нужно собрать своё поле ввода с другой отрисовкой поверх той же
логики буфера/фокуса.

`RimUI.Elements.InputBase : UiElement` (не sealed)

## Пример

Прямое использование в готовом коде фреймворка не встречается — обычно используется через
`InputText`/`Textarea`/`InputNumber`. Пример работы с общими полями (актуален и для потомков):

```csharp
var field = new InputText();
field.Value = () => myText;
field.OnCommit = v => myText = v;
field.Placeholder = "...";
field.MaxLength = 40;
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Value` | `Func<string>` | `null` | Источник значения — применяется, только когда поле НЕ в фокусе (чтобы не перетирать то, что печатает пользователь). |
| `OnChange` | `Action<string>` | `null` | Колбэк на каждое изменение текста. |
| `OnCommit` | `Action<string>` | `null` | Колбэк по Enter (однострочные поля) или по потере фокуса. |
| `Filter` | `Func<string,string>` | `null` | Фильтр ввода — используется `InputNumber` и hex-полем `ColorPicker`. |
| `Placeholder` | `string` | `null` | Подсказка, когда поле пустое. |
| `ReadOnly` | `bool` | `false` | Только чтение. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `MaxLength` | `int` | `0` (без лимита) | Максимальная длина текста. |
| `Multiline` | `bool` | `false` | Многострочный режим. |
| `MinHeight` | `float` | `28` | Высота однострочного поля. |
| `FrameStyle` | `Style` (readonly) | — | Явный стиль рамки, перекрывает слоты `input/frame(+focus/disabled)`. |

## Стили

Слоты темы: `input/frame`, `input/frame_focus`, `input/frame_disabled` — все поддерживают
9-slice спрайт-рамку.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Focus(LayoutContext ctx)` | `void` | Программно поставить запрос фокуса на следующий кадр. |
| `StateOf(UiState s)` | `InputState` | Доступ к внутреннему состоянию (буфер текста/фокус); используется составными контролами (`InputNumber`, hex-поле `ColorPicker`). |
| `GetValue()` | `string` | Текущий текст поля: источник `Value`, если задан, иначе кэш последнего отрисованного кадра (до первого показа — `""`, либо значение из `SetValue`). |
| `SetValue(string v)` | — | Программно установить текст (как ввод пользователя): значение пройдёт штатный конвейер поля (`Filter` → `OnChange`) на ближайшем кадре. При заданном `Value`-источнике источник главнее — обновите его в `OnChange`, иначе он вернёт своё прежнее значение. |
| `SetStyle(string path, string value)` | — | Часть: `frame` — например `SetStyle("frame.borderColor", "#FF5050")`. Неизвестный путь/значение — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<string>` | новый текст | На каждое изменение содержимого поля. |
| `OnCommit` | `Action<string>` | итоговый текст | По Enter (однострочные) или по потере фокуса. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `InputBase` не переопределяет эффективную активность (`EffectiveDisabled`) — событие не отражает поле `Disabled` и не сработает при его изменении. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `FrameStyle`. |
| Спрайт-рамка | Да | `input/frame` (9-slice). |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Блокирует редактирование. |
| Hover-fade | Нет | Фокус — жёсткое переключение состояния (`OverlayState`), без плавного перехода. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [Field](10_field_ru.md) | След.: [Select&lt;T&gt;](../06-selection/01_select_ru.md) | [English](11_inputbase_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
