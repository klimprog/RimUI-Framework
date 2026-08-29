![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.7.9` · мод `0.2.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Формы | Пред.: [StyleAnimation](../04-animations/03_styleanimation_ru.md) | След.: [Checkbox](02_checkbox_ru.md) | [English](01_label_en.md)

---

# Label

Подпись поля ввода: можно добавить иконку слева, пометить поле обязательным звёздочкой и вывести
строку-подсказку под подписью.

`RimUI.Components.Label : FlexBox`

## Пример

```csharp
new Label("Имя колониста");
new Label("Пароль") { Required = true, Hint = "Не короче 8 символов", IconIndex = Icons.Info };
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Content` | `string` | `null` | Текст подписи (сырой). |
| `TextElement` | `Text` | `null` | Готовый элемент текста вместо `Content`, если задан — приоритетнее. |
| `Required` | `bool` | `false` | Рисует маркер «*» после текста. |
| `Hint` | `string` | `null` | Текст подсказки под подписью (сырой). |
| `HintElement` | `Text` | `null` | Готовый элемент подсказки, приоритетнее `Hint`. |
| `IconElement` | `Icon` | `null` | Готовая иконка перед текстом. |
| `IconIndex` | `int` | `-1` | Индекс листа `Icons`, альтернатива `IconElement`. |
| `For` | `UiElement` | `null` | Задел на будущую фокусировку по клику на подпись (функционально пока не реализовано). |

## Стили

Наследует `Style` от `FlexBox` (см. страницу «FlexBox») — фон/бордюр/gap группы. Текст подписи и
подсказки стилизуются через собственные `TextElement.Style.Text`/`HintElement.Style.Text`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Label(string content = null)` (конструктор) | — | Создать подпись с текстом. |
| `SetStyle(string path, string value)` | — | `Label` не переопределяет части (`StylePart`) — путь применяется прямо к корневому `Style` (унаследованному от `FlexBox`), например `SetStyle("text.color", "#FF0000")`. Неизвестный путь или значение — молча игнорируется. |

`Label` не хранит значение — своих `GetValue`/`SetValue` у него нет.

## События

Собственных колбэков нет. Универсальные события (доступны у любого элемента через `Events`):

| Событие | Тип | Параметры | Когда вызывается |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Курсор вошёл/покинул границы (по разу на переход). |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Курсор над элементом — КАЖДЫЙ КАДР. Тяжёлый обработчик просадит FPS. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | ЛКМ / ПКМ по элементу. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Колесо мыши над элементом. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | У `Label` своего понятия «отключена» нет — событие никогда не сработает. |

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Стиль | Да | Как у `FlexBox`. |
| Спрайт-рамка | Да | Через `Style.Sprite` (унаследовано). |
| Анимация | Да | Общий механизм `Style.Animation`. |
| Disabled | Нет | Своего понятия «отключена» нет. |
| Hover-fade | Нет | Не интерактивный элемент. |


---

[Оглавление](../index_ru.md) - Формы | Пред.: [StyleAnimation](../04-animations/03_styleanimation_ru.md) | След.: [Checkbox](02_checkbox_ru.md) | [English](01_label_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
