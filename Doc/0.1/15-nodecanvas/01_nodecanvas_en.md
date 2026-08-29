![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - NodeCanvas | Previous: [HeatmapChart](../14-charts/07_heatmapchart_en.md) | [Русский](01_nodecanvas_ru.md)

---

# NodeCanvas

A canvas of draggable nodes and curved links. Higher-Z nodes draw above overlapping lower-Z nodes. Links run from the right output of one node to the left input of another and obey per-block type limits. Canvas state can be saved and loaded as JSON.

`RimUI.Components.NodeCanvas : UiElement`

## Example

```csharp
var canvas = new NodeCanvas { Style = { Height = 300f } };

canvas.LinkTypes.Add(new CanvasLinkType("Flow", new ColorRGBA(0.75f, 0.62f, 0.39f, 1f)));
canvas.LinkTypes.Add(new CanvasLinkType("Signal", Blue));

string flow = "Flow", signal = "Signal";
canvas.BlockTypes.Add(new CanvasBlockType("Source").Allow(flow, 0, -1).Allow(signal, 0, 1));
canvas.BlockTypes.Add(new CanvasBlockType("Process").Allow(flow, -1, -1).Allow(signal, 1, 1));
canvas.BlockTypes.Add(new CanvasBlockType("Sink").Allow(flow, -1, 0).Allow(signal, 1, 0));

canvas.Groups.Add(new CanvasGroup("Core", new ColorRGBA(0.75f, 0.62f, 0.39f, 1f)));

canvas.Nodes.Add(new CanvasNode { Id = "n1", Title = "Source", Type = "Source",
    Group = "Core", Pos = new Vec2(30f, 40f) });
canvas.Nodes.Add(new CanvasNode { Id = "n2", Title = "Process", Type = "Process",
    Group = "Core", Pos = new Vec2(260f, 110f) });
canvas.Links.Add(new CanvasLink { Type = flow, From = "n1", To = "n2" });

canvas.ConfirmDelete = (node, accept) =>
    ConfirmWindow.OfText("Delete node?", "Delete node '" + node.Title + "'?", accept).Show();
canvas.OnConfigure = node => OpenNodeConfig(canvas, node);
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `BlockTypes` | `List<CanvasBlockType>` | empty | Block types and their link rules. |
| `LinkTypes` | `List<CanvasLinkType>` | empty | Link names and colors. |
| `Groups` | `List<CanvasGroup>` | empty | Group names and node-border colors. |
| `AllowCreate` | `bool` | `true` | Allows creating nodes by right-clicking empty space. |
| `Nodes` | `List<CanvasNode>` | empty | Mutable node state. |
| `Links` | `List<CanvasLink>` | empty | Mutable link state. |
| `NodeHeight` | `float` | `30` | Node height. |
| `LinkThickness` | `float` | `1.5` | Link line width. |
| `ArrowLength` | `float` | `10` | Arrow at curve midpoint; zero hides it. Requires GL rendering. |
| `PortRadius` | `float` | `3.5` | Port radius; zero hides ports. |
| `AddBlockLabel` | `string` | `"+"` | Localizable add-node menu label. |
| `DeleteLinkLabel` | `string` | `"x"` | Localizable delete-link menu label. |
| `NewBlockTitle` | `string` | `"block"` | Default new-node title. |

## Data types

`CanvasBlockType(string name)` has a color and fluent `Allow(string linkType, int maxIn, int maxOut)` rules, where `-1` is unlimited and `0` disallows the direction. `RuleFor` reads a rule.

`CanvasLinkType` stores link name and color. `CanvasGroup` stores group name and border color.

`CanvasNode` stores `Id`, `Title`, `Type`, `Group`, `TitleColor`, `Pos`, and `Z`. A null `Type` cannot link. A null `Group` uses the default gray border.

`CanvasLink` stores `Type`, `From`, and `To`. Output is the right node edge; input is the left.

## Exact ports and Bezier geometry

Output is `(node.Pos.X + NodeWidth, node.Pos.Y + NodeHeight / 2)`. Input is `(node.Pos.X, node.Pos.Y + NodeHeight / 2)`. `NodeWidth` fits title text and icon buttons with a minimum of 90.

Links are cubic Bezier curves. Control points extend horizontally from ports by `dx = abs(deltaX) * 0.5` clamped to `[24, 120]`. They do not follow the connection normal. Tessellation uses `distance / 2.5` segments clamped to `[24, 128]`.

The arrow sits at parametric `t = 0.5`, not half arc length. Direction is approximated from points at `t = 0.45` and `t = 0.55`. It draws only when GL is available and `ArrowLength > 0`.

## Styles

The root canvas uses normal container styling. Nodes and links are procedural and do not use theme slots.

## Methods

| Method | Returns | Description |
|---|---|---|
| `Serialize()` | `string` | Serializes `Nodes` and `Links` to flat JSON. |
| `Deserialize(string json, out string error)` | `bool` | Loads state; false on error. |
| `SetStyle(string path, string value)` | - | Applies only to the canvas root, not nodes or links. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Changed` | `Action` | - | Drag completes, or a node/link is created or deleted. |
| `OnConfigure` | `Action<CanvasNode>` | node | The settings button is clicked; null hides it. |
| `ConfirmDelete` | `Action<CanvasNode, Action>` | node and accept callback | Before deletion. Show your own dialog and call `accept` to proceed. Null deletes immediately. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the canvas bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Click on the canvas as a whole. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the canvas. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

## Mouse controls

- Drag a node body to move it freely without grid snapping.
- The arrow button opens available outgoing link types, respecting `MaxOut`. Choosing one enters linking mode, highlights compatible targets, and creates a link on target click. Right-click cancels.
- Right-click a link to open its delete menu.
- Right-click empty space to add a node when `AllowCreate` is enabled. IDs auto-increment, and `OnConfigure` opens immediately when supplied.
- Use the node close button to delete through `ConfirmDelete` or immediately when no callback exists.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Partly | Canvas background and border only. |
| Sprite frame | Yes (canvas only) | Through root `Style.Sprite`. |
| Animation | Yes | Root `Style.Animation`. |
| Save / load | Yes | Flat JSON through `Serialize` and `Deserialize`. |
| Disabled | No | No disabled state. |
| Hover fade | No | Link-target highlighting is immediate. |


---

[Table of contents](../index_en.md) - NodeCanvas | Previous: [HeatmapChart](../14-charts/07_heatmapchart_en.md) | [Русский](01_nodecanvas_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
