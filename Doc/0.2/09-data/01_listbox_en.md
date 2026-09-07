![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [Stepper](../08-containers/04_stepper_en.md) | Next: [OrderList&lt;T&gt;](02_orderlist_en.md) | [Русский](01_listbox_ru.md)

---

# ListBox&lt;T&gt;

A selectable list. Click selects a row, Ctrl-click toggles one row, Shift-click selects a range, and double-click can run a custom action.

`RimUI.Components.ListBox<T> : UiElement` (generic, not sealed)

## Example

```csharp
var lb = new ListBox<string> { Style = { Height = 170f } };
lb.Items.AddRange(new[] { "Cooking", "Construction", "Plants" });
lb.OnSelect = i => { /* row index, or -1 when cleared */ };
lb.OnActivate = i => { /* double-click */ };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `Items` | `List<T>` | empty | List rows. |
| `Display` | `Func<T,string>` | `null` (`item.ToString()`) | Row text. |
| `OnSelect` | `Action<int>` | `null` | Selection callback. |
| `OnActivate` | `Action<int>` | `null` | Double-click callback. |
| `RowHeight` | `float` | `24` | Row height. |
| `Disabled` | `bool` | `false` | Disables the list. |

## Exact anchor and modifier behavior

`Anchor` changes only on a plain click or Ctrl-click. Shift-click never moves it, allowing repeated ranges from the same origin. If Shift is used before an anchor exists (`Anchor == -1`), it behaves as a plain single click and establishes the anchor.

Ctrl-click toggles only the clicked row. Deselecting it sets the single-selection `Selected` value to `-1` even when other rows remain in the multi-selection set. Double-click detection belongs to `ListBox` and is reused by `OrderList` and `PickList`.

## Performance on long lists

`ListBox` is **virtualized**: each frame only the rows inside the visible area are walked (the
range is derived from the scroll position and the viewport height, with one row of slack on each
side). The per-frame work does not depend on the list length — 10 rows and 100,000 rows cost the
same.

What this does not change: you still build the `Items` list yourself. If it is rebuilt every frame,
that is where the cost will be — see the Architecture page, the performance section.

> For reference: a measurement on 10,000 rows showed 0.07 ms of frame building against a 16.7 ms
> budget — and that was before virtualization, with nothing but the invisible-row skip. `DataTable`,
> `Tree` and `TreeTable` skip invisible rows too but do not narrow the range: there the gain did not
> justify the risk to selection, sorting and row order.

## Styles

Slots: `list/frame`, `list/row`, `list/row_disabled`, `list/row_hover`, `list/row_selected`, and `list/scrollbar`.

## Methods

Only the parameterless `ListBox<T>()` constructor is public.

| Method | Returns | Description |
|---|---|---|
| `GetSelected()` | `int` | Active row index, or `-1` when none; `-1` before the first render. |
| `SetSelected(int index)` | `void` | Selects a row programmatically, or clears with `-1`. Updates internal state for the next frame and calls `OnSelect`. |
| `SetStyle(string path, string value)` | `void` | Applies the path to the root style. Unknown paths and values are ignored. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnSelect` | `Action<int>` | row index or `-1` | A row is clicked. |
| `OnActivate` | `Action<int>` | row index | A row is double-clicked. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | After selection logic and `OnSelect`. Value is the selected `T`, or `null`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | `list/*` slots. |
| Sprite frame | Yes | `list/frame`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | Yes | Disables clicks and uses row styling. |
| Hover fade | No | Rows use immediate `HoverOn`. |


---

[Table of contents](../index_en.md) - Data | Previous: [Stepper](../08-containers/04_stepper_en.md) | Next: [OrderList&lt;T&gt;](02_orderlist_en.md) | [Русский](01_listbox_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
