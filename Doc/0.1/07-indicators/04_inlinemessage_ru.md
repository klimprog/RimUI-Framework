![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [Badge](03_badge_ru.md) | След.: [MeterGroup](05_metergroup_ru.md) | [English](04_inlinemessage_en.md)

---

# InlineMessage

Плашка-уведомление прямо в потоке интерфейса (не всплывающая): цвет и иконка зависят от уровня
важности (`Severity`), можно сделать закрываемой крестиком.

`RimUI.Components.InlineMessage : UiElement`

## Пример

```csharp
new InlineMessage(Severity.Info, "Информация");
new InlineMessage(Severity.Warn, "Внимание") { Key = "im1", Closable = true };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Content` | `string` | `null` | Текст сообщения. |
| `TextElement` | `Text` | `null` | Готовый элемент текста вместо `Content`. |
| `Severity` | `Severity` (`Info`\|`Success`\|`Warn`\|`Error`) | — | Уровень важности, задаётся конструктором — определяет цвет и иконку (акцент/зелёный/жёлтый/красный соответственно; полная таблица цветов — на странице «Tag»). |
| `Closable` | `bool` | `false` | Включает крестик закрытия. |
| `OnClose` | `Action` | `null` | Колбэк после закрытия. |

## Стили

Слот темы — `"message"` (gap/padding/radius/borderWidth); фон/бордюр считаются из тона
`Severity` внутри компонента, не мержатся с пользовательским `Style`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `InlineMessage(Severity severity, string content = null)` (конструктор) | — | Создать плашку уровня важности с текстом. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути; применяется к корневому `Style` элемента — именованных частей у `InlineMessage` нет. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnClose` | `Action` | — | После того как плашка помечена закрытой. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS, не делайте в нём аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу (по всей плашке, не только по крестику закрытия — независимо от `OnClose`). |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

**Важно:** состояние «закрыто» хранится в ID-store по `Key` — если на экране несколько плашек,
каждой нужен свой уникальный `Key`, иначе закрытие одной закроет и другие.

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Частично | Слот `"message"` даёт gap/padding/radius; фон/бордюр — из `Severity`, не из пользовательского `Style`. |
| Спрайт-рамка | Да | `slot.Sprite` из слота `"message"`. |
| Анимация | Да | Общий механизм `Style.Animation` доступен базово. |
| Disabled | Нет | Своего понятия «отключена» нет. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [Badge](03_badge_ru.md) | След.: [MeterGroup](05_metergroup_ru.md) | [English](04_inlinemessage_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
