![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Containers | Previous: [Tabs](03_tabs_en.md) | Next: [ListBox&lt;T&gt;](../09-data/01_listbox_en.md) | [Русский](04_stepper_ru.md)

---

# Stepper

A step-by-step wizard: a header strip showing progress, the content of the active step and
navigation buttons. On the last step "Next" turns into "Finish".

`RimUI.Components.Stepper : UiElement`

## Two modes

**Linear** (the default) — you can only move in order: one step forward, and back through steps
already visited. Arbitrary jumps forward are refused, because a wizard usually collects data and
step N+2 makes no sense without step N+1.

**Free** (`Linear = false`) — any header can be clicked. For wizards whose steps are independent:
settings sections, tabs with progress.

## Example

```csharp
var wizard = new Stepper
{
    BackLabel = "Back",
    NextLabel = "Next",
    FinishLabel = "Finish",
    OnFinish = () => ApplySettings(),
    Style = { Height = 190f }
};

wizard.Steps.Add(new StepperItem("Start", introPanel));
wizard.Steps.Add(new StepperItem("Name", namePanel)
{
    CanLeave = () => !string.IsNullOrEmpty(_name)     // will not let you leave without a name
});
wizard.Steps.Add(new StepperItem("Summary", summaryPanel));

// free mode without buttons
new Stepper { Linear = false, ShowNav = false };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Steps` | `List<StepperItem>` (readonly) | empty | The steps of the wizard. |
| `Active` | `Func<int>` | `null` | Active step source (index **from 0**). `null` = the step lives in the ID store. |
| `OnChange` | `Action<int>` | `null` | The active step changed. |
| `OnFinish` | `Action` | `null` | "Finish" was pressed on the last step. |
| `Linear` | `bool` | `true` | In order only: jumping forward past a step is refused. |
| `ShowNav` | `bool` | `true` | "Back" / "Next" buttons below the content. |
| `BackLabel`, `NextLabel`, `FinishLabel` | `string` | `"<"`, `">"`, `"OK"` | Button captions. You supply them — the core knows nothing about languages. |
| `HeaderHeight` | `float` | `0` (auto) | Height of the header strip. `0` = derived from the tallest caption at the actual cell width. |
| `MarkerSize` | `float` | `24` | Diameter of a step circle. |
| `NavHeight` | `float` | `30` | Height of the button strip. |
| `SectionGap` | `float` | `10` | Gap between the headers, the content and the buttons. |
| `Disabled` | `bool` | `false` | Disabled state. |

### StepperItem

| Field | Type | Description |
|---|---|---|
| `Title` | `string` | Caption under the circle. Wraps by words. |
| `TitleContent` | `UiElement` | `null` | Content of your own for the header instead of a caption; the circle with the step number stays in place. |
| `Content` | `UiElement` | The step's content. |
| `Icon` | `int` | Icon instead of the number in the circle (`-1` = number). |
| `Disabled` | `bool` | The step cannot be clicked. |
| `CanLeave` | `Func<bool>` | Whether the step may be left **forwards**. `null` = always. |

## Behaviour

**Validation applies forwards only.** `CanLeave` is checked when moving forward and when pressing
"Finish". The user must always be able to go back, otherwise an unfilled form becomes a trap with
no way out.

**`SetActive` does not apply validation** — it is a command from code, not a user click: the modder
knows what they are doing.

**A completed step is marked with a tick**, not a number: the number is no longer information
there, while a tick answers "what is done" at a glance. A custom icon (`StepperItem.Icon`)
overrides both.

**The connector between circles** is drawn in the accent colour over the completed part, so you see
not only "where I am" but "how much is behind".

**Headers split the width evenly.** That keeps the connectors the same length, so the strip does
not shift about as you move between steps with names of different lengths.

**The header strip height is computed automatically** — from the tallest caption at the actual cell
width. A fixed height would clip long names that need to wrap onto a second line.

**How far back you can go.** The component remembers the furthest step reached: in linear mode a
click can return to any step already visited, but no further.

## Styles

| Slot | Purpose |
|---|---|
| `stepper/marker` | The circle of an upcoming step. |
| `stepper/marker_active` | The circle of the current step; `Text.Color` is the colour of the number and the tick. |
| `stepper/marker_done` | The circle of a completed step. |
| `stepper/connector` | Colour of the line between circles (colour only). |
| `stepper/connector_done` | Colour of the completed part of the line. |

The navigation buttons are ordinary `Button`s, so they are themed through the `button` slot.

## Methods

| Method | Returns | Description |
|---|---|---|
| `GetActive()` | `int` | Index of the active step (from 0): the `Active` source if set, otherwise the cache from the last frame. |
| `SetActive(int index)` | — | Move to a step programmatically, bypassing `Linear` and `CanLeave`. Raises `OnChange`. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<int>` | step index (from 0) | Moving to another step: a header click, the buttons, or `SetActive`. |
| `OnFinish` | `Action` | — | "Finish" on the last step (after `CanLeave` passes). |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Step change by header click; the value is the `StepperItem` itself. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The cursor entered or left the bounds. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left / right click on the component. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Disabled` changed. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Styling | Yes | Marker and connector slots, plus the component's own `Style`. |
| Sprite frame | Yes | Through the `stepper/marker` slot and the component's `Style`. |
| Animation | Yes | The common `Style.Animation` mechanism. |
| Disabled | Yes | Both the headers and the navigation buttons are blocked. |
| Hover fade | No | Step states switch instantly. |
| Cursor | Yes | A hand over headers you are allowed to move to. |


---

[Table of contents](../index_en.md) - Containers | Previous: [Tabs](03_tabs_en.md) | Next: [ListBox&lt;T&gt;](../09-data/01_listbox_en.md) | [Русский](04_stepper_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
