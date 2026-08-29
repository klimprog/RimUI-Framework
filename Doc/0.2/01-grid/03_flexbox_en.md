![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.15` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Grid | Previous: [GridCell](02_gridcell_en.md) | Next: [Style — style model](../02-styles/01_style_en.md) | [Русский](03_flexbox_ru.md)

---

# FlexBox

A flex container that lays out children along one axis, row or column, with growth, alignment, and
space distribution modeled after CSS flexbox. Use it inside a `GridCell` or as a standalone
container. FlexBox can also act as a drag-and-drop **list**.

`RimUI.Layout.FlexBox : UiElement`

## Example

```csharp
var row = new FlexBox(Axis.Row) { Style = { Gap = 8f, JustifyContent = JustifyContent.SpaceBetween } };
row.Add(new Text("Left"));
row.Add(Button.Make("Right"), grow: 0f);

// Drag and drop: items are inserted at the pointer position.
var list = new FlexBox(Axis.Column) { DragGroup = "queue" };
list.Add(itemA);
list.Add(itemB);
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Axis` | `Axis` (`Row`\|`Column`) | `Column` | Main layout axis. Row centers children vertically by default (`Style.AlignItems = Center`); Column stretches them (`Stretch`). An explicit `Style.AlignItems` overrides the default. |
| `Grow` | `List<float>` | empty | Per-child growth factors by index, equivalent to `flex-grow`; 0 means no growth. |
| `DragGroup` | `string` | `null` | Drag-and-drop group ID; `null` disables it. |
| `DragOut` | `bool` | `true` | Whether children can be dragged out of this zone. |
| `DropIn` | `bool` | `true` | Whether the zone accepts drops, inserted at the pointer position. |
| `DragCopy` | `bool` | `false` | Copy instead of move; requires `CopyOf`. |
| `CopyOf` | `Func<UiElement, UiElement>` | `null` | Clone factory used by `DragCopy`. |

## Exact free-space distribution (Grow)

Positive leftover space follows CSS `flex-grow` exactly: `mainSize[i] += leftover ×
(Grow[i] / sumGrow)`, where `leftover` is the free space after measuring every child at its desired
size. **Important:** if any child has `Grow > 0`, `JustifyContent` is ignored even when space is
left over. Growth and main-axis justification are mutually exclusive; `JustifyContent` only runs
when the sum of all `Grow` values is zero.

When the children need more room than is available, **every child shrinks in proportion to its own
desired size**, not its `Grow`: `mainSize[i] *= target/total`. There is no per-item `flex-shrink`.
In other words, `Grow` only distributes excess space; it does not control overflow behavior.

**`AlignSelf` overrides the container's `AlignItems` for one child**, but it is read from the
child's raw style (`Children[i].Style.AlignSelf`), not its theme-resolved style. A theme slot cannot
set `AlignSelf` on a child; only user code can.

`Style.JustifyContent` (`Start|Center|End|SpaceBetween|SpaceAround|SpaceEvenly`) distributes items
along the main axis when no `Grow` is active. `Style.Gap` controls spacing between children, and
`Style.AlignItems`/`AlignSelf` control the cross axis.

## Methods

| Method | Returns | Description |
|---|---|---|
| `FlexBox(Axis axis = Axis.Column)` (constructor) | — | Create a Row or Column container. |
| `Add(UiElement child, float grow = 0f)` | `FlexBox` | Add a child with a growth factor and return this FlexBox. |
| `RemoveChildAt(int i)` | `void` | Remove a child while keeping `Grow` in sync. |
| `InsertChild(int i, UiElement el, float grow = 0f)` | `void` | Insert a child and matching `Grow` entry at an index. |

The drag threshold is the same as GridCell: `dx² + dy² > 25`, exactly 5 units from the press point.
Until the threshold is crossed, normal clicks on children continue to work.

## Events

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `OnDragStart` | `Action<UiElement>` | dragged element | An item is picked up after crossing the movement threshold. |
| `OnDragOut` | `Action<UiElement>` | dragged element | An item is moved out, after removal. |
| `OnDropIn` | `Action<UiElement, int>` | accepted element, insertion index | The zone accepts an item. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style (`Style`) | Yes | Background, border, padding, and gap work normally. |
| Sprite frame | Yes | Through `Style.Sprite`. |
| Animation (`Style.Animation`) | Yes | Uses the shared animation path. |
| Disabled | No | FlexBox has no disabled state. |
| Hover fade | No | Compatible-zone highlighting during drag is immediate. |
| Drag and drop | Yes | A **list** zone; drops are inserted between existing children at the pointer position. |


---

[Table of contents](../index_en.md) - Grid | Previous: [GridCell](02_gridcell_en.md) | Next: [Style — style model](../02-styles/01_style_en.md) | [Русский](03_flexbox_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
