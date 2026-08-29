![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [SegmentProgress](09_segmentprogress_ru.md) | След.: [Panel](../08-containers/01_panel_ru.md) | [English](10_divider_en.md)

---

# Divider

Тонкая разделительная линия — горизонтальная или вертикальная.

`RimUI.Elements.Divider : UiElement`

## Пример

```csharp
container.Add(new Divider());
var vDiv = new Divider(vertical: true, thickness: 2f, color: Orange);
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Vertical` | `bool` | `false` | Вертикальная ориентация. |
| `Thickness` | `float` | `1` | Толщина линии. |

Цвет передаётся третьим параметром конструктора (`color`); если не задан — берётся из темы
(`Theme.DividerStyle`).

## Стили

Слот темы — `Theme.DividerStyle`. Если цвет задан явно в конструкторе — задаёт
`Style.Background = Fill.Solid(color)` напрямую.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Divider(bool vertical = false, float thickness = 1f, ColorRGBA? color = null)` (конструктор) | — | Создать разделитель. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути (например, `"color"`); применяется к корневому `Style` элемента — именованных частей у `Divider` нет. |

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
| Стиль | Да | Стандартно, в т.ч. через `RS` (резолв стиля). |
| Спрайт-рамка | Да | `Style.Sprite` работает через базовый `UiElement.Emit`, но собственной темизации спрайтом нет — только цвет из `Theme.DividerStyle`. |
| Анимация | Да | Общий механизм `Style.Animation` доступен базово. |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [SegmentProgress](09_segmentprogress_ru.md) | След.: [Panel](../08-containers/01_panel_ru.md) | [English](10_divider_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
