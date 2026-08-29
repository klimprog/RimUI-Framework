![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Контейнеры | Пред.: [Panel](01_panel_ru.md) | След.: [Tabs](03_tabs_ru.md) | [English](02_accordion_en.md)

---

# Accordion

Секции «заголовок + тело»: клик по заголовку раскрывает секцию. При `Multiple = false` раскрытие
одной секции закрывает остальные.

`RimUI.Components.Accordion : UiElement`

## Пример

```csharp
var acc = new Accordion();
acc.Section("Общее", body1, startOpen: true);
acc.Section("Бой", body2);
acc.Section("Разное", body3);
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Sections` | `List<AccordionSection>` (readonly) | пусто | Список секций. |
| `Multiple` | `bool` | `false` | Можно ли держать открытыми несколько секций одновременно. |
| `HeaderHeight` | `float` | `30` | Высота заголовка секции. |
| `Gap` | `float` | `4` | Отступ между секциями. |

`AccordionSection`: `string Title`/`Text TitleElement`, `UiElement Body`, `bool StartOpen`.

**Раскрытие мгновенное**, как и у `Panel`: клик сразу переключает булев флаг секции, высота в
`Measure` берётся напрямую из текущего состояния — интерполяции высоты между кадрами нет.

## Стили

Слоты: `accordion/header`, `accordion/header_hover`, `accordion/body`, `accordion/title`,
`accordion/chevron`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Section(string title, UiElement body, bool startOpen = false)` | `Accordion` | Добавить секцию, возвращает себя (цепочка). |
| `SetStyle(string path, string value)` | — | Задать стиль по пути; применяется к корневому `Style` элемента — именованных частей у `Accordion` нет (заголовки/тело секций настраиваются через слоты темы, не через `SetStyle`). |

## События

Собственных `Action`-колбэков нет (`OnChange`/`OnToggle` отсутствуют) — состояние раскрытия
хранится в ID-store. Доступны универсальные события `Events.*`:

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS, не делайте в нём аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу (по всему аккордеону — границы всего компонента, не отдельной секции). |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Слоты `accordion/*`. |
| Спрайт-рамка | Частично | Через `_headStyles[i].Sprite` (устанавливается из резолва стиля слота). |
| Анимация | Нет (кроме `Style.Animation`) | Только дискретное раскрытие. |
| Disabled | Нет | Своего понятия «отключена»/disabled-секций нет. |
| Hover-fade | Нет | Заголовок использует обычный `HoverOn`, без плавного перехода. |


---

[Оглавление](../index_ru.md) - Контейнеры | Пред.: [Panel](01_panel_ru.md) | След.: [Tabs](03_tabs_ru.md) | [English](02_accordion_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
