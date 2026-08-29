![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.1` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Индикация | Пред.: [ProgressBar](06_progressbar_ru.md) | След.: [CircularProgress](08_circularprogress_ru.md) | [English](07_progressspinner_en.md)

---

# ProgressSpinner

Вращающееся кольцо-индикатор загрузки: настраивается размер, скорость вращения и цвет.

`RimUI.Components.ProgressSpinner : UiElement`

## Пример

```csharp
new ProgressSpinner();
new ProgressSpinner { Size = 20f, Speed = 540f, Tint = new ColorRGBA(0.70f, 0.54f, 0.18f, 1f) };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Speed` | `float` | `270` | Скорость вращения, град/сек. |
| `Tint` | `ColorRGBA?` | `null` (= из слота `"spinner"`/акцент темы) | Цвет кольца. |
| `Size` | `float` | `32` | Размер (перекрывается `Style.Width`, если задан). |

## Точная формула вращения

```
Rotation = (ctx.Time * Speed) % 360   // градусы, по часовой стрелке
```

`Speed` — именно градусы в секунду (не радианы). При дефолтном `Speed = 270` полный оборот
занимает `360 / 270 ≈ 1.33` секунды. Направление вращения — только по часовой; чтобы получить
обратное, нужно явно передать отрицательный `Speed` (код это не запрещает, но по умолчанию не
делает).

## Стили

Слот `"spinner"` — используется только для цвета (`Background.Color`), не для спрайта.

## Методы

Явного публичного конструктора с параметрами нет — только беспараметрический
`ProgressSpinner()`.

| Метод | Возвращает | Описание |
|---|---|---|
| `SetStyle(string path, string value)` | — | Задать стиль по пути; применяется к корневому `Style` элемента — именованных частей у `ProgressSpinner` нет. |

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
| Стиль | Частично | `Style.Width` переопределяет `Size`. |
| Спрайт-рамка | Нет | Иконка рисуется с фиксированным `Source`, слот `"spinner"` не читает `Sprite`. |
| Анимация | Да (своя) | Собственное вращение через `Icon.Rotation` от времени; `Style.Animation` доступен базово поверх. |
| Disabled | Нет | Не интерактивный элемент. |
| Hover-fade | Нет | Не найдено доказательств. |


---

[Оглавление](../index_ru.md) - Индикация | Пред.: [ProgressBar](06_progressbar_ru.md) | След.: [CircularProgress](08_circularprogress_ru.md) | [English](07_progressspinner_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
