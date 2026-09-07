![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [MeterGroup](05_metergroup_ru.md) | След.: [ProgressSpinner](07_progressspinner_ru.md) | [English](06_progressbar_en.md)

---

# ProgressBar

Полоса прогресса: показывает значение от 0 до 1 либо работает в режиме «неопределённо» (бегущая
анимация без конкретного значения, когда прогресс заранее неизвестен).

`RimUI.Components.ProgressBar : UiElement`

## Пример

```csharp
var pb = new ProgressBar(() => (Time.realtimeSinceStartup * 0.1f) % 1f) { StretchFill = true };
pb.FillStyle.Background = Fill.Linear(GradientDirection.Horizontal,
    new GradientStop(0f, Green), new GradientStop(1f, Orange));

new ProgressBar { Indeterminate = true, ShowText = false };
new ProgressBar(() => 0.66f) { Vertical = true, Style = { Height = 90f } };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Value` | `Func<float>` | `null` | Значение 0..1. |
| `Indeterminate` | `bool` | `false` | Бегущий сегмент ~25% длины, цикл ~1.6с (собственная time-based анимация). |
| `ShowText` | `bool` | `true` | Показывать текст значения. |
| `Format` | `Func<float,string>` | `null` (дефолт «NN%») | Форматирование текста значения. |
| `BarHeight` | `float` | `16` | Толщина полосы. |
| `Vertical` | `bool` | `false` | Заполнение снизу вверх. |
| `StretchFill` | `bool` | `false` | Растягивать рисунок заливки в заполненную часть; `false` — заливка обрезается клипом по проценту. |
| `TrackStyle` | `Style` (readonly) | — | Перекрытие стиля трека. |
| `FillStyle` | `Style` (readonly) | — | Перекрытие стиля заливки (например, градиент). |

## Точная формула `Indeterminate`

```
seg   = 0.25 * длина_полосы          // бегущий сегмент — ровно 25% длины трека
cycle = (ctx.Time * 0.625) % 1       // пилообразная фаза 0..1, период 1/0.625 = 1.6 сек ровно
p     = -seg + (длина + seg) * cycle // передний край сегмента: от -seg до +длина за один цикл
```

Видимая часть — пересечение `[p, p+seg]` с `[0, длина]`; кадр не рисуется вовсе, если видимый
кусок меньше 1 пикселя (защита от мерцания на самой границе появления/исчезновения). Полный цикл
занимает ровно 1.6 секунды независимо от FPS (отсчёт от `ctx.Time`, не от числа кадров).

## Стили

Слоты: `progressbar/track`, `progressbar/fill` (оба поддерживают 9-slice спрайт; спрайт заливки
всегда рисуется в заполненную часть, не обрезается).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ProgressBar(Func<float> value = null)` (конструктор) | — | Создать полосу с источником значения. |
| `GetValue()` | `float` | Текущая доля 0..1: если задан `Value` (источник-делегат) — читает его напрямую, иначе возвращает кэш значения последнего отрисованного кадра. `ProgressBar` — только для чтения, метода `SetValue()` нет: писать в компонент нечего, значение всегда приходит извне через `Value`. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути. У `ProgressBar` есть именованные части: `"track"` (`TrackStyle`), `"fill"` (`FillStyle`), `"text"` (`TextStyleOverride`) — например, `pb.SetStyle("fill.color", "#00FF00")`. Путь без префикса части применяется к корневому `Style`. |

## События

Собственных `Action`-колбэков нет — значение читается пассивно каждый кадр. Доступны универсальные события `Events.*`:

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
| Стиль | Да | `TrackStyle`/`FillStyle` + слоты темы. |
| Спрайт-рамка | Да | `progressbar/track`, `progressbar/fill` (9-slice). |
| Анимация | Да | Общий механизм `Style.Animation`; плюс собственная time-based анимация `Indeterminate` (не через реестр). |
| Disabled | Нет | Своего понятия «отключена» нет. |
| Hover-fade | Нет | Не интерактивный элемент. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [MeterGroup](05_metergroup_ru.md) | След.: [ProgressSpinner](07_progressspinner_ru.md) | [English](06_progressbar_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
