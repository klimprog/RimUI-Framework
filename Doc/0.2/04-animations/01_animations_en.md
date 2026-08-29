![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Animations | Previous: [Theme — themes and theme files](../03-themes/01_theme_en.md) | Next: [Animated](02_animated_en.md) | [Русский](01_animations_ru.md)

---

# Animations — module registry

The animation module registry contains built-in effects and accepts custom ones. Immediate-mode
rendering makes animation cheap to integrate: the element is redrawn every frame, and offsets,
scale, and opacity are applied to its emitted commands afterward. Attach a module with an
`Animated` wrapper or with a string in `Style.Animation` or a theme slot.

`RimUI.Animation.Animations` (static class)

## Example

```csharp
using RimUI.Animation;

public sealed class MyBounce : IUiAnimation
{
    public void Apply(ref AnimFx fx, float k, AnimSpec spec)
    {
        float amp = spec.Param > 0f ? spec.Param : 8f;
        fx.Dy -= amp * Mathf.Sin(k * Mathf.PI);   // Mutate fx; effects accumulate.
    }
}

Animations.Register("my-bounce", new MyBounce());
// Use it like a built-in module: Animated.Wrap(el, "my-bounce", 0.5f)
// or Style.Animation = "my-bounce 0.5"
```

## Built-in modules

| Name | Behavior | Type | Default `Param` |
|---|---|---|---|
| `fade-in` | Opacity 0→1 | one-shot | — |
| `fade-out` | Opacity 1→0 | one-shot | — |
| `pulse` | Scale pulse ±amp | loop | amplitude 0.05 (±5%) |
| `blink` | Opacity blink for alerts | loop | depth 0.45 |
| `slide-in-left` | Slide in from the left while fading in | one-shot | distance 24 units |
| `slide-in-top` | Slide in from the top while fading in | one-shot | distance 16 units |
| `shake` | Decaying shake for invalid input | one-shot | amplitude 4 units |
| `fade-in-up` | Softer upward slide and fade for small elements | one-shot | distance 12 units |
| `pop` | Scale in with a slight overshoot | one-shot | starting scale 0.8 |
| `flash` | Single opacity flash; repeats when `Loop=true` | one-shot/loop | depth 0.7 |

`AnimSpec` describes one animation instance: `string Name`, `float Duration = 0.3f` in seconds
(the period for loops), `bool Loop`, `float Delay` in seconds, and module-specific `float Param`,
where 0 uses the module default.

`AnimFx` contains per-frame effects accumulated across all active animations on an element:
`float Dx, Dy` in framework units, `float Scale` around the element center (1 means unchanged),
and `float Opacity` as a multiplier (1 means opaque).

### Exact built-in formulas

The shared phase is `k = loop ? (t/dur) % 1 : min(t/dur, 1)`, where
`t = ctx.Time - started - delay`, clamped to 0 before the animation starts. `dur` is clamped to at
least `0.001` to prevent division by zero. Tiny one-shot durations jump almost immediately to
`k=1`; tiny loop durations cycle extremely quickly.

| Module | Formula |
|---|---|
| `fade-in` | `Opacity *= 1 - (1-k)²` |
| `fade-out` | `Opacity *= 1 - k²` |
| `pulse` | `Scale *= 1 + amp·sin(k·2π)`. Frequency is fixed at one full cycle per `Duration`; `Param` changes amplitude only. |
| `blink` | `Opacity *= 1 - depth·0.5·(1 + sin(k·2π))`, ranging from `[1-depth, 1]`. |
| `flash` | `Opacity *= 1 - depth·sin(k·π)`. Unlike blink, this uses π, not 2π: opacity is full at `k=0` and `k=1`, with the dip at `k=0.5`. |
| `shake` | `Dx += amp·(1-k)·sin(k·8π)`. `8π` produces four full oscillations, while amplitude decays linearly to zero. |
| `slide-in-left`/`slide-in-top`/`fade-in-up` | Offset by `d·(1-easeOut(k))` on the relevant axis, plus `Opacity *= easeOut(k)`. |
| `pop` | EaseOutBack: `x=k-1; back=1 + c3·x³ + c1·x²` with `c1=1.70158`, `c3=2.70158`, the standard constants for a 10% overshoot. `Scale *= from + (1-from)·back`, converging exactly to 1. Also applies `Opacity *= easeOut(k)`, so pop begins slightly transparent. |

`pulse`, `blink`, `flash`, and `shake` hard-code the number of oscillations within a cycle.
`Duration` stretches the entire cycle; it does not change that internal count independently.

## Styles

Not applicable directly. The registry operates through `Style.Animation` and `Animated` and has no
visual style of its own.

## Methods

| Method | Returns | Description |
|---|---|---|
| `static Register(string name, IUiAnimation anim)` | `void` | Register a module; a custom module **overrides** a built-in with the same name. |
| `static Get(string name)` | `IUiAnimation` | Get a module by name, or `null`. |
| `static EaseOut(float k)` | `float` | Easing helper for custom modules. |
| `static EaseIn(float k)` | `float` | Easing helper. |
| `static EaseInOut(float k)` | `float` | Easing helper. |

`IUiAnimation.Apply(ref AnimFx fx, float k, AnimSpec spec)` is the module contract. `k` is a
normalized phase from 0..1: one-shots clamp and stay at 1, while loops use a sawtooth phase. The
module mutates `fx`.

## Events

Not applicable. Animation modules have no callbacks; they are evaluated passively every frame.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Custom modules | Yes | `Animations.Register(name, module)`. |
| Code attachment | Yes | Use `Animated`. |
| String attachment | Yes | Use `Style.Animation` or theme JSON; see StyleAnimation. |
| Rotation | No | Rotation is only available on `Icon.Rotation`; it is not part of `AnimFx`. |
| Child overlay layers | No | Dropdowns, menus, and tooltips do not animate with the owning element. |


---

[Table of contents](../index_en.md) - Animations | Previous: [Theme — themes and theme files](../03-themes/01_theme_en.md) | Next: [Animated](02_animated_en.md) | [Русский](01_animations_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
