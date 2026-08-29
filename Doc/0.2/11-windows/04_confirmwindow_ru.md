![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Окна | Пред.: [MessageWindow](03_messagewindow_ru.md) | След.: [ImageBox](../12-display/01_imagebox_ru.md) | [English](04_confirmwindow_en.md)

---

# ConfirmWindow

Окно-подтверждение: две кнопки — согласие и отказ, оба варианта приходят как отдельные события.
**Любое закрытие, кроме явного согласия (крестик, Esc, кнопка «Отмена»), трактуется как отказ.**

`RimUI.Adapter.ConfirmWindow : ModalWindow`

## Пример

```csharp
ConfirmWindow.OfText("Удалить блок?", "Действие необратимо.",
    onAccept: () => DeleteBlock(node),
    onDecline: null).Show();
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `OnAccept` | `Action` | `null` | Вызывается при согласии (после закрытия). |
| `OnDecline` | `Action` | `null` | Вызывается при отказе (закрытие крестиком/Esc/кнопкой «Отмена»). |

Остальное — унаследовано от `ModalWindow` (см. соответствующую страницу).

## Стили

Свой стиль и спрайт рамки можно задать отдельно от обычного модального окна — слот
`Theme.ConfirmPanel`, `SheetKey => "confirm"`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ConfirmWindow(string title, UiElement body, string acceptText = "Да", string declineText = "Отмена", Button accept = null, Button decline = null, Theme theme = null)` (конструктор) | — | Создать окно-подтверждение. |
| `static OfText(string title, string text, Action onAccept, Action onDecline = null, float width = 340f)` | `ConfirmWindow` | Фабрика с текстовым телом. |

Кнопка согласия — пресет `Success` по умолчанию; кнопка отказа — `Danger`.

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnAccept` | `Action` | — | Клик по кнопке согласия. |
| `OnDecline` | `Action` | — | Любое закрытие, КРОМЕ явного согласия: крестик, Esc, кнопка «Отмена». |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `PanelStyle`, слот `Theme.ConfirmPanel` (независимо от `Theme.ModalPanel`). |
| Спрайт-рамка | Да | Отдельно от обычного модального окна. |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled/Hover-fade | См. «ModalWindow» | Полностью унаследовано. |


---

[Оглавление](../index_ru.md) - Окна | Пред.: [MessageWindow](03_messagewindow_ru.md) | След.: [ImageBox](../12-display/01_imagebox_ru.md) | [English](04_confirmwindow_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
