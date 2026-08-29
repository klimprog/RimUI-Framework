# Mod changelog

[Русский](CHANGELOG_ru.md) · [README](README.md)

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
