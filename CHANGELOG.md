# Mod changelog

[Русский](CHANGELOG_ru.md) · [README](README.md)

## 0.3.0

**Layout**

- **Size limits**: `MinWidth`, `MaxWidth`, `MinHeight`, `MaxHeight` in the style. They apply both
  when the size is computed from content and when it is set explicitly; on conflict the minimum
  wins. There was previously no way to constrain a block — `MinHeight` only worked inside
  `FlexBox`.
- **Wrapping children onto new lines** (`WrapChildren`): the container moves elements that no
  longer fit, and each line is laid out on its own. Supported by row components as well —
  `SelectButton`, `Paginator`, `Stepper`, `Tabs`, `Gallery`.
- **Shrinking** (`Shrink`): `0` means "do not shrink me". Previously everything shrank
  indiscriminately, and a block with a fixed height turned its content into an overlap — text on
  top of text.
- **`Stretch` no longer overrides an explicit size**: a bar with a set width inside a column keeps
  that width.
- **Clipping at the border** (`Overflow.Clip`) fixed: the property existed but did nothing, so
  content that was not allowed to grow spilled outside.
- **Row height from content** (`AutoHeight`) in `ListBox`, `Tree`, `DataTable`, `TreeTable`: text
  wraps and the row grows. Scrolling stays virtualized.

**Events**

- **A click goes to exactly one element** — the first that handles it. Previously a button and the
  block underneath both treated the press as their own, and suppressing that was manual work.
- **The mouse wheel** goes to whatever is under the cursor: scrolling no longer "falls through"
  into the list beneath the area.
- **The cursor** is claimed by the deepest element under the mouse, not by its parent.
- **Order among siblings** (`ZOrder`) for overlapping elements: the higher one gets the click, the
  wheel and the cursor first. Drawing order is unaffected.

**Windows**

- **Mouse resizing** by edges and corners — for windows and for any block with a border
  (`Style.Resizable`, eight directions). Only the regular width and height change, within
  Min/Max; there is no grab along an axis the layout owns; a block never grows past its parent. A
  window's dragged size is remembered and is not reset back to the content-derived size.
- **Dragging a window off-screen**: up to 80% of it may go past the left, right and bottom edges;
  past the top edge is forbidden, otherwise the header you drag it by could not be reached again.
  Recomputed when the resolution or UI scale changes, too.
- **Custom modal window header**: three zones (left, centre, right), each accepting your own
  elements before and after the built-in ones, and the built-in ones can be switched off. Pressing
  a button in the header no longer drags the window.

**Components**

- **A text field's value is available while the field is off-screen** — from a hidden tab or a
  collapsed panel. `Flush()` was added: it commits what was typed when a "Save" button is pressed,
  without Enter and without losing focus.
- **`CascadeSelect`**: clearing the selection, selecting a branch, multi-select with check marks,
  `SetSelected`/`Clear` from code. The list can now be filled or rebuilt after the first frame —
  previously the menu was built exactly once and kept showing the old set.
- **Initial table sorting** (`DefaultSortColumn`/`DefaultSortAsc`) in `DataTable` and `TreeTable`;
  it never overrides what the player chose.
- **Value axis for charts**: your own bounds, step and label format, with the step rounded to
  1/2/5·10ⁿ. That removes fractional "2.5 items" on whole-number data. In the heatmap the same
  bounds set the colour scale, which makes two maps side by side comparable.

**Themes**

- **Style classes**: a named set of properties attaches to any element (`AddClass`/`SetClass`) and
  is declared in the theme as a section prefixed with `#`, or from code. Previously the slot was
  chosen by the element's type and could not be substituted from outside.

## 0.2.1
Fix: components without an explicit `Key` lost their state.

- **Fixed:** a component with no `Key` failed to keep its internal state between the phases of a
  frame — tabs would not switch, trees would not expand, lists would not scroll. The internal key
  is now tied to the element itself rather than to call order, so a single component works without
  a `Key`.
- An explicit `Key` is still needed when elements are recreated, or when their state must survive
  the window being rebuilt. Existing keys need no changes: they take priority, as before.
- **Fixed:** `PickList` crashed when a row was moved across by double-click — the list shrank
  mid-draw and the component reached for a row that was already gone.
- Documentation: a new "Keys and state" section on the Architecture page, and `Key` added to the
  parameter tables of every stateful component.

## 0.2.0
Cursors, an org-chart component, horizontal scrolling, and versioned documentation.

- **The cursor now changes on hover**: a hand over buttons and window headers, a caret over text
  fields. Nine kinds (arrow, hand, move, text, four resize directions) are available via
  `Style.Cursor`; textures and hotspots are swappable through the theme.
- **OrganizationChart** - a hierarchical diagram: nodes with arbitrary content, connector lines,
  collapsible subtrees, and node selection (single, multiple, or cascading over a whole subtree).
  Grows downwards or to the right.
- **Horizontal scrolling** in `ScrollBox` (opt-in via the `Horizontal` flag).
- **Scrollbars inside lists and tables can now be dragged** - the handle used to be an indicator
  only. Clicking an empty part of the bar jumps to that position.
- **Fixed**: the wheel over an inner list also scrolled the outer area.
- `NodeCanvas`: `AddNode`/`RemoveNode`/`AddLink`/`RemoveLink` keep the field consistent (deleting
  a node removes its links), and the saved JSON now carries a format version - older saves still
  load, while a file from a newer version fails with a clear message instead of corrupting the field.
- Documentation is versioned: [Doc/](Doc/index_en.md) is a version picker, `0.2` is current,
  `0.1` is archived.

## 0.1.0
Initial release.

- UI library for RimWorld modders: a 12-column grid, styles (spacing, solid/gradient/image
  fills, borders, corner radii, and shadows), plus JSON-driven themes.
- No vanilla patches, no Harmony requirement, and no runtime dependencies.
- Built-in modding tools in the mod settings: component showcase, UI builder, and theme
  template export.
