![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.31` · mod `0.3.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Data | Previous: [ScrollBox](07_scrollbox_en.md) | Next: [Timeline](09_timeline_en.md) | [Русский](08_paginator_ru.md)

---

# Paginator

Page navigation: first / previous / next / last arrows, page numbers with ellipses, and an
optional page-size selector.

The component **does not own the data**: it has no idea what to select or where from — its job is
to report which page the user asked for. The selection (`Skip`/`Take`) stays with you.

`RimUI.Components.Paginator : UiElement`

## Pages are numbered from one

Externally (`Page`, `OnChange`, `GetPage`, `SetPage`) pages are counted **from 1** — the way the
user sees them on screen. Zero-based indices are handier when selecting data, but then you would
have to keep in mind that "page 3" in your handler is the fourth one on screen; that mistake costs
more than the single subtraction it saves:

```csharp
int from = (page - 1) * pageSize;   // the conversion to an index happens once, on your side
```

## Example

```csharp
var pager = new Paginator
{
    TotalItems = items.Count,
    PageSize = 10,
    Page = () => _page,
    OnChange = p => { _page = p; RebuildPage(); },
    SummaryText = (a, b, c) => a + "-" + b + " of " + c
};
pager.PageSizeOptions.Add(10);
pager.PageSizeOptions.Add(25);
pager.PageSizeOptions.Add(50);
pager.OnPageSizeChange = size => { _pageSize = size; RebuildPage(); };

// compact variant
new Paginator { TotalItems = 42, PageSize = 10, ShowFirstLast = false, VisiblePages = 3 };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Key` | `string` | `null` | State key in the ID store (see "Keys and state" on the Architecture page). Needed when the element is recreated between frames, or when its state must survive such recreation. |
| `TotalItems` | `int` | `0` | Total item count. The number of pages follows from it and `PageSize`. |
| `PageSize` | `int` | `10` | Items per page. |
| `Page` | `Func<int>` | `null` | Current page source (from 1). `null` = the page lives in the ID store. |
| `OnChange` | `Action<int>` | `null` | The user picked a page. |
| `PageSizeOptions` | `List<int>` (readonly) | empty | Page size options. Empty = the selector is not shown. |
| `OnPageSizeChange` | `Action<int>` | `null` | The user changed the page size. |
| `ShowFirstLast` | `bool` | `true` | "First" and "last" buttons (double chevrons). |
| `VisiblePages` | `int` | `5` | How many numbers to show around the current one (excluding the first and last). |
| `SummaryText` | `Func<int,int,int,string>` | `null` | Caption on the right: arguments are `(first, last, total)`. You supply the text — the core knows nothing about languages. |
| `ButtonSize` | `float` | `26` | Button size. |
| `ButtonGap` | `float` | `3` | Gap between buttons. |
| `PageSizeWidth` | `float` | `62` | Width of the page-size selector. |
| `Disabled` | `bool` | `false` | Disabled state. |

## Behaviour

**The first and last pages are always visible.** They are the ones people jump to most, and the
ellipsis only says that something in between was skipped. The window of numbers around the current
page is set by `VisiblePages`.

**Changing the page size returns to the first page.** The current page almost always falls outside
the new range (20 pages of 10 became 4 of 50), and the user would be looking at emptiness.

**There is always at least one page.** An empty list is one empty page, not zero pages: otherwise
the current page number would fall outside the valid range.

**The current number is not clickable**, but it does not look disabled either — it is highlighted
with the accent colour. Only the arrows at the ends of the range get the disabled look.

**`PageSizeWidth` is fixed for a reason.** A `Select` without an explicit width takes all the space
it is offered, and it is offered the width of the block — the field would run off the edge. There
are only ever two or three short numbers in it.

## Styles

| Slot | Purpose |
|---|---|
| `paginator/button` | A button (number or arrow). |
| `paginator/button_hover` | Under the cursor. |
| `paginator/button_active` | The current page. |
| `paginator/button_disabled` | An unavailable button (end of the range). |
| `paginator/text` | Number colour. |
| `paginator/text_active` | Number colour on the current page. |
| `paginator/text_disabled` | Colour of unavailable numbers and the ellipsis. |

The page-size field is a regular `Select<int>`, so it is themed through the `select/*` slots.

## Methods

| Method | Returns | Description |
|---|---|---|
| `PageCount` (property) | `int` | Total number of pages (at least 1). |
| `GetPage()` | `int` | The current page (from 1): the `Page` source if set, otherwise the cache from the last frame. |
| `SetPage(int page)` | — | Go to a page programmatically: the number is clamped to the range and `OnChange` is raised. |

## Events

| Event | Type | Parameters | When it fires |
|---|---|---|---|
| `OnChange` | `Action<int>` | page number (from 1) | A number or arrow was clicked, and also when the page size changes (jump to the first page). |
| `OnPageSizeChange` | `Action<int>` | the new size | A different page size was selected. |
| `Events.SelectionChanged` | `UiEventHandler` | `data.SelectedIndex`, `data.SelectedValue` | Same as `OnChange`. The index is the **zero-based** page number, the value is the number from 1. |
| `Events.HoverEnter` / `Events.HoverLeave` | `UiEventHandler` | `data.MousePosition` | The cursor entered or left the bounds. |
| `Events.Click` / `Events.RightClick` | `UiEventHandler` | `data.MousePosition` | Left / right click on the element. |
| `Events.DisabledChanged` | `UiEventHandler` | `data.Disabled` | `Disabled` changed. |

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Styling | Yes | Button and text slots, plus the row's own `Style`. |
| Sprite frame | Yes | Through the `paginator/button` slot. |
| Animation | Yes | The common `Style.Animation` mechanism. |
| Disabled | Yes | The `paginator/button_disabled` slot; input is blocked. |
| Hover fade | No | Button states switch instantly. |
| Cursor | Yes | A hand over the available buttons. |


---

[Table of contents](../index_en.md) - Data | Previous: [ScrollBox](07_scrollbox_en.md) | Next: [Timeline](09_timeline_en.md) | [Русский](08_paginator_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
