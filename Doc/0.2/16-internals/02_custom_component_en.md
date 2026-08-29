![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.15` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Internals & extending | Previous: [Architecture: how the framework works inside](01_architecture_en.md) | [Русский](02_custom_component_ru.md)

---

# Writing your own component

There are plenty of ready-made components, but sooner or later you will need something of your
own. This page walks through a complete working component — not pseudocode: the class below
compiles and runs exactly as written.

Our example is **Spoiler** — a header that reveals its content when clicked. It is small, yet it
exercises everything worth knowing: the three phases of a frame, styling and theming, internal
state, input, the cursor and callbacks.

It helps to understand the frame model first; it is described on the
[Architecture](01_architecture_en.md) page.

## The complete code

```csharp
using RimUI.Core;
using RimUI.Elements;
using RimUI.Rendering;
using RimUI.Styling;

namespace MyMod.Ui
{
    public sealed class Spoiler : UiElement
    {
        public string Title { get; set; } = "";
        public UiElement Content { get; set; }
        public float HeaderHeight { get; set; } = 26f;
        public bool Disabled { get; set; }
        public System.Action<bool> OnToggle { get; set; }

        // --- state that survives between frames (the ID store) ---
        private sealed class SpoilerState : WidgetState
        {
            public bool Open;
        }

        private SpoilerState St(UiState s)
            => s != null ? s.GetOrCreate(s.MakeId(StateKey) + "/spoiler", () => new SpoilerState()) : null;

        public bool IsOpen(LayoutContext ctx)
        {
            SpoilerState st = ctx != null ? St(ctx.State) : null;
            return st != null && st.Open;
        }

        public void SetOpen(LayoutContext ctx, bool open)
        {
            SpoilerState st = ctx != null ? St(ctx.State) : null;
            if (st != null) st.Open = open;
        }

        public override bool EffectiveDisabled => Disabled;

        // Buffers: reused between frames so the component does not allocate garbage.
        private readonly Style _headerStyle = new Style { Radius = BorderRadius.Small };
        private TextStyle _titleTs = new TextStyle { VAlign = VerticalAlign.Middle, Wrap = false };

        // ---------- phase 1: how much room is needed ----------
        public override SizeF Measure(SizeF available, LayoutContext ctx)
        {
            ResolveStyle(ctx);
            Style s = RS;
            Thickness m = EffMargin(s);
            Thickness pad = MeasurePadding(s);

            float innerW = available.Width - pad.Horizontal - m.Horizontal;
            float h = HeaderHeight;

            if (Content != null && IsOpen(ctx))
            {
                SizeF c = Content.Measure(new SizeF(innerW, available.Height), ctx);
                h += s.Gap + c.Height;
            }

            Desired = new SizeF(available.Width, h + pad.Vertical + m.Vertical);
            return Desired;
        }

        // ---------- phase 2: hand out the space ----------
        public override void Arrange(RectF finalRect, LayoutContext ctx)
        {
            Bounds = finalRect;
            RectF inner = ContentRect(finalRect);

            if (Content == null || !IsOpen(ctx)) return;

            float top = inner.Y + HeaderHeight + RS.Gap;
            Content.Arrange(new RectF(inner.X, top, inner.Width, inner.Bottom - top), ctx);
        }

        // ---------- phase 3: draw and handle input ----------
        public override void Emit(DrawList list, LayoutContext ctx)
        {
            Theme t = ctx.Theme ?? Theme.Default;
            UiState s = ctx.State;
            SpoilerState st = St(s);

            RectF inner = ContentRect(Bounds);
            var header = new RectF(inner.X, inner.Y, inner.Width, HeaderHeight);

            bool hover = !Disabled && s != null && s.HoverOn(header);
            _headerStyle.Background = Fill.Solid(hover ? t.MenuItemHover : t.SurfaceAlt);
            _headerStyle.BorderColor = t.BorderColor;
            _headerStyle.BorderWidth = BorderWidth.Small;
            list.Add(DrawCommand.Background(header, _headerStyle));
            list.Add(DrawCommand.Border(header, _headerStyle));

            bool open = st != null && st.Open;
            var chev = new RectF(header.X + 6f, header.Y + (HeaderHeight - 12f) * 0.5f, 12f, 12f);
            list.Add(DrawCommand.ImagePart(chev, Icons.AtlasKey,
                Icons.Rect(open ? Icons.ChevronDown : Icons.ChevronRight),
                Disabled ? t.ControlDisabled : t.TextMuted));

            _titleTs.Color = Disabled ? t.ControlDisabled : t.TextColor;
            list.Add(DrawCommand.Label(new RectF(chev.Right + 6f, header.Y,
                                                 header.Right - chev.Right - 10f, header.Height),
                                       Title ?? "", _titleTs));

            if (!Disabled && s != null)
            {
                if (hover) s.RequestCursor(CursorKind.Hand);
                if (s.ClickOn(header) && st != null)
                {
                    st.Open = !st.Open;
                    if (OnToggle != null) OnToggle(st.Open);
                }
            }

            if (open && Content != null) Content.EmitStyled(list, ctx);
        }
    }
}
```

Using it is no different from any built-in component:

```csharp
var body = new FlexBox(Axis.Column) { Style = { Gap = 6f } };
body.Add(new Text("First line of content"));
body.Add(new Text("Second line"));

var sp = new Spoiler
{
    Key = "settings_advanced",       // needed when the element is recreated; see below
    Title = "Advanced",
    Content = body,
    Style = { Gap = 6f, Padding = new Thickness(4f) }
};
```

## Part by part

### Inheritance and the three phases

The component derives from `UiElement` and overrides three methods — `Measure`, `Arrange` and
`Emit`. The framework calls them in order; you never call them yourself.

`Measure` **must** set `Desired`, and `Arrange` **must** set `Bounds`. The parent relies on those
fields; forgetting them gives you an element of zero size, or one parked in the corner of the
window.

Note that the size depends on state: a collapsed spoiler occupies only the header height and does
not measure its content at all. That is normal and typical — in immediate mode "collapsed" quite
literally means "not accounted for in this frame".

### Style: use `RS`, not `Style`

`Style` is what the user of the component set. `RS` is the **resolved** style: theirs, filled in
with theme defaults. Inside a component always work with `RS`, and call `ResolveStyle(ctx)` in
`Measure` before touching it for the first time — the theme can change on the fly, so the merge is
recomputed every frame.

Three helpers on the base class save you from doing spacing arithmetic by hand:

- `EffMargin(s)` — the outer margin, sprite frame taken into account;
- `MeasurePadding(s)` — the inner padding for `Measure`;
- `ContentRect(rect)` — the content rectangle: `rect` minus margin, padding and the sprite edge.

`Measure` and `ContentRect` must agree with each other: compute a size without the padding while
drawing inside `ContentRect`, and the content will not fit.

### Theming: take colours from the palette

The component hardcodes no colours — it takes them from the theme: `t.SurfaceAlt`,
`t.BorderColor`, `t.TextColor`, `t.TextMuted`, `t.MenuItemHover`, `t.ControlDisabled`, `t.Accent`.
That way your component follows whatever theme the user has and looks like it belongs.

> Built-in components also use **slots** (`t.Slot("button")`), named sets of defaults. For an
> external component that route is less convenient: slots are declared in a shared registry before
> themes are built, which makes initialisation order matter. The palette above, plus your own
> public `Style` fields for whatever the user should be able to configure, is simpler and safer.

### State: the ID store and `StateKey`

Whether the spoiler is open is internal widget mechanics, so it lives in the ID store:

```csharp
private sealed class SpoilerState : WidgetState { public bool Open; }

s.GetOrCreate(s.MakeId(StateKey) + "/spoiler", () => new SpoilerState())
```

The `"/spoiler"` suffix keeps your state apart from anyone else's under the same key.

**Reach for state through `StateKey`, not through `Key`.** `StateKey` is the `Key` when one is set,
otherwise an automatic key tied to the element object itself. The difference matters:
`MakeId(null)` hands out numbers BY CALL COUNT, and a component touches its state twice per frame —
in `Measure` and again in `Emit`. With `Key = null` it would read the state under one number and
write it under another, which looks exactly like "the component does not work without a key".

**If the user does set a `Key`, it must be stable across frames.** Something like `"sp" + i` inside
a loop is fine as long as `i` is stable. A key derived from the time, a random number or the
current sort order is a source of bugs that are hard to pin down: state "disappears", the spoiler
snaps shut on its own.

Note the `IsOpen(ctx)` / `SetOpen(ctx, open)` pair. The state lives in `UiState`, but the user of
your component should not have to reach in there — give them methods instead. The whole framework
follows this convention (`GetSelected()`, `SetValue()` and friends).

### Input

Input is handled in `Emit`, because only there are the final `Bounds` known:

```csharp
bool hover = !Disabled && s != null && s.HoverOn(header);
if (s.ClickOn(header)) { ... }
```

Useful members of `UiState`: `HoverOn(rect)`, `ClickOn(rect)`, `RightClicked`, `MouseDown`,
`MousePosition`, `ScrollDelta`, `CtrlDown`, `ShiftDown`, and for dragging `Capture(id)` /
`IsCaptured(id)` / `ReleaseCapture()`.

Check `Disabled` **before** reacting to input, not only when drawing: a disabled element that
still responds to clicks is a common mistake.

### The cursor

One line makes the component feel alive:

```csharp
if (hover) s.RequestCursor(CursorKind.Hand);
```

The element under the mouse files a request; the deepest one wins, and the cursor is applied once
per frame. See the styling page for details.

### Children

A child goes through the same three phases: `Content.Measure(...)`, `Content.Arrange(...)` and
`Content.EmitStyled(list, ctx)`.

For children call **`EmitStyled`**, not `Emit`: it is the wrapper that additionally applies
style-driven animation, user event callbacks and the cursor request. `Emit` is the method you
override; `EmitStyled` is the one you call on someone else's elements.

### Events

The `OnToggle` callback is the component's own event. On top of that, every `UiElement` already
supports a common event set with no extra work:

```csharp
sp.Events.HoverEnter = (sender, data) => Log.Message("entered");
sp.Events.Click = (sender, data) => Log.Message("click at " + data.MousePosition.X);
```

The `Events` block is created lazily, so elements without subscribers cost nothing.

If your component has a disabled state, override `EffectiveDisabled` — the `DisabledChanged` event
and whether the element files a cursor request both depend on it.

## Checklist

- [ ] `Measure` sets `Desired`, `Arrange` sets `Bounds`.
- [ ] `ResolveStyle(ctx)` is called at the top of `Measure`; everything after that uses `RS`.
- [ ] Spacing goes through `EffMargin` / `MeasurePadding` / `ContentRect`, and `Measure` agrees
      with where you actually draw.
- [ ] Colours come from the theme, not from constants.
- [ ] State lives in the ID store under `StateKey` (not `Key`), exposed through methods.
- [ ] Input is handled in `Emit`, with a `Disabled` check.
- [ ] Children are drawn with `EmitStyled`, not `Emit`.
- [ ] Buffers (`Style`, `TextStyle`, lists) are created once in fields, not per frame.

## Common mistakes

**Forgetting `Desired` or `Bounds`.** The element collapses to nothing or draws in the wrong
place. The single most common first-component mistake.

**Creating a new `Style` every frame.** A draw command stores a **reference** to the style, not a
copy. Reuse one buffer across rows in a loop and every row ends up with the last row's style.
Either keep one buffer per row, or do not mutate it between commands.

**Reaching for state through `Key` instead of `StateKey`.** With no key set, `MakeId(null)` returns
different numbers in `Measure` and in `Emit`: the component reads one state and writes another. The
symptom is "it only works if I set a Key".

**An unstable `Key`.** If a key is set but changes between frames, state is lost: scrolling jumps,
the spoiler closes, `HoverEnter` fires nonstop.

**Doing data work in `Emit`.** A map scan, a file read or LINQ over a large list inside frame
building runs 60 times per second. Compute it ahead of time and hand the component a ready value
or a `Func<T>` delegate.

**Unity inside a component.** Components live in the core, which knows nothing about Unity. If you
find yourself needing `UnityEngine`, the task can most likely be expressed as a draw command
instead of drawing directly.


---

[Table of contents](../index_en.md) - Internals & extending | Previous: [Architecture: how the framework works inside](01_architecture_en.md) | [Русский](02_custom_component_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
