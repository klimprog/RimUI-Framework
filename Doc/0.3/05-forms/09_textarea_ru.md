![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.3.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [InputNumber](08_inputnumber_ru.md) | След.: [Field](10_field_ru.md) | [English](09_textarea_en.md)

---

# Textarea

Многострочное поле ввода фиксированной высоты (заданное число видимых строк) — для длинных
текстов.

`RimUI.Components.Textarea : InputBase`

## Пример

```csharp
new Textarea(null) { Rows = 4, Placeholder = "Многострочный текст" };
```

## Параметры

Собственное поле (остальное — см. страницу «InputBase», которую `Textarea` наследует целиком —
конструктор сразу выставляет `Multiline = true`):

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Rows` | `int` | `4` | Высота в строках; работает, только если `Style.Height` не задан явно (формула `Rows * 20f + 8f`). |

## Стили

Слот `input/frame` (+`_focus`/`_disabled`) — унаследован от `InputBase`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Textarea(Action<string> onChange = null)` (конструктор) | — | Создаёт многострочное поле (`Multiline = true`). |

Переопределяет только `Measure` (проставляет высоту по `Rows` перед вызовом базового `Measure`).
Методы значения и стиля — унаследованы от `InputBase` целиком (см. страницу «InputBase»):

| Метод | Возвращает | Описание |
|---|---|---|
| `GetValue()` | `string` | Текущий текст поля: источник `Value`, если задан, иначе кэш последнего отрисованного кадра (до первого показа — `""`, либо значение из `SetValue`). |
| `SetValue(string v)` | — | Программно установить текст (как ввод пользователя): значение пройдёт штатный конвейер поля (`Filter` → `OnChange`) на ближайшем кадре. |
| `SetStyle(string path, string value)` | — | Часть: `frame` — например `SetStyle("frame.borderColor", "#FF5050")`. Неизвестный путь/значение — молча игнорируется. |

## События

Унаследованы от `InputBase`: `OnChange` (`Action<string>`), `OnCommit` (`Action<string>`, по
потере фокуса — многострочные поля не коммитят по Enter).

Универсальные события (доступны у любого элемента через `Events`):

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Textarea`/`InputBase` не переопределяет эффективную активность (`EffectiveDisabled`) — событие не отражает поле `Disabled` и не сработает при его изменении. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `FrameStyle` (унаследовано). |
| Спрайт-рамка | Да | `input/frame` (9-slice). |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | `Disabled` (унаследовано). |
| Hover-fade | Нет | Фокус переключается жёстко, без плавного перехода. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [InputNumber](08_inputnumber_ru.md) | След.: [Field](10_field_ru.md) | [English](09_textarea_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
