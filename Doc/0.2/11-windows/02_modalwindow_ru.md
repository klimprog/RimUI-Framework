![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.21` · мод `0.2.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Окна | Пред.: [UiWindow](01_uiwindow_ru.md) | След.: [MessageWindow](03_messagewindow_ru.md) | [English](02_modalwindow_en.md)

---

# ModalWindow

Полноценное окно с шапкой (заголовок + опциональное гамбургер-меню + кнопка закрытия), телом
(заворачивается в скролл при переполнении) и футером с кнопками.

`RimUI.Adapter.ModalWindow : UiWindow`

## Пример

```csharp
var body = new Field { Style = { Gap = 10f } };
body.Add(new Text("Описание...") { Style = { Text = new TextStyle { Wrap = true } } });

var modal = new ModalWindow(body)
{
    Title = "Пример модалки",
    FixedSize = new Vector2(460f, 300f),
    MenuItems = new List<MenuItem> { new MenuItem("Пункт 1", () => { }) },
};
var apply = Button.Make("Применить"); apply.Preset = ButtonPreset.Success;
modal.FooterButtons.Add(apply);
var cancel = Button.Make("Закрыть"); cancel.Preset = ButtonPreset.Danger;
cancel.OnClick = () => modal.Close();
modal.FooterButtons.Add(cancel);
modal.Show();
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Title` | `string` | `null` | Сырой текст заголовка. |
| `TitleText` | `Text` | `null` | Готовый элемент заголовка (приоритетнее `Title`). |
| `MenuItems` | `List<MenuItem>` | `null` | Пункты гамбургер-меню в шапке; `null` = без гамбургера. |
| `HamburgerIcon` | `string` | `null` (= `Icons.Hamburger`) | Свой ключ иконки гамбургера. |
| `CloseIcon` | `string` | `null` (= `Icons.Close`) | Свой ключ иконки закрытия. |
| `Body` | `UiElement` | из конструктора | Контент тела. |
| `FooterButtons` | `List<UiElement>` (readonly) | пусто | Кнопки в футере. |
| `FooterAlign` | `JustifyContent` (`Start`\|`Center`\|`End`\|`SpaceBetween`\|`SpaceAround`\|`SpaceEvenly`) | `End` | Выравнивание кнопок футера — те же значения, что у `Style.JustifyContent` (см. страницу «Style — модель стиля»). |
| `Draggable` | `bool` | `true` | Перетаскивание окна за шапку (своя реализация, не нативный drag). |
| `OnClose` | `Action` | `null` | Вызывается при ЛЮБОМ закрытии (кнопка закрытия, крестик, Esc). |
| `PanelStyle` | `Style` | `null` (= слот `Theme.ModalPanel`) | Стиль панели окна. |
| `SectionGap` | `float` | `0` (из темы) | Зазор между шапкой/телом/футером. |

### Точная механика перетаскивания за шапку

Нативный drag окна RimWorld отключён (`draggable = false`) и заменён собственной реализацией:
нажатие ЛКМ внутри прямоугольника шапки сразу переводит окно в режим перетаскивания (**порога
смещения в пикселях нет вообще** — в отличие от drag&drop в `Grid`/`FlexBox`, где есть порог 5px)
— окно начинает двигаться с первого же события перемещения мыши с зажатой кнопкой. Позиция
клампится к границам экрана по обеим осям: левый/верхний край окна не может стать меньше 0,
правый/нижний — не может выйти за `screenWidth`/`screenHeight`, то есть окно физически невозможно
утащить полностью за пределы экрана.

## Стили

Слот панели — `Theme.ModalPanel`. Если задан спрайт панели — гасится нативная подложка окна и
тень.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `ModalWindow(UiElement body = null, Theme theme = null)` (конструктор) | — | Создать модальное окно с телом. |
| `InitialSize` (override, свойство) | `Vector2` | Авто-размер по контенту либо `FixedSize`. |

## События

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `OnClose` | `Action` | — | Любое закрытие окна: кнопка закрытия в шапке, крестик, клавиша Esc. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | `PanelStyle`, слот `Theme.ModalPanel`. |
| Спрайт-рамка | Да | Спрайт панели гасит нативную подложку и тень окна. |
| Анимация | Да | Общий механизм `Style.Animation` у элементов внутри окна. |
| Disabled | Не применимо к окну | У кнопок футера — обычный `Button.Disabled`. |
| Hover-fade | Не в самом окне | У вложенных `Button`/`DropdownMenu` — да (см. их страницы). |
| Перетаскивание | Да | `Draggable`, своя реализация за шапку. |


---

[Оглавление](../index_ru.md) - Окна | Пред.: [UiWindow](01_uiwindow_ru.md) | След.: [MessageWindow](03_messagewindow_ru.md) | [English](02_modalwindow_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
