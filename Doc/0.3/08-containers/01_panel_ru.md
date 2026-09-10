![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Контейнеры | Пред.: [Divider](../07-indicators/10_divider_ru.md) | След.: [Accordion](02_accordion_ru.md) | [English](01_panel_en.md)

---

# Panel

Панель с заголовком и телом произвольного содержимого; можно сделать сворачиваемой — клик по
шапке прячет и снова показывает тело.

`RimUI.Components.Panel : UiElement`

## Пример

```csharp
var p = new Panel("Заголовок", myBody) { Collapsible = true };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Title` | `string` | `null` | Текст заголовка. |
| `TitleContent` | `UiElement` | `null` | Своё содержимое заголовка вместо подписи; шеврон сворачивания остаётся на месте. |
| `TitleElement` | `Text` | `null` | Готовый элемент заголовка (приоритетнее `Title`). |
| `Body` | `UiElement` | `null` | Тело панели. |
| `Collapsible` | `bool` | `false` | Сворачиваемость по клику на шапку. |
| `StartCollapsed` | `bool` | `false` | Начальное состояние, если `Collapsible`. |
| `HeaderHeight` | `float` | `30` | Высота шапки. |

## Стили

Слоты: `panel/frame` (9-slice), `panel/header`, `panel/header_hover`, `panel/title`,
`panel/chevron`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Panel(string title = null, UiElement body = null)` (конструктор) | — | Создать панель с заголовком и телом. |
| `SetStyle(string path, string value)` | — | Задать стиль по пути; применяется к корневому `Style` элемента — именованных частей у `Panel` нет (шапка/рамка настраиваются через слоты темы, не через `SetStyle`). |

## События

Собственных `Action`-колбэков нет — состояние раскрытия хранится в ID-store (`BoolState`), клик
обрабатывается внутри `Emit`. Доступны универсальные события `Events.*`:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS, не делайте в нём аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу (по всей панели, включая шапку — независимо от сворачивания по клику). |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

**Раскрытие мгновенное, без интерполяции высоты.** Клик по шапке сразу переключает булев флаг, а
высота панели в `Measure` берётся напрямую из текущего состояния (открыто — высота шапки + тела,
закрыто — высота только шапки) — никакого лерпа/анимации высоты между кадрами нет, панель
«прыгает» к новой высоте за один кадр.

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Через `Style`, слоты `panel/*`. |
| Спрайт-рамка | Да | `panel/frame`, 9-slice. |
| Анимация | Нет (кроме `Style.Animation`) | Только дискретное сворачивание, без собственных модулей. |
| Disabled | Нет | Своего понятия «отключена» нет. |
| Hover-fade | Нет | Наведение на шапку — обычный `HoverOn`, без плавного перехода. |


---

[Оглавление](../index_ru.md) - Контейнеры | Пред.: [Divider](../07-indicators/10_divider_ru.md) | След.: [Accordion](02_accordion_ru.md) | [English](01_panel_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
