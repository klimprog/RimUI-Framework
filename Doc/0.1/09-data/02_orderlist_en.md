![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.1` · mod `0.1.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [ListBox&lt;T&gt;](01_listbox_en.md) | Next: [PickList&lt;T&gt;](03_picklist_en.md) | [Русский](02_orderlist_ru.md)

---

# OrderList&lt;T&gt;

A reorderable list with four buttons: move to start, up one position, down one position, or move to end. **It does not support mouse dragging.**

`RimUI.Components.OrderList<T> : UiElement` (generic)

## Example

```csharp
var ord = new OrderList<string>(null) { Style = { Height = 190f } };
ord.Items.AddRange(new[] { "Cooking", "Construction", "Plants" });
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `List` | `ListBox<T>` (readonly) | new `ListBox<T>()` | Internal list. |
| `Items` | `List<T>` | `List.Items` | List items. |
| `OnReorder` | `Action<List<T>>` | `null` | Receives the current list after each move. |
| `Disabled` | `bool` | `false` | Disables list and buttons. |
| `ButtonsWidth` | `float` | `26` | Button-column width. |
| `Rows` | `int` | `0` | Fixed row count; otherwise content or `Style.Height` determines height. |

## Styles

Styles the internal `ListBox` and `ListButton` controls through `list/button*`.

## Methods

| Method | Returns | Description |
|---|---|---|
| `OrderList(Action<List<T>> onReorder = null)` (constructor) | - | Creates a list with a reorder callback. |
| `SetStyle(string path, string value)` | `void` | Applies the path to the root style. Unknown paths and values are ignored. |

The four buttons mutate `Items` in place. There is no drag threshold or drag operation. Selection belongs to `List`; use `orderList.List.GetSelected()` and `SetSelected(index)`.

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnReorder` | `Action<List<T>>` | current list | After each move. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | Pointer enters or leaves the bounds, once per transition. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | Every frame while hovered; keep the handler lightweight. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left or right click within the bounds. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Mouse wheel over the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | Effective enabled state changes. |

`OrderList` does not raise `SelectionChanged`. Subscribe to `orderList.List.Events.SelectionChanged`.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style | Yes | Internal `ListBox` and `list/button*`. |
| Sprite frame | Yes | `list/button`. |
| Animation | Yes | Shared `Style.Animation`. |
| Disabled | Yes | Forwarded to list and buttons. |
| Hover fade | No | Immediate row and button hover. |


---

[Table of contents](../index_en.md) - Data | Previous: [ListBox&lt;T&gt;](01_listbox_en.md) | Next: [PickList&lt;T&gt;](03_picklist_en.md) | [Русский](02_orderlist_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
