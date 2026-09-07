![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [ColorPicker](../06-selection/07_colorpicker_ru.md) | След.: [Chip](02_chip_ru.md) | [English](01_tag_en.md)

---

# Tag

Неинтерактивная цветная плашка для статусов: цвет задаётся уровнем важности (`Severity`), можно
добавить иконку. Клики не обрабатывает.

`RimUI.Components.Tag : FlexBox`

## Пример

```csharp
Row(new Tag("NEW", Severity.Success), new Tag("BETA", Severity.Warn),
    new Tag("BROKEN", Severity.Error), new Tag("INFO") { IconIndex = Icons.Info });
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Content` | `string` | `null` | Текст. |
| `TextElement` | `Text` | `null` | Готовый элемент текста вместо `Content`. |
| `IconIndex` | `int` | `-1` | Индекс листа `Icons`. |
| `Severity` | `Severity` | `Info` | Уровень важности — задаёт цвет фона, см. таблицу ниже. |
| `Rounded` | `bool` | `true` | Пилюля (`BorderRadius.Middle`); `false` — `Small`. |

`Severity` (`RimUI.Components.Severity`, общий enum для `Tag`/`Badge`/`InlineMessage`) — все
значения и их цвет:

| Значение | Цвет фона |
|---|---|
| `Info` (по умолчанию) | акцентный цвет темы (`Theme.Accent`) |
| `Success` | зелёный (`Theme.ButtonSuccessBg`) |
| `Warn` | жёлтый (`Theme.ButtonWarningBg`) |
| `Error` | красный (`Theme.ButtonDangerBg`) |

## Стили

Слот темы — `"tag"` (gap/padding/radius).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Tag(string content = null, Severity severity = Severity.Info)` (конструктор) | — | Создать плашку с текстом и уровнем важности. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути (например, `"text.color"`); применяется к корневому `Style` элемента — именованных частей у `Tag` нет. |

## События

Собственных `Action`-колбэков нет, но доступны универсальные события `Events.*` (см. `RimUI.Core.UiElement.Events`) — работают даже у неинтерактивного `Tag`, так как курсор/клик проверяются по границам элемента независимо от того, обрабатывает ли компонент их сам:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS, не делайте в нём аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слот `"tag"`. |
| Спрайт-рамка | Да | `slot.Sprite` из слота `"tag"`. |
| Анимация | Да | Продемонстрировано и обёрткой (`Animated.Wrap(new Tag(...), "pop", ...)`), и строкой (`Style.Animation = "pulse 1.2 loop"`). |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [ColorPicker](../06-selection/07_colorpicker_ru.md) | След.: [Chip](02_chip_ru.md) | [English](01_tag_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
