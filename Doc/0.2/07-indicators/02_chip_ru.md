![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [Tag](01_tag_ru.md) | След.: [Badge](03_badge_ru.md) | [English](02_chip_en.md)

---

# Chip

Интерактивная пилюля на нейтральном фоне: можно добавить иконку, а крестиком справа —
по-настоящему убрать чип (например, снять выбранный тег).

`RimUI.Components.Chip : FlexBox`

## Пример

```csharp
new Chip("Обычный");
new Chip("С иконкой") { IconIndex = Icons.Dot };
row.Add(new Chip(label, () => labels.Remove(label)));   // с удалением
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Content` | `string` | `null` | Текст. |
| `TextElement` | `Text` | `null` | Готовый элемент текста вместо `Content`. |
| `IconIndex` | `int` | `-1` | Индекс листа `Icons`. |
| `Removable` | `bool` | авто `true`, если передан `onRemove` | Показывать ли крестик удаления. |
| `OnRemove` | `Action` | `null` | Колбэк клика по крестику. |

## Стили

Слот темы — `"chip"`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Chip(string content = null, Action onRemove = null)` (конструктор) | — | Создать чип; если передан `onRemove`, `Removable` включается автоматически. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути (например, `"background"`); применяется к корневому `Style` элемента — именованных частей у `Chip` нет. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnRemove` | `Action` | — | Клик по крестику удаления. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS, не делайте в нём аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу (по всей пилюле, не только по крестику — `Events.Click` не подменяет `OnRemove`, они независимы). |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слот `"chip"`. |
| Спрайт-рамка | Да | `slot.Sprite` из слота `"chip"`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Своего понятия «отключён» нет. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [Tag](01_tag_ru.md) | След.: [Badge](03_badge_ru.md) | [English](02_chip_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
