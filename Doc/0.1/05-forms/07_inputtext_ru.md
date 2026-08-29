![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [Slider](06_slider_ru.md) | След.: [InputNumber](08_inputnumber_ru.md) | [English](07_inputtext_en.md)

---

# InputText

Однострочное поле ввода: можно задать плейсхолдер (подсказку, которая исчезает при вводе),
привязать значение к вашей переменной, добавить иконку, сделать поле только для чтения,
отключить его или задать спрайт рамки.

`RimUI.Components.InputText : InputBase`

## Пример

```csharp
new InputText { Placeholder = "Введите имя" };
new InputText { Placeholder = "Поиск", LeftIcon = Icons.Info };
new InputText { Value = () => "только чтение", ReadOnly = true };
```

## Параметры

Собственные поля (остальное — см. страницу «InputBase», которую `InputText` наследует целиком):

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `LeftIcon` | `int` | `-1` | Индекс `Icons` слева внутри рамки поля (ужимает контентную зону). |
| `RightIcon` | `int` | `-1` | Индекс `Icons` справа внутри рамки поля. |

## Стили

Слот `input/frame` (+`_focus`/`_disabled`) — унаследован от `InputBase`, поддерживает 9-slice
спрайт-рамку.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `InputText(Action<string> onChange = null)` (конструктор) | — | Создать поле с колбэком изменения. |

Переопределяет только `Emit` (рисует иконки поверх базовой отрисовки поля из `InputBase`). Методы
значения и стиля — унаследованы от `InputBase` целиком (см. страницу «InputBase»):

| Метод | Возвращает | Описание |
|---|---|---|
| `GetValue()` | `string` | Текущий текст поля: источник `Value`, если задан, иначе кэш последнего отрисованного кадра (до первого показа — `""`, либо значение из `SetValue`). |
| `SetValue(string v)` | — | Программно установить текст (как ввод пользователя): значение пройдёт штатный конвейер поля (`Filter` → `OnChange`) на ближайшем кадре. |
| `SetStyle(string path, string value)` | — | Часть: `frame` — например `SetStyle("frame.borderColor", "#FF5050")`. Неизвестный путь/значение — молча игнорируется. |

## События

Унаследованы от `InputBase`: `OnChange` (`Action<string>`, на каждое изменение), `OnCommit`
(`Action<string>`, по Enter или потере фокуса).

Универсальные события (доступны у любого элемента через `Events`):

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `InputText`/`InputBase` не переопределяет эффективную активность (`EffectiveDisabled`) — событие не отражает поле `Disabled` и не сработает при его изменении. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `FrameStyle` (унаследовано). |
| Спрайт-рамка | Да | `input/frame` (9-slice). |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | `Disabled` (унаследовано). |
| Hover-fade | Нет | Фокус — жёсткое переключение состояния (`OverlayState`), без плавного перехода. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [Slider](06_slider_ru.md) | След.: [InputNumber](08_inputnumber_ru.md) | [English](07_inputtext_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
