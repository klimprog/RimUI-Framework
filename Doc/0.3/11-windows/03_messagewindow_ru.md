![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Окна | Пред.: [ModalWindow](02_modalwindow_ru.md) | След.: [ConfirmWindow](04_confirmwindow_ru.md) | [English](03_messagewindow_en.md)

---

# MessageWindow

Упрощённая модалка без гамбургер-меню, в футере всего одна кнопка, которая её закрывает.

`RimUI.Adapter.MessageWindow : ModalWindow`

## Пример

```csharp
MessageWindow.OfText("Информация", "Текст сообщения...").Show();
```

## Параметры

Собственных полей нет — используются унаследованные от `ModalWindow` (`Title`, `Body`,
`FooterButtons`, `OnClose`, `PanelStyle`, `Draggable` и т.д., см. страницу «ModalWindow»).

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| — | — | — | См. «ModalWindow»; `MessageWindow` не добавляет новых публичных полей. |

## Стили

Свой стиль и спрайт рамки можно задать отдельно от обычного модального окна — слот
`Theme.MessageboxPanel`, `SheetKey => "messagebox"`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `MessageWindow(string title, UiElement body, string buttonText = "ОК", Button button = null, Theme theme = null)` (конструктор) | — | Создать окно-сообщение. |
| `static OfText(string title, string text, string buttonText = "ОК", float width = 340f)` | `MessageWindow` | Фабрика: тело автоматически заворачивается в `Text` с переносом строк. |

## События

Унаследовано от `ModalWindow`: `OnClose` (`Action`) — вызывается при любом закрытии, в т.ч. по
клику единственной кнопки футера (она сама закрывает окно).

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `PanelStyle`, слот `Theme.MessageboxPanel` (независимо от `Theme.ModalPanel`). |
| Спрайт-рамка | Да | Отдельно от обычного модального окна. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled/Hover-fade | См. «ModalWindow» | Полностью унаследовано. |


---

[Оглавление](../index_ru.md) - Окна | Пред.: [ModalWindow](02_modalwindow_ru.md) | След.: [ConfirmWindow](04_confirmwindow_ru.md) | [English](03_messagewindow_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
