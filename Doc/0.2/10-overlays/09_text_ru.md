![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Icons](08_icons_ru.md) | След.: [ConfirmPopup](10_confirmpopup_ru.md) | [English](09_text_en.md)

---

# Text

Текстовый элемент.

`RimUI.Elements.Text : UiElement` (sealed)

## Пример

```csharp
new Text("Панель A") { Style = { Text = new TextStyle { Align = TextAlign.Center,
    VAlign = VerticalAlign.Middle, Color = ColorRGBA.White } } };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Content` | `string` | из конструктора | Текст. |
| `Ts` (свойство) | `TextStyle` | создаётся по требованию | `Style.Text ?? (Style.Text = new TextStyle())` — удобно для прямой правки без ручного создания `TextStyle`. |

## Стили

Эффективный стиль текста — резолвнутый стиль темы (слот `Theme.TextSlot`), если задан слотом,
иначе собственный `Style.Text`/`Ts`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Text(string content)` (конструктор) | — | Создать текстовый элемент. |
| `SetStyle(string path, string value)` | — | Изменить стиль строкой (обычные пути стиля, включая `text.*`, применяются к корневому `Style` — именованных частей у `Text` нет). Опечатка в пути — молча игнорируется. |

Ширина по умолчанию — натуральная ширина строки (если `Style.Width` не задан); перенос
(`TextStyle.Wrap`) применяется, только если строка не влезает в доступную ширину.

## События

Собственных событий нет, но, как и у любого `UiElement`, доступны универсальные события:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы текста (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по тексту. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над текстом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У `Text` нет своего disabled — не сработает. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | В т.ч. фон/бордюр как у контейнера (обычно не используется для текста). |
| Спрайт-рамка | Да | Через базовые механизмы `UiElement` (паддинг и функциональная рамка спрайта учитываются в `Measure`). |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [Icons](08_icons_ru.md) | След.: [ConfirmPopup](10_confirmpopup_ru.md) | [English](09_text_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
