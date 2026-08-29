![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.15` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Выбор | Пред.: [CascadeSelect](06_cascadeselect_ru.md) | След.: [Tag](../07-indicators/01_tag_ru.md) | [English](07_colorpicker_en.md)

---

# ColorPicker

Выбор цвета: квадрат HSB (тон/насыщенность/яркость) и полоса тона рядом, ползунок прозрачности,
поле для ручного ввода hex-кода. Пипетки (взять цвет с экрана) нет.

`RimUI.Components.ColorPicker : UiElement`

## Пример

```csharp
new ColorPicker(c => _picked = c) { Value = () => _picked };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Key` | `string` | `null` | Ключ состояния в ID-store (см. «Ключ и состояние» на странице «Архитектура»). Нужен, если элемент пересоздаётся между кадрами или его состояние надо сохранить между пересозданиями. |
| `Value` | `Func<ColorRGBA>` | `null` | Источник текущего цвета. |
| `OnChange` | `Action<ColorRGBA>` | `null` | Колбэк изменения. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `ShowHex` | `bool` | `true` | Показывать редактируемое hex-поле под палитрой. |
| `ShowAlpha` | `bool` | `true` | Показывать полосу прозрачности + альфу в hex. |
| `FieldWidth` | `float` | `44` | Ширина поля-образца цвета. |
| `FieldHeight` | `float` | `24` | Высота поля-образца цвета. |
| `SquareW` | `float` | `160` | Ширина HSB-квадрата. |
| `SquareH` | `float` | `120` | Высота HSB-квадрата. |
| `HueW` | `float` | `16` | Ширина полосы тона (и полосы альфы). |

## Стили

Слоты темы: `colorpicker/field` (бордюр поля-образца), `colorpicker/panel` (панель, поддерживает
спрайт). HSB-квадрат и полосы рисуются собственными градиентами, не спрайтами.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ColorPicker(Action<ColorRGBA> onChange = null)` (конструктор) | — | Создать выбор цвета с колбэком. |
| `GetValue()` | `ColorRGBA` | Текущий цвет: источник `Value`, иначе кэш последнего кадра (либо значение из `SetValue`). У этого компонента пара методов называется `GetValue`/`SetValue` (не `GetSelected`/`SetSelected`), хотя выбор всё равно поднимает событие `Events.SelectionChanged`. |
| `SetValue(ColorRGBA v)` | — | Программно установить цвет (как drag/hex-ввод): без источника `Value` пишет во внутреннее HSB-состояние (эффект — со следующего кадра), вызывает `OnChange`. |
| `SetStyle(string path, string value)` | — | Точечно задать стиль по пути темы (например `"colorpicker.field.bordercolor"` — см. слоты выше); неизвестный путь или невалидное значение игнорируются. У `ColorPicker` нет именованных частей стиля (`StylePart` не переопределён) — путь применяется к стилю самого элемента. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnChange` | `Action<ColorRGBA>` | новый цвет | Перетаскивание по HSB-квадрату/полосам или ввод/коммит валидного hex. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS: только лёгкая логика, без аллокаций/поиска/IO. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Цвет изменился (drag по квадрату/полосам или коммит hex, после `OnChange`). `SelectedIndex` всегда `-1`, `SelectedValue` — новый цвет (`ColorRGBA`, упакован как `object`). |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `colorpicker/field`, `colorpicker/panel`. |
| Спрайт-рамка | Да (только панель) | Слот `colorpicker/panel` может быть спрайтом. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | Образец и рамка приглушаются, панель не открывается. |
| Hover-fade | Нет | Не найдено доказательств плавного перехода при наведении. |


---

[Оглавление](../index_ru.md) - Выбор | Пред.: [CascadeSelect](06_cascadeselect_ru.md) | След.: [Tag](../07-indicators/01_tag_ru.md) | [English](07_colorpicker_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
