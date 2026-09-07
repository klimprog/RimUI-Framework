![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.3.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Animations | Previous: [Animations — module registry](01_animations_en.md) | Next: [StyleAnimation](03_styleanimation_en.md) | [Русский](02_animated_ru.md)

---

# Animated

An animation wrapper for any element, with no changes required inside that element. The child emits
commands normally; Animated then post-processes that command range using the accumulated offset,
centered scale, and opacity from every active animation.

`RimUI.Animation.Animated : UiElement`

## Example

```csharp
var fadeBtn = Animated.Wrap(Button.Make("Fade in"), "fade-in", 0.8f);
var pulseTag = new Animated(new Tag("Pulse")).Play("pulse", 1.2f, loop: true);

// Restart a one-shot animation on click.
var shakeBtn = Button.Make("Shake", ButtonPreset.Danger);
var shake = new Animated(shakeBtn).Play("shake", 0.45f);
shakeBtn.OnClick = shake.Restart;
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Child` | `UiElement` | — | Wrapped element. |
| `Specs` | read-only `List<AnimSpec>` | empty | Active animations in effect-accumulation order. |

## Styles

Animated has no style of its own. It is visually transparent and applies effects to commands
already emitted by `Child`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Animated(UiElement child = null)` (constructor) | — | Wrap an element. |
| `Play(string name, float duration = 0.3f, bool loop = false, float param = 0f, float delay = 0f)` | `Animated` | Add a registered animation and return this wrapper. Call repeatedly to accumulate effects. |
| `static Wrap(UiElement child, string name, float duration = 0.3f, bool loop = false, float param = 0f)` | `Animated` | Convenience helper that wraps an element and starts one animation. |
| `Restart()` | `void` | Restart one-shot animations from the beginning, for example on button click. |

## Events

Animated has no component-specific events or callbacks. Animation time is derived from `ctx.Time`,
and each start time is stored in the ID store across frames.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Registry modules | Yes | Any built-in or custom name registered in `Animations`. |
| Multiple animations | Yes | Multiple `Play(...)` calls accumulate effects. |
| Restart | Yes | `Restart()` for one-shot animations. |
| Loop | Yes | Use the `loop` argument to `Play(...)`. |
| Disabled/hover fade | Not applicable | This is a wrapper, not an interactive element. |


---

[Table of contents](../index_en.md) - Animations | Previous: [Animations — module registry](01_animations_en.md) | Next: [StyleAnimation](03_styleanimation_en.md) | [Русский](02_animated_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
