![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.8.21` · mod `0.2.1` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Getting started | Next: [Grid](../01-grid/01_grid_en.md) | [Русский](01_start_ru.md)

---

# Getting started

RimUI Framework is a UI library for RimWorld mods with a web-inspired workflow: a 12-column grid, 
ready-made components (buttons, fields, tables, charts, and more), and
themes with custom colors and sprites. It does not patch the game or require Harmony. Your mod
simply references it as a library.

Rendering uses **immediate mode**. Every frame, you describe the element tree again: which grid
and components to show and what their current values are. RimUI measures (`Measure`), positions
(`Arrange`), and draws (`Emit`) everything for you. When content changes, there is no need to
recalculate coordinates by hand. Just describe the UI you want from the current data, much like
React or an immediate-mode GUI.

## Setup

1. Add `RimUI Framework` as a dependency in your mod's `About.xml`:
   ```xml
   <modDependencies>
     <li>
       <packageId>klimprog.rimui</packageId>
       <displayName>RimUI Framework</displayName>
     </li>
   </modDependencies>
   <loadAfter>
     <li>klimprog.rimui</li>
   </loadAfter>
   ```
2. Add the mod's `Assemblies/RimUI.dll` as a reference in your C# project.
3. Public types live under `RimUI.Core`, `RimUI.Layout`, `RimUI.Styling`,
   `RimUI.Components`, `RimUI.Elements`, `RimUI.Animation`, and `RimUI.Adapter`.

## Your first window

```csharp
using RimUI.Adapter;
using RimUI.Elements;
using RimUI.Layout;

var root = new Grid();
root.Cell(12, Button.Make("Hello, RimUI!"));

var window = new ModalWindow(root) { Title = "My window" };
window.Show();
```

`ModalWindow` is a regular RimWorld window (`Verse.Window` under the hood) with a header, title,
close button, body, and an optional action footer. See the Windows section for details.

## Modding tools

The tools are available from both the mod settings and Debug actions
(Dev mode → Debug actions → RimUI):

- **Component showcase**: live, interactive samples of every framework component. The showcase
  itself is built entirely with RimUI.
- **UI builder**: assemble a block grid with the mouse and export ready-to-use C# code to the
  clipboard.
- **Theme template export**: copies complete theme JSON with every slot, key, and default value.

## How to use these docs

Every component page follows the same structure: overview, code example, parameter table, styles,
methods, events, and a feature-support table covering animation, sprites, tiling, disabled state,
and hover fading. Use the table of contents and the Previous/Next links to move through the pages
in order.


---

[Table of contents](../index_en.md) - Getting started | Next: [Grid](../01-grid/01_grid_en.md) | [Русский](01_start_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
