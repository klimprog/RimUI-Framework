![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.15` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [Paginator](08_paginator_en.md) | Next: [OrganizationChart](10_organizationchart_en.md) | [Русский](09_timeline_ru.md)

---

# Timeline

A timeline of events: markers on an axis, connector lines between them, content to the side.
Vertical (top to bottom) and horizontal (left to right) orientations.

`RimUI.Components.Timeline : UiElement`

## Three bands across the timeline

Across the timeline the space is divided into three bands:

```
[ opposite side ] [ axis with markers ] [ content ]
```

The "opposite side" is for a date, a time, a status: a short caption that belongs to the event but
is not its content. If no event uses it, the band is not created and no space is wasted.

Which side the content sits on is set by `Align`. With `Alternate` the content alternates, and then
both bands get the **same size** — otherwise the axis would drift across the timeline from event to
event, following the content.

## Example

```csharp
var tl = new Timeline { OppositeSize = 90f };
tl.Items.Add(new TimelineItem("Landed on the planet", "Day 1") { Icon = Icons.Check });
tl.Items.Add(new TimelineItem("Shelter built", "Day 3"));
tl.Items.Add(new TimelineItem("First raid", "Day 12") { MarkerColor = danger });

// alternating sides
new Timeline { Align = TimelineAlign.Alternate };

// horizontal timeline
new Timeline { Orientation = TimelineOrientation.Horizontal, MinItemExtent = 96f };

// arbitrary content instead of a string
tl.Items.Add(new TimelineItem { Content = myPanel, Opposite = "Day 20" });
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Items` | `List<TimelineItem>` (readonly) | empty | The events of the timeline. |
| `Orientation` | `TimelineOrientation` | `Vertical` | `Vertical` (top to bottom) or `Horizontal` (left to right). |
| `Align` | `TimelineAlign` | `End` | Content side: `Start`, `End` or `Alternate`. |
| `MarkerSize` | `float` | `12` | Marker diameter. |
| `LineThickness` | `float` | `1` | Connector line thickness. |
| `AxisSize` | `float` | `28` | Width of the band holding the markers. |
| `ItemGap` | `float` | `10` | Gap between events along the timeline. |
| `OppositeSize` | `float` | `0` | Size of the opposite-side band. `0` = automatic. |
| `MinItemExtent` | `float` | `34` | Minimum size of an event along the timeline. |

### TimelineItem

| Field | Type | Description |
|---|---|---|
| `Key` | `string` | Stable key of the event (`null` = index). |
| `Text` | `string` | Content as a string (when `Content` is not set). |
| `Opposite` | `string` | Caption on the opposite side. |
| `Content` | `UiElement` | Arbitrary content (takes precedence over `Text`). |
| `OppositeContent` | `UiElement` | Arbitrary content for the opposite side. |
| `Icon` | `int` | Icon inside the marker (icon sheet index; `-1` = none). |
| `MarkerColor` | `ColorRGBA?` | Custom marker colour (`null` = from the theme). |
| `Value` | `object` | Payload. |

## Behaviour

**The line is drawn from marker to marker**, so the last event has no tail: the timeline ends
exactly on it instead of trailing off into nothing.

**Captions are aligned towards the axis**, not towards the outer edge of their band: otherwise a
gap would open between the text and the marker, growing with the band width, and the
caption-to-marker link would be lost. In a horizontal timeline captions are centred under the
marker, which sits in the middle of the event's span. This applies to string captions only;
arbitrary `Content` is aligned by you.

**The size across the timeline** is taken the way other components take it: an explicit
`Style.Width` / `Style.Height`, otherwise whatever the parent offers. If the parent does not
constrain that axis (a horizontal timeline inside a column, say), the bands are sized **by their
content** — otherwise the timeline would claim all the available height and leave a void beneath it.

**`MinItemExtent`** keeps events with short text from sticking together: without it the line
between markers all but disappears.

## Styles

| Slot | Purpose |
|---|---|
| `timeline/marker` | The marker: `Background` is the fill, `Text.Color` the colour of the icon inside. |
| `timeline/line` | Connector colour (colour only: the lines are drawn as commands, not as a background). |

An individual marker's colour is overridden by `TimelineItem.MarkerColor` — handy for
"done / in progress / ahead" statuses.

## Methods

The component has no methods of its own: a timeline is a presentation and holds no state.
Everything is driven through the `Items` list and the fields of each event.

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The cursor entered or left the timeline bounds. |
| `Events.Hover` | `UiEventHandler` | `data.MousePosition` | The cursor is over the timeline — EVERY FRAME. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left / right click on the timeline. |
| `Events.Scroll` | `UiEventHandler` | `data.ScrollDelta` | Wheel over the timeline. |

There are no per-event events: if you need a click on a particular entry, put a clickable element
into its `Content` (a `Button`, for instance) — it will get its own event.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Styling | Yes | Marker and line slots, plus the timeline's own `Style`. |
| Sprite frame | Yes | Through the `timeline/marker` slot and the timeline's `Style`. |
| Animation | Yes | The common `Style.Animation` mechanism. |
| Disabled | No | The timeline is not interactive; unavailability is expressed by its content. |
| Hover fade | No | It has no hover states of its own. |
| Cursor | No | Requested by the content and nested elements. |


---

[Table of contents](../index_en.md) - Data | Previous: [Paginator](08_paginator_en.md) | Next: [OrganizationChart](10_organizationchart_en.md) | [Русский](09_timeline_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
