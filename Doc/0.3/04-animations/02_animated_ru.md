![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — ядро `0.8.31` · мод `0.3.1` · RimWorld `1.6`

---

[Оглавление](../index_ru.md) - Анимации | Пред.: [Animations — реестр модулей](01_animations_ru.md) | След.: [StyleAnimation](03_styleanimation_ru.md) | [English](02_animated_en.md)

---

# Animated

Обёртка-аниматор: оборачивает любой элемент, не меняя его код. Ребёнок эмитит команды как обычно,
затем диапазон команд пост-обрабатывается эффектами (смещение, масштаб вокруг центра,
прозрачность), аккумулированными по всем активным на элементе анимациям.

`RimUI.Animation.Animated : UiElement`

## Пример

```csharp
var fadeBtn = Animated.Wrap(Button.Make("Появление"), "fade-in", 0.8f);
var pulseTag = new Animated(new Tag("Pulse")).Play("pulse", 1.2f, loop: true);

// перезапуск one-shot анимации по клику
var shakeBtn = Button.Make("Тряхнуть", ButtonPreset.Danger);
var shake = new Animated(shakeBtn).Play("shake", 0.45f);
shakeBtn.OnClick = shake.Restart;
```

## Параметры

| Параметр | Тип | По умолчанию | Описание |
|---|---|---|---|
| `Child` | `UiElement` | — | Оборачиваемый элемент. |
| `Specs` | `List<AnimSpec>` (только чтение) | пусто | Активные анимации; порядок = порядок аккумуляции эффектов. |

## Стили

Собственного стиля не имеет — визуально прозрачна, эффекты применяются к уже нарисованным
командам `Child`.

## Методы

| Метод | Возвращает | Описание |
|---|---|---|
| `Animated(UiElement child = null)` (конструктор) | — | Обернуть элемент. |
| `Play(string name, float duration = 0.3f, bool loop = false, float param = 0f, float delay = 0f)` | `Animated` | Добавить анимацию по имени из реестра `Animations`; возвращает себя — можно вызвать несколько раз подряд, анимации аккумулируются. |
| `static Wrap(UiElement child, string name, float duration = 0.3f, bool loop = false, float param = 0f)` | `Animated` | Сахар: обернуть и сразу запустить одну анимацию. |
| `Restart()` | `void` | Перезапустить one-shot анимации с нуля (например, по клику на кнопку). |

## События

Собственных событий/колбэков нет — время анимации отсчитывается от `ctx.Time`, старт каждой
анимации хранится в ID-store (переживает кадры).

## Поддержка механик

| Механика | Поддержка | Пояснение |
|---|---|---|
| Модули из реестра | Да | Любое имя, зарегистрированное в `Animations` (встроенное или своё). |
| Несколько анимаций сразу | Да | Несколько вызовов `Play(...)` — эффекты аккумулируются. |
| Перезапуск | Да | `Restart()` для one-shot анимаций. |
| Loop | Да | Параметр `loop` в `Play(...)`. |
| Disabled/Hover-fade | Не применимо | Это обёртка, а не интерактивный элемент. |


---

[Оглавление](../index_ru.md) - Анимации | Пред.: [Animations — реестр модулей](01_animations_ru.md) | След.: [StyleAnimation](03_styleanimation_ru.md) | [English](02_animated_en.md)

## Поддержать автора
Если тебе нравится RimUI Framework, можешь поддержать разработку:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Спасибо! ❤️
