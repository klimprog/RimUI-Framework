![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.3.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [OrganizationChart](../09-data/10_organizationchart_ru.md) | След.: [Popover](02_popover_ru.md) | [English](01_button_en.md)

---

# Button

Кнопка: готовые цветовые пресеты, иконки слева и/или справа от текста, отключённое состояние,
всплывающая подсказка (нативный тултип RimWorld). По ширине не растягивается — только по своему
содержимому.

`RimUI.Elements.Button : UiElement` (sealed)

## Пример

```csharp
var b = Button.Make("Сохранить");
b.OnClick = () => Save();
b.Preset = ButtonPreset.Success;

var iconBtn = new Button { Left = Icons.Get(Icons.Close, 16f), Content = new Text("Закрыть") };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Left` | `UiElement` | `null` | Содержимое слева (обычно иконка). |
| `Content` | `UiElement` | `null` | Основное содержимое (обычно текст). |
| `Right` | `UiElement` | `null` | Содержимое справа (обычно иконка). |
| `OnClick` | `Action` | `null` | Колбэк клика. |
| `Disabled` | `bool` | `false` | Отключённое состояние. |
| `DisabledWhen` | `Func<bool>` | `null` | Динамическое условие disabled, проверяется каждый кадр вместе с `Disabled`. |
| `Preset` | `ButtonPreset` | `Default` | Цветовой пресет — см. таблицу ниже. |
| `Tooltip` | `string` | `null` | Нативный тултип RimWorld (не `TooltipBox` фреймворка). |

`ButtonPreset` — все значения и их цвет фона (из палитры темы):

| Значение | Цвет |
|---|---|
| `Default` | нейтральный тёплый серо-коричневый (`Theme.ButtonDefaultBg`) — под ванильные серые кнопки игры |
| `Danger` | красный (`Theme.ButtonDangerBg`) — разрушительные/необратимые действия |
| `Warning` | жёлто-оранжевый (`Theme.ButtonWarningBg`) — предупреждающие действия |
| `Success` | зелёный (`Theme.ButtonSuccessBg`) — подтверждающие/положительные действия |

## Точная формула hover-fade и цветовых сдвигов

Переход фона при наведении — **линейная интерполяция во времени** (не eased-кривая): скорость
постоянна, `speed = 1 / Theme.HoverFadeDuration` (единиц в секунду), при дефолте `0.12` — переход
0→1 занимает ровно 0.12 сек независимо от FPS (отсчёт от игрового времени, не от числа кадров).
Состояние перехода хранится персистентно между кадрами: если навести и убрать курсор на середине
перехода, при повторном наведении переход **продолжится с текущей точки**, а не начнётся заново
с нуля — резкого «скачка» цвета при частом наведении-уведении не будет.

Итоговый цвет — `Lerp(базовый_цвет, сдвинутый_цвет, k)`, где сдвиг — **прямое сложение по
каждому RGB-каналу отдельно** (без перевода в HSV/HSL): `R+=shift, G+=shift, B+=shift`, с клампом
в `[0,1]`; альфа не трогается. `Theme.ButtonHoverShift = 0.08` (осветление), `ButtonPressShift =
-0.07` (затемнение). Pressed и Disabled применяются МГНОВЕННО, без `HoverFade` — сознательное
решение, чтобы нажатие и блокировка кнопки не выглядели «вязкими». Disabled использует другую
формулу — не сдвиг канала, а лерп к среднему по каналам (обесцвечивание) на 55%, а не осветление/
затемнение.

## Стили

Спрайт кнопки берётся из слота `button.sprite`, меняется по состоянию: `enum ButtonState {
Default, Hover, Pressed, Disabled }`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Button()` (конструктор) | — | Пустая кнопка — собрать `Left`/`Content`/`Right` вручную. |
| `static Make(string text, string leftIcon = null, string rightIcon = null)` | `Button` | Фабрика: заворачивает `text` в `Text` (Center/Middle/без переноса), `leftIcon`/`rightIcon` — в `Icon`. |
| `SetStyle(string path, string value)` | — | Изменить стиль строкой (обычные пути стиля, применяется к корневому `Style` — именованных частей у `Button` нет). Опечатка в пути — молча игнорируется. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnClick` | `Action` | — | Клик по кнопке, если она не `Disabled`/`DisabledWhen`. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по кнопке — независимо от `OnClick`, оба механизма работают параллельно. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над кнопкой. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Изменилась эффективная активность (`Disabled`/`DisabledWhen`). |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `Padding`/`Radius` по умолчанию из `Theme.Button`, всё переопределяемо. |
| Спрайт-рамка | Да | Слот `button.sprite`, меняется по `ButtonState`. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Да | `Disabled`/`DisabledWhen`; фон затемняется в серый, текст — `Theme.ButtonTextDisabled`. |
| Hover-fade | Да | Плавный переход фона на hover; pressed/disabled — мгновенно (обратная связь на клик). |


---

[Оглавление](../index_ru.md) - Оверлеи и меню | Пред.: [OrganizationChart](../09-data/10_organizationchart_ru.md) | След.: [Popover](02_popover_ru.md) | [English](01_button_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
