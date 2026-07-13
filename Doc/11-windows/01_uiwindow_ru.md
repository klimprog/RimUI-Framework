![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — ядро `0.6.7` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Окна | Пред.: [Text](../10-overlays/09_text_ru.md) | След.: [ModalWindow](02_modalwindow_ru.md) | [English](01_uiwindow_en.md)

---

# UiWindow

Базовый класс окна: подключает дерево элементов фреймворка к обычному окну RimWorld
(`Verse.Window`). Обычно напрямую не создаётся — используйте `ModalWindow` (или его потомков
`MessageWindow`/`ConfirmWindow`, см. соответствующие страницы).

`RimUI.Adapter.UiWindow : Verse.Window`

## Пример

```csharp
var root = new Grid();
root.Cell(12, Button.Make("Привет"));
var window = new UiWindow(root) { FixedSize = new Vector2(400f, 300f) };
window.Show();
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Root` | `UiElement` | из конструктора | Корневой элемент содержимого. |
| `Theme` | `Theme` | `null` | Явная тема окна; `null` = следовать активной теме, меняющейся на лету. |
| `FixedSize` | `Vector2` | `Vector2.zero` | Фиксированный размер; `0` = авто-подгон под контент (ширина до 92% экрана, высота до `Theme.MaxWindowHeightFraction`). |

**Жизнь состояния окна**: внутреннее состояние (скроллы, фокус, раскрытия деревьев, значения
контролов без пользовательского источника) живёт СТРОГО от открытия до закрытия окна. Каждое
открытие — чистая инициализация, даже при повторном `Show()` того же экземпляра. Ничего не
переживает закрытие; сбор данных перед закрытием — задача мододела (колбеки/`OnClose`).

## Точная формула авто-размера

```
maxW = UI.screenWidth * 0.92         // захардкоженный литерал, НЕ настраивается темой
maxH = UI.screenHeight * Theme.MaxWindowHeightFraction   // по умолчанию 0.85

итоговая ширина  = min(содержимое.Width  + 36, maxW)   // 36 = 2×Chrome (поля окна по 18px)
итоговая высота  = min(содержимое.Height + 36, maxH)
```

`maxH` считается от ПОЛНОЙ высоты экрана в пикселях — без вычета высоты нижней панели RimWorld
или других элементов интерфейса игры. Содержимое меряется с доступной шириной `maxW - 36` (то
есть уже с учётом полей окна). Если после этих ограничений контент по высоте всё равно не влезает
в получившееся окно — включается внутренний скролл (порог `содержимое.Height > доступная_высота
+ 0.5`, эпсилон на погрешность float).

## Стили

Панель окна — `Theme.WindowPanelFor(SheetKey)` (`protected virtual string SheetKey => "window"`,
переопределяется наследниками).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `UiWindow(UiElement root, Theme theme = null)` (конструктор) | — | Создать окно с корневым элементом. |
| `Show()` | `void` | Добавить окно в `Find.WindowStack`. |
| `InitialSize` (override, свойство) | `Vector2` | Авто-размер по контенту либо `FixedSize`. |

## События

Собственных `Action`-колбэков нет — жизненный цикл управляется методами `Verse.Window`
(`PreOpen`, `PostClose` и т.п.), переопределяемыми в `ModalWindow` (см. страницу «ModalWindow»,
поле `OnClose`).

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль панели | Да | `Theme.WindowPanelFor(SheetKey)`. |
| Спрайт-рамка | Да | Если задан спрайт панели — гасится нативная подложка окна (`doWindowBackground=false`). |
| Скролл при переполнении | Да | Встроенный собственный `ScrollBox`, не нативный `Widgets.BeginScrollView`. |
| Состояние между открытиями | Нет | Намеренно: каждое открытие — с чистого листа; данные собирает мододел. |
| Disabled/Hover-fade | Не применимо | Понятия относятся к отдельным элементам внутри окна, не к окну целиком. |


---

[Оглавление](../index_ru.md) - Окна | Пред.: [Text](../10-overlays/09_text_ru.md) | След.: [ModalWindow](02_modalwindow_ru.md) | [English](01_uiwindow_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
