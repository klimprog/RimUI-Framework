![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [ProgressSpinner](07_progressspinner_ru.md) | След.: [SegmentProgress](09_segmentprogress_ru.md) | [English](08_circularprogress_en.md)

---

# CircularProgress

Круговой прогресс-бар: разные формы дуги (круг, полукруг, три четверти), точка старта и
направление заполнения, градиент вдоль дуги, бордюры вокруг полосы, текст или иконка по центру.

`RimUI.Components.CircularProgress : UiElement`

## Пример

```csharp
var c = new CircularProgress(() => 0.7f) { Style = { Width = 72f } };
c.BarColorEnd = Orange; c.OuterBorder = 1f; c.InnerBorder = 1f;

var icon = new CircularProgress(() => 1f) { Icon = Icons.Check, Style = { Width = 72f } };
var half = new CircularProgress(() => 0.7f) { Shape = ArcShape.Half, Style = { Width = 72f } };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Value` | `Func<float>` | `null` | Значение 0..1. |
| `Shape` | `ArcShape` (`Full`\|`Half`\|`ThreeQuarter`) | `Full` | Форма дуги. |
| `Start` | `ArcStart` (`Top`\|`Right`\|`Bottom`\|`Left`) | `Top` | Точка старта (только для `Full`). |
| `Reverse` | `bool` | `false` | Заполнение против часовой стрелки. |
| `Thickness` | `float` | `6` | Толщина полосы. |
| `OuterBorder` | `float` | `0` | Толщина внешнего бордюра. |
| `InnerBorder` | `float` | `0` | Толщина внутреннего бордюра. |
| `BarColor` | `ColorRGBA?` | `null` | Цвет полосы (начало градиента, если задан `BarColorEnd`). |
| `BarColorEnd` | `ColorRGBA?` | `null` | Конечный цвет градиента вдоль дуги. |
| `TrackColor` | `ColorRGBA?` | `null` | Цвет фонового трека. |
| `BorderColor` | `ColorRGBA?` | `null` | Цвет бордюров. |
| `ShowText` | `bool` | `true` | Показывать текст значения в центре. |
| `Format` | `Func<float,string>` | `null` | Форматирование текста. |
| `Icon` | `int` | `-1` | Индекс `Icons` в центре (приоритетнее текста). |
| `IconSize` | `float` | `16` | Размер иконки в центре. |

## Точная геометрия дуги

Система отсчёта углов — «циферблат»: **0° = верх, углы растут по часовой стрелке**.

`Start` переводится в градусы (действует ТОЛЬКО при `Shape = Full`, для `Half`/`ThreeQuarter`
игнорируется):

| `ArcStart` | Угол начала |
|---|---|
| `Top` | 0° |
| `Right` | 90° |
| `Bottom` | 180° |
| `Left` | 270° |

`Shape` задаёт и точку старта, и угловой диапазон (при `Half`/`ThreeQuarter` — жёстко, без
влияния `Start`):

| `ArcShape` | Начало | Диапазон | Где визуально разрыв |
|---|---|---|---|
| `Full` | по `Start` (см. таблицу выше) | 360° | нет разрыва |
| `Half` | 270° (слева) | 180° | нижняя половина круга |
| `ThreeQuarter` | 225° (низ-лево) | 270° | 90° внизу (между 135° и 225°) |

`Reverse`: без него закрашенная часть растёт от начала диапазона по часовой на `span × value`
градусов; с `Reverse = true` закрашенная часть стартует от КОНЦА диапазона (`start + span`) и
растёт против часовой к началу — то есть дуга «наполняется» с противоположного края, а не просто
меняет визуальное направление заливки на месте.

## Стили

Размер круга — `Style.Width`/`Style.Height`. Текстовый слот — `"circular/text"`. Дуга рисуется
процедурно (`ArcDraw.Arc`), не спрайтами.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `CircularProgress(Func<float> value = null)` (конструктор) | — | Создать круговой индикатор с источником значения. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути (например, `"width"`); применяется к корневому `Style` элемента — именованных частей у `CircularProgress` нет (текстовый стиль в центре — через `TextStyleOverride`, не через `SetStyle`). |

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
| Стиль | Да | Размер через `Style.Width`/`Height`. |
| Спрайт-рамка | Нет | Дуга рисуется процедурно (заливка/бордюр цветом), не спрайтами. |
| Анимация | Да | Общий механизм `Style.Animation`; сама дуга статична (без вращения). |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [ProgressSpinner](07_progressspinner_ru.md) | След.: [SegmentProgress](09_segmentprogress_ru.md) | [English](08_circularprogress_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
