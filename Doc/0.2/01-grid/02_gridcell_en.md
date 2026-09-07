![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Grid | Previous: [Grid](01_grid_en.md) | Next: [FlexBox](03_flexbox_en.md) | [Русский](02_gridcell_ru.md)

---

# GridCell

Wraps the content of one `Grid` cell and defines how many columns and rows it occupies. A GridCell
can also act as a drag-and-drop **slot**, allowing content to be moved between cells with the mouse.

`RimUI.Layout.GridCell : UiElement`

## Example

```csharp
var cell = new GridCell(4, rowSpan: 2).With(someContent);
grid.Cell(cell);

// Drag and drop: two slots in one group. Dropping onto an occupied slot swaps the items.
var slotA = new GridCell(4) { DragGroup = "inventory" }.With(itemA);
var slotB = new GridCell(4) { DragGroup = "inventory" };   // Empty slots participate too.
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `ColSpan` | `int` | `12` | Number of the 12 columns occupied by the cell; clamped to 1..12. |
| `RowSpan` | `int` | `1` | Number of rows spanned vertically. |
| `Content` | `UiElement` | `null` | Cell content. |
| `DragGroup` | `string` | `null` | Drag-and-drop group ID; `null` disables dragging. |
| `DragOut` | `bool` | `true` | Whether content can be dragged out of this cell. |
| `DropIn` | `bool` | `true` | Whether this cell accepts drops; an occupied slot swaps content. |
| `DragCopy` | `bool` | `false` | Copy instead of move. Requires `CopyOf`; otherwise silently falls back to moving. |
| `CopyOf` | `Func<UiElement, UiElement>` | `null` | Clone factory used by `DragCopy`. |

## Styles

Uses regular `Style` for background, border, and padding. Cell height is based on its content unless
`Style.Height` is set explicitly, which is rarely necessary.

## Methods

| Method | Returns | Description |
|---|---|---|
| `GridCell(int colSpan = 12, int rowSpan = 1)` (constructor) | — | Create a cell with the given spans. |
| `With(UiElement content)` | `GridCell` | Set content and return this cell for chaining. |
| `SetContent(UiElement content)` | `void` | Replace content; also used by drag-and-drop swaps. |

The exact drag threshold is a squared mouse displacement greater than 25 units²:
`dx² + dy² > 25`, or exactly **5 units** of movement. Until that threshold is crossed, the action
remains a normal click and clickable cell content such as buttons continues to work. No drag
session is created until the pointer has moved far enough.

## Events

| Event | Type | Parameters | Fired when |
|---|---|---|---|
| `OnDragStart` | `Action<UiElement>` | dragged element | Content is picked up after crossing the movement threshold. |
| `OnDragOut` | `Action<UiElement>` | dragged element | Content is moved out of the cell, after removal. |
| `OnDropIn` | `Action<UiElement>` | accepted element | The cell accepts an element, after assigning `Content`. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style (`Style`) | Yes | Background, border, radius, and shadow work normally. |
| Sprite frame | Yes | Through `Style.Sprite`. |
| Animation (`Style.Animation`) | Yes | Uses the shared animation path. |
| Disabled | No | GridCell has no disabled state. |
| Hover fade | No | Compatible-slot highlighting during drag is immediate. |
| Drag and drop | Yes | A single-item **slot**; dropping onto an occupied slot swaps items. |


---

[Table of contents](../index_en.md) - Grid | Previous: [Grid](01_grid_en.md) | Next: [FlexBox](03_flexbox_en.md) | [Русский](02_gridcell_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
