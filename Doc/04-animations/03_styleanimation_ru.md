![RimUI Framework](../../About/Preview.png)

**RimUI Framework** — ядро `0.6.7` · мод `0.1.0` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Анимации | Пред.: [Animated](02_animated_ru.md) | След.: [Label](../05-forms/01_label_ru.md) | [English](03_styleanimation_en.md)

---

# StyleAnimation

Подключение анимации СТРОКОЙ прямо в стиле или в теме — без обёртки `Animated`. Механика
применения та же (пост-обработка диапазона команд), но хук вызывается автоматически внутри
`UiElement.EmitStyled` для любого элемента, у которого задан `Style.Animation`.

`RimUI.Animation.StyleAnimation` (static class)

## Пример

```csharp
var tag = new Tag("Pulse (строкой)") { Style = { Animation = "pulse 1.2 loop" } };
var combo = new Tag("Fade+Slide") { Style = { Animation = "fade-in 0.6; slide-in-top 0.6" } };
```

Та же строка работает и в JSON темы — ключ `"animation"` в любом слоте (например `"button": {
"animation": "pulse 1.2 loop" }`) применится ко всем элементам этого слота сразу, без единой
строки кода.

## Параметры — формат строки-спеки

`"имя [длительность] [loop] [delay:X] [param:X]"` — токены через пробел/запятую; несколько
анимаций — через `;`.

| Часть спеки | Описание |
|---|---|
| Первое «голое» число | Длительность в секундах (`AnimSpec.Duration`). |
| `loop` | Включает циклический режим (`AnimSpec.Loop = true`). |
| `delay:X` | Задержка старта в секундах (`AnimSpec.Delay`). |
| `param:X` | Модуле-специфичный параметр, переопределяет дефолт модуля (`AnimSpec.Param`). |
| Второе «голое» число | То же, что `param:X`, если ключ не указан явно. |

## Стили

Работает через поле `Style.Animation` (см. страницу «Style — модель стиля»); неизвестные имена
модулей и опечатки в токенах молча игнорируются (устойчиво к ошибкам в файле темы).

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `static Install()` | `void` | Включить механизм «анимации из стиля» (идемпотентно): ставит `UiElement.StyleAnimationHook`. Вызывается один раз при инициализации адаптера. |
| `static Parse(string spec)` | `List<AnimSpec>` | Разобрать строку в список анимаций; результат кэшируется по строке. |

## События

Не применимо — хук вызывается автоматически внутри `EmitStyled`, колбэков наружу нет.

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Подключение без кода | Да | Строка прямо в `Style.Animation` или в JSON темы. |
| Несколько анимаций | Да | Через `;` в одной строке. |
| Свои модули | Да | Работает с любым именем, зарегистрированным через `Animations.Register`. |
| Устойчивость к опечаткам | Да | Незнакомые токены и имена модулей молча пропускаются. |


---

[Оглавление](../index_ru.md) - Анимации | Пред.: [Animated](02_animated_ru.md) | След.: [Label](../05-forms/01_label_ru.md) | [English](03_styleanimation_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
