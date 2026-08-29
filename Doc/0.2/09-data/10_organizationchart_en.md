![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.15` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [Timeline](09_timeline_en.md) | Next: [Button](../10-overlays/01_button_en.md) | [Русский](10_organizationchart_ru.md)

---

# OrganizationChart

A hierarchy diagram: node cards, connector lines, collapsible subtrees and node selection. Grows
downwards (the classic org chart) or to the right.

`RimUI.Components.OrganizationChart : UiElement`

## How it differs from Tree

`Tree` is a **list** of rows indented by depth: the height grows, the width stays fixed.
`OrganizationChart` is a **two-dimensional** layout: levels run across, siblings run along. The
width grows fast, which is why the chart is almost always wrapped in a `ScrollBox` with horizontal
scrolling.

The component **does not scroll on its own**: it honestly reports its full size, and clipping and
scrolling are left to the `ScrollBox` — otherwise two scrollers would fight over the wheel.

## Example

```csharp
var boss = new OrgChartNode("Governor") { Icon = Icons.Gear };
var supply = new OrgChartNode("Supply");
supply.Sub(new OrgChartNode("Cook")).Sub(new OrgChartNode("Farmer"));
boss.Sub(supply).Sub(new OrgChartNode("Defence"));

var chart = new OrganizationChart { OnSelect = n => Log.Message(n.Text) };
chart.Roots.Add(boss);

var box = new ScrollBox(chart) { Horizontal = true, Style = { Height = 230f } };
```

A custom card through a template:

```csharp
new OrganizationChart
{
    NodeTemplate = n =>
    {
        var col = new FlexBox(Axis.Column) { Style = { Gap = 2f, AlignItems = AlignItems.Center } };
        col.Add(new Text(n.Text));
        col.Add(new Text(n.Value as string) { Style = { Text = new TextStyle { Size = FontSize.Tiny } } });
        return col;
    }
};
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Roots` | `List<OrgChartNode>` (readonly) | empty | Roots of the chart. There may be several — a forest is drawn in one row. |
| `Orientation` | `OrgChartOrientation` | `Vertical` | Growth direction: `Vertical` (down) or `Horizontal` (right). |
| `NodeTemplate` | `Func<OrgChartNode,UiElement>` | `null` | Card template. Called **once per node**; the result is cached by key. |
| `Selectable` | `bool` | `true` | Selection as a whole: `false` = cards are neither highlighted nor clickable. |
| `MultiSelect` | `bool` | `false` | Multiple selection: a click adds or removes a node. |
| `SelectChildren` | `bool` | `false` | A click selects the node and its whole subtree. Requires `MultiSelect`. |
| `SiblingGap` | `float` | `14` | Gap between neighbouring subtrees. |
| `LevelGap` | `float` | `34` | The corridor between levels, where the lines run. |
| `NodeMinWidth` | `float` | `74` | Minimum card width. |
| `OnSelect` | `Action<OrgChartNode>` | `null` | A node was selected. |
| `OnToggle` | `Action<OrgChartNode,bool>` | `null` | A subtree was collapsed or expanded (the second argument says whether it is now collapsed). |
| `Disabled` | `bool` | `false` | Disabled state. |

### OrgChartNode

| Field | Type | Description |
|---|---|---|
| `Key` | `string` | Stable key of the node (`null` = path built from the texts). |
| `Text` | `string` | Card caption. |
| `Icon` | `int` | Icon before the caption (`-1` = none). |
| `Content` | `UiElement` | Arbitrary card content (takes precedence over `Text` and `NodeTemplate`). |
| `Collapsible` | `bool` | Show the collapse button (default `true`). |
| `Selectable` | `bool` | Whether this node can be selected (default `true`). |
| `Children` | `List<OrgChartNode>` | Child nodes; the `Sub` method is the convenient way to add them. |
| `Value` | `object` | Payload. |

## Behaviour

**Layout in two passes.** First the extent of a subtree is computed — the larger of "its own card"
and "the sum of the children's subtrees plus gaps". Then space is handed out to the children in
order, and the parent is centred **between the first and the last child**, not over the middle of
the whole subtree: that way the node sits exactly above its own group even when the children's
subtrees differ wildly in size.

**A level's size** follows the largest card on that level, so the lines between levels are always
the same length.

**Collapsed state is stored, not expanded state.** The chart is fully expanded by default, and an
empty set means exactly that.

**`SelectChildren` requires `MultiSelect`.** Without it only one node can be selected, and there is
simply nowhere to record "the whole subtree" — the flag silently does nothing.

**The collapse button** is a circle on the bottom edge of the card. Its click area is wider than
the drawing: hitting a 16-pixel circle with the mouse is otherwise awkward.

## Styles

| Slot | Purpose |
|---|---|
| `orgchart/node` | The node card. |
| `orgchart/node_hover` | Under the cursor. |
| `orgchart/node_selected` | A selected card. |
| `orgchart/line` | Connector colour (colour only). |
| `orgchart/toggle` | The collapse circle: `Background`/`BorderColor`, and `Text.Color` for the chevron. |

## Methods

| Method | Returns | Description |
|---|---|---|
| `GetSelected()` | `OrgChartNode` | The last selected node (single mode) or the last toggled one. |
| `GetSelectedKeys(ctx)` | `IEnumerable<string>` | Keys of the selected nodes. In single mode, the key of the selected node if there is one. |
| `ClearSelection(ctx)` | — | Clear the selection entirely. |
| `SetCollapsed(ctx, nodeKey, collapsed)` | — | Collapse or expand a node programmatically, by the same key a click uses. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnSelect` | `Action<OrgChartNode>` | the node | A card was clicked. With `SelectChildren`, once, for the root of the subtree. |
| `OnToggle` | `Action<OrgChartNode,bool>` | the node, whether collapsed | The collapse circle was clicked. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedValue` | The same as `OnSelect`, in generic form. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The cursor entered or left the chart bounds. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left / right click on the chart. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Disabled` changed. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Styling | Yes | Slots for the card, the lines and the collapse button. |
| Sprite frame | Yes | Through the `orgchart/node` slot. |
| Animation | Yes | The common `Style.Animation` mechanism. |
| Disabled | Yes | Clicks and highlighting are turned off. |
| Hover fade | No | Card highlighting switches instantly. |
| Cursor | Yes | A hand over cards and the collapse button. |


---

[Table of contents](../index_en.md) - Data | Previous: [Timeline](09_timeline_en.md) | Next: [Button](../10-overlays/01_button_en.md) | [Русский](10_organizationchart_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
