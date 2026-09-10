![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [Textarea](09_textarea_ru.md) | След.: [InputBase](11_inputbase_ru.md) | [English](10_field_en.md)

---

# Field

Стилизованная flex-группа: «карточка»/группа с рамкой вокруг произвольного содержимого, дефолты
берутся из слота темы `Theme.Field` (фон/бордюр/радиус/паддинг/gap).

`RimUI.Elements.Field : FlexBox`

## Пример

```csharp
var f = new Field();
f.Style.Width = 240f;
f.Style.Height = 90f;
f.Style.Sprite = new SpriteFrame("demo/pattern", 3f) { Repeat = true };
```

## Параметры

Собственных полей нет — используется целиком API `FlexBox` (см. страницу «FlexBox»): `Axis`,
`Grow`, `DragGroup` и т.д.

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| — | — | — | Собственных параметров нет; см. `FlexBox`. |

## Стили

Дефолтный слот темы — `Theme.Field`: `Padding = 12`, `Gap = 8`, `Radius = Small`, `BorderWidth =
Small`, `BorderColor` с альфой `0.35`, `Background = SurfaceAlt`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Field()` (конструктор) | — | Ось `Column` по умолчанию. |
| `Field(Axis axis)` (конструктор) | — | Явно задать ось. |
| `SetStyle(string path, string value)` | — | `Field` не переопределяет части (`StylePart`) — путь применяется прямо к корневому `Style`, например `SetStyle("background", "#2A2A2A")`. Неизвестный путь/значение — молча игнорируется. |

Остальные методы — унаследованы от `FlexBox` (`Add`, `RemoveChildAt`, `InsertChild`).
`Field` не хранит значение — своих `GetValue`/`SetValue` у него нет.

## События

Собственных событий нет — см. `FlexBox` (drag&drop-колбэки, если задан `DragGroup`). Универсальные
события (доступны у любого элемента через `Events`):

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У `Field` своего понятия «отключена» нет — событие никогда не сработает. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Дефолты — из слота `Theme.Field`. |
| Спрайт-рамка | Да | Через `Style.Sprite`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Своего понятия «отключена» нет. |
| Hover-fade | Нет | Не интерактивный элемент сам по себе. |
| Drag&drop | Да | Унаследовано от `FlexBox` (зона-список). |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [Textarea](09_textarea_ru.md) | След.: [InputBase](11_inputbase_ru.md) | [English](10_field_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
