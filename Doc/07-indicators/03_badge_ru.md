![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — ядро `0.6.7` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [Chip](02_chip_ru.md) | След.: [InlineMessage](04_inlinemessage_ru.md) | [English](03_badge_en.md)

---

# Badge

Кружок-счётчик поверх другого элемента: показывает число или просто точку без текста — например,
индикатор непрочитанного.

`RimUI.Components.Badge : UiElement`

## Пример

```csharp
Row(new Badge(() => "7"),
    new Badge(() => "99+") { Severity = Severity.Warn },
    new Badge { Severity = Severity.Error },                 // без Value - просто точка
    new Badge(() => "3") { Target = Button.Make("С бейджем") });
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Value` | `Func<string>` | `null` | Текст бейджа; `null`/пусто = точка размером `DotSize`. |
| `Severity` | `Severity` (`Info`\|`Success`\|`Warn`\|`Error`) | `Error` | Уровень важности — задаёт цвет (акцент/зелёный/жёлтый/красный соответственно; полная таблица цветов — на странице «Tag»). |
| `Target` | `UiElement` | `null` | Если задан, бейдж рисуется поверх правого-верхнего угла этого элемента. |
| `MinSize` | `float` | `16` | Минимальный размер кружка. |
| `DotSize` | `float` | `8` | Размер точки без текста. |

## Стили

Слоты `"badge"` (фон), `"badge/text"` (текст).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Badge(Func<string> value = null)` (конструктор) | — | Создать бейдж с текстовой функцией. |
| `static Count(Func<int> count)` | `Badge` | Фабрика: оборачивает числовую функцию в текстовый бейдж. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути; применяется к корневому `Style` элемента — именованных частей у `Badge` нет. |

## События

Собственных `Action`-колбэков нет, но доступны универсальные события `Events.*`:

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
| Стиль | Частично | Позиционирование не читает `Style` элемента (`Measure` игнорирует `RS`), но фон/текст берутся из слота. |
| Спрайт-рамка | Да, особый | Слот `"badge"` рисуется как целая картинка (`DrawCommand.Image`), а не как 9-slice рамка. |
| Анимация | Да | Общий механизм `Style.Animation` доступен базово. |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [Chip](02_chip_ru.md) | След.: [InlineMessage](04_inlinemessage_ru.md) | [English](03_badge_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
