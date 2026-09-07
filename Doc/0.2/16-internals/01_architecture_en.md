![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Internals & extending | Previous: [NodeCanvas](../15-nodecanvas/01_nodecanvas_en.md) | Next: [Writing your own component](02_custom_component_en.md) | [Русский](01_architecture_ru.md)

---

# Architecture: how the framework works inside

This page explains the model everything else is built on. You do not need it to assemble a window
out of ready-made components, but you do need it if you are writing your own component, chasing
strange behaviour, or running into performance limits.

## Immediate mode: the frame is recomputed

A conventional (retained) UI is made of long-lived objects: you create a button, put it into a
panel, and there it stays. Change the data and you must remember to call `button.SetText(...)`,
or the screen keeps showing the old value.

Here it works differently. **Every frame the interface is recomputed from the current data.**
Values are not copied into elements: components read them through delegates (`Func<T>`) at draw
time. The "data changed but the screen still shows the old thing" mismatch cannot happen: the
screen always shows what is in your data right now.

> **The element tree itself usually lives on.** The normal path is to build it once
> (`UiWindow.Root` is set when the window is created) and then leave it alone: there is no need to
> rebuild the objects, because the data is re-read every frame anyway. Recreating the tree is
> allowed, but then widget state has to be tied to explicit keys — see "Keys and state" below.

Hence the key consequence: **you never synchronise the UI with your data, because there is
nothing to fall out of sync.** No change subscriptions, no "please refresh me".

## The three phases of a frame

Every element goes through three phases, always in this order:

```
Measure  →  Arrange  →  Emit
```

**`Measure(available, ctx)` — "how much room do you need?"**
The element computes its desired size (`Desired`) and, if it has children, asks them the same.
This runs bottom-up: to size a panel you first have to size its contents. `available` is how much
room is on offer; the element is free to ask for less or for more.

**`Arrange(finalRect, ctx)` — "here is your space, lay out your children"**
The parent has decided which rectangle goes to whom. The element stores it in `Bounds` and hands
out space to its children. This runs top-down: space can only be divided once you know how much
of it there is.

**`Emit(list, ctx)` — "draw yourself"**
The element appends draw commands (background, border, text, image) to the `DrawList` and handles
input — clicks and hovering — right here. Input lives in this phase for a reason: only now are the
final `Bounds` known, and with them whether the cursor is actually over the element.

Why two passes instead of one? Because size depends on content while position depends on size.
A single pass could not centre an element without already knowing its width.

## Where state lives: the ID store

The scroll position, the text in an input field, the expanded branches of a tree — that is a
widget's internal machinery. It lives not in the element's fields but in the **ID store**
(`UiState`), a "keyed by a stable id" container:

```csharp
public sealed class MyState : WidgetState
{
    public float Scroll;
}

MyState st = ctx.State.GetOrCreate(ctx.State.MakeId(StateKey) + "/my", () => new MyState());
```

State then survives not only the frame but the recreation of the element — provided the key stayed
the same.

## Keys and state

`StateKey` (a property of `UiElement`) is **the explicit `Key` if one is set, otherwise an
automatic key tied to the element object itself**. Hence a simple rule:

| Situation | Is `Key` needed |
|---|---|
| The tree is built once and lives on (the normal path) | No, the automatic key is enough |
| Elements are recreated every frame | **Yes** — otherwise every new object starts with fresh state |
| State must survive the window or list being rebuilt | **Yes** |
| A predictable id is wanted (debugging, programmatic control) | Yes |

If a `Key` is set, it **must be stable across frames**: a key derived from the time, a random
number or the current sort order creates a brand new widget every frame, with fresh state — the
scroll jumps back to the top, the input field loses its caret, hover never fires.

The key must also be **unique among siblings**: two lists sharing `Key = "list"` in one window
share one state and scroll in lockstep.

> **Writing your own component? Reach for state through `StateKey`, never through `Key`.**
> `UiState.MakeId(null)` hands out numbers by call count, and a component touches its state twice
> per frame (in `Measure` and again in `Emit`) — with `Key = null` it would read the state under
> one number and write it under another. `StateKey` prevents exactly that.

**Important: your mod's data does not belong in the ID store.** Only the widget's internal
mechanics live there. The colonist list, the selected item, the text being edited — those are your
data and they stay with you. The framework neither copies nor caches them, which is exactly why
"reactivity" is free: change your list and the next frame draws the new one.

## Core and adapter

The code is split by a seam into two parts:

| Layer | Folders | Dependencies |
|-------|---------|--------------|
| Core | `Core`, `Layout`, `Styling`, `Rendering`, `Elements`, `Components`, `Animation` | plain C#, no Unity |
| Adapter | `Adapter` | `UnityEngine` + `Verse` |

The core **knows nothing about the game**: it does not draw, it describes what should be drawn,
appending commands like "a rectangle with this style" or "text inside this frame" to a `DrawList`.
The adapter executes those commands through `Widgets`/`GUI` and supplies the core with platform
details it cannot compute itself: text measurement (`ITextMeasurer`), sprite sizes
(`ISpriteMetrics`), time, input.

The practical takeaway: **a component is written in the core and never touches Unity.** If a
component needs `UnityEngine`, something is almost certainly being done on the wrong layer.

## Styles and themes

Every element has its own `Style` — whatever you set. But most fields you leave alone, and those
come from a **theme slot**: a named set of defaults (`"button"`, `"input/frame"`,
`"orgchart/node"`). Merging your style with the slot produces the **resolved style**, available
inside a component as `RS`.

The rule is simple: **what you set always beats the theme**, and what you leave unset comes from
the theme. That is why switching themes recolours the whole interface without overriding anything
you specified explicitly.

## What this means for performance

A frame is computed roughly 60 times per second, and everything that runs inside it runs 60 times
per second: value delegates, handlers, and your own content-building code if you keep it there.

**Do not do this:**

```csharp
// BAD: the list is rebuilt from scratch every frame
var items = new List<string>();
foreach (Pawn p in map.mapPawns.FreeColonists)
    items.Add(p.LabelShort);
listBox.Items = items;
```

That is 60 new lists per second and 60 walks over the colony — work spent on a result that is
almost always identical.

**Do this instead:**

```csharp
// GOOD: the list is yours and is rebuilt only when it ACTUALLY changed
if (_colonistsDirty)
{
    _items.Clear();
    foreach (Pawn p in map.mapPawns.FreeColonists) _items.Add(p.LabelShort);
    _colonistsDirty = false;
}
```

**For live values pass a delegate, not a copy.** Components accept `Func<T>`: the value is read at
draw time, on its own, and is always current.

```csharp
// GOOD: progress is read from your data every frame, no copy to keep updated
new ProgressBar(() => job.Progress);

// GOOD: the field shows your value and reports changes back
new InputText { Value = () => settings.Name, OnChange = v => settings.Name = v };
```

**Keep heavy work out of the frame.** Scanning every thing on the map, reading a file or parsing
XML inside UI-building code is that same scan 60 times per second. Compute ahead of time, cache,
and invalidate on an event.

**A note on `Events.Hover`.** It fires every frame while the cursor is over the element, unlike
`HoverEnter`/`HoverLeave` which fire once per transition. A heavy handler in `Hover` will cost you
FPS; if you need work "on hover", do it in `HoverEnter`.

## What the model does not have

Worth knowing up front, so you do not go looking:

- **No "redraw element X".** The whole frame is always redrawn.
- **No child-to-parent back references.** The tree is walked top-down.
- **No data-change events.** Data is re-read every frame, so there is nothing to notify about.
- **No positions before `Arrange`.** `Bounds` are meaningless until the second phase: asking for
  coordinates in `Measure` is pointless, they do not exist yet.

## Where to go next

The practical side is on the next page: [Writing your own component](02_custom_component_en.md)
walks through a complete working component line by line, covering input handling, theming, state
and events.


---

[Table of contents](../index_en.md) - Internals & extending | Previous: [NodeCanvas](../15-nodecanvas/01_nodecanvas_en.md) | Next: [Writing your own component](02_custom_component_en.md) | [Русский](01_architecture_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
