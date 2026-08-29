![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Отображение | Пред.: [ConfirmWindow](../11-windows/04_confirmwindow_ru.md) | След.: [CustomDraw](02_customdraw_ru.md) | [English](01_imagebox_en.md)

---

# ImageBox

Изображение по ключу текстуры: обычные стили (отступы, скругление, рамка), по умолчанию —
просто картинка. Режимы вписывания: растянуть, вписать целиком, заполнить с обрезкой. Класс
называется `ImageBox`, а не `Image` — это имя занято фабрикой `Fill.Image(...)`.

`RimUI.Components.ImageBox : UiElement`

## Пример

```csharp
var framed = new ImageBox("demo/pattern")
{
    Fit = ImageFit.Cover,
    Style = { Width = 96f, Height = 96f, Radius = BorderRadius.Middle,
              BorderWidth = BorderWidth.Middle, BorderColor = Orange }
};
var tinted = new ImageBox("demo/pattern")
    { Fit = ImageFit.Contain, Tint = new ColorRGBA(0.55f, 0.85f, 1f, 1f), Style = { Width = 96f, Height = 96f } };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `TextureKey` | `string` | `null` | Ключ текстуры. |
| `Fit` | `ImageFit` (`Stretch`\|`Cover`\|`Contain`\|`Auto`) | `Contain` | Режим вписывания. |
| `Tint` | `ColorRGBA` | `White` | Тонирование картинки. |

## Точная математика режимов вписывания

- **`Contain`** — масштаб `k = min(area.width/tex.width, area.height/tex.height)` (по МЕНЬШЕМУ
  отношению, чтобы картинка влезла целиком без обрезки); итоговый размер `tex.size * k`,
  свободное место делится по `Fill.ImageAnchor` (по умолчанию `(0.5, 0.5)` — точное
  центрирование).
- **`Cover`** — масштаб `k = max(area.width/tex.width, area.height/tex.height)` (по БОЛЬШЕМУ
  отношению — картинка гарантированно перекрывает всю область). Геометрически картинка не
  ресайзится — вместо этого вычисляется видимое окно (кроп) текстуры, которое двигается тем же
  якорем `ImageAnchor` внутри полного изображения; лишнее обрезается.
- **`Auto`** — натуральный размер 1:1 по каждой оси отдельно: если размер текстуры меньше
  области — центрируется/якорится внутри неё; если больше — область кропает окно текстуры (как
  `Cover`, но без масштабирования).
- Если у элемента задано скругление (`Style.Radius != None`), используется другой,
  нативный путь отрисовки с маской, и в этом случае **`ImageAnchor` игнорируется** — `Cover`/
  `Auto` кропаются строго по центру независимо от заданного якоря.

## Стили

Реализован через `Style.Background = Fill.Image(...)` — margin/padding/radius/border/тень
работают как у любого обычного элемента, «бесплатно».

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ImageBox(string textureKey = null)` (конструктор) | — | Создать изображение по ключу текстуры. |
| `SetStyle(string path, string value)` | — | Установить поле стиля строкой (`"text.color"`, `"background"`, `"radius"` и т.д.). Именованных частей стиля (`StylePart`) у `ImageBox` нет — путь применяется напрямую к стилю элемента. Неизвестный путь/значение — тихо игнорируется. |

## События

Собственных событий нет, но доступны универсальные события `UiElement.Events` (см. `Source/Core/UiEvents.cs`) — они есть у любого элемента, включая `ImageBox`:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР (~60 раз/сек). Обработчик должен быть лёгким: без аллокаций, поиска, IO — тяжёлая логика здесь просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Полностью, как у любого элемента (реализовано через `Style.Background`). |
| Спрайт-рамка | Косвенно | `Style.Sprite` не игнорируется базовым `UiElement.Emit`, но собственного слота темы, читающего `Sprite`, у `ImageBox` нет. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Отображение | Пред.: [ConfirmWindow](../11-windows/04_confirmwindow_ru.md) | След.: [CustomDraw](02_customdraw_ru.md) | [English](01_imagebox_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
