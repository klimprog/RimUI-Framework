![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [TooltipBox](06_tooltipbox_ru.md) | След.: [Icons](08_icons_ru.md) | [English](07_icon_en.md)

---

# Icon

Одна иконка — изображение из атласа с тонированием и поворотом.

`RimUI.Elements.Icon : UiElement` (sealed)

## Пример

```csharp
var raw = new Icon("MyMod/MyIcon") { Tint = ColorRGBA.White, Fit = 0.9f };

// обычно через фабрику встроенного листа - см. страницу "Icons"
var rotated = Icons.Get(Icons.ChevronRight, 20f);
rotated.Rotation = 45f;
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `IconKey` | `string` | из конструктора | Ключ текстуры/атласа. |
| `Tint` | `ColorRGBA` | `White` | Тонирование иконки. |
| `Fit` | `float` | `1` | Доля области под иконку (запас по краям, если < 1). |
| `Rotation` | `float` | `0` | Поворот вокруг центра, градусы по часовой (используется, например, `ProgressSpinner`). |
| `Source` | `RectF?` | `null` (= вся текстура) | Под-прямоугольник атласа для спрайт-листа иконок (см. страницу «Icons»). |

## Стили

Может иметь свой фон/бордюр/радиус как контейнер (унаследовано от `UiElement`) — `Padding`/
`ContentRect` учитываются при размещении самой иконки внутри.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Icon(string iconKey)` (конструктор) | — | Создать иконку по ключу текстуры. |
| `SetStyle(string path, string value)` | — | Изменить стиль строкой (применяется к корневому `Style` — именованных частей у `Icon` нет). Опечатка в пути — молча игнорируется. |

## События

Собственных событий нет, но, как и у любого `UiElement`, доступны универсальные события:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы иконки (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по иконке. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над иконкой. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У `Icon` нет своего disabled — не сработает. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Фон/бордюр/радиус как у контейнера. |
| Спрайт-рамка | Да | `Style.Sprite` поддерживается базовым `UiElement.Emit` для фона-контейнера; собственной темизации спрайтом у `Icon` нет. |
| Анимация | Да | Общий механизм `Style.Animation` + собственное поле `Rotation` (статичный угол, не анимация сама по себе). |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [TooltipBox](06_tooltipbox_ru.md) | След.: [Icons](08_icons_ru.md) | [English](07_icon_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
