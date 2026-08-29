# RimUI Framework

![RimUI Framework](About/Preview.png)

[Русский](README_ru.md)

`Core 0.7.9` · `Mod 0.2.0` · RimWorld `1.6`

## Overview

**RimUI Framework** is a UI library for RimWorld modders. It provides a 12-column grid,
styles (spacing, solid/gradient/image fills, borders, corner radii, and shadows), and
JSON-driven themes. It does not patch vanilla code, require Harmony, or pull in any
runtime dependencies.

The mod does nothing on its own; it is a dependency for other mods. If none of your
active mods use it, having RimUI installed will not affect the game.

## Links

- **Steam Workshop:** [Workshop page](https://steamcommunity.com/sharedfiles/filedetails/?id=3763759074)
- **Documentation:** [Doc/index_en.md](Doc/index_en.md)

## Compatibility

RimUI Framework is a library, not a content mod. Which means:

- **It does not patch the game.** Harmony is not required, vanilla classes are not overridden and
  game windows are not replaced. The framework draws only inside windows opened by the mod using it.
- **It adds and changes no defs.** Not a single item, building, incident or research appears in the
  game — the `Defs` folder is empty.
- **It writes nothing to the save.** No `GameComponent`, no `WorldComponent`, no `Scribe` fields:
  removing the mod does not corrupt existing saves.
- **No runtime dependencies.** Just the framework's own code, no third-party libraries.
- **Supports RimWorld 1.6.**

**Load order:** RimUI Framework must load **before** the mods that use it — they declare the
dependency and `loadAfter` in their own `About.xml`.

**Conflicts.** The library does not interfere with anyone else's interface, so it has nothing to
conflict over with other UI mods. When several mods use RimUI Framework, they all work with one
shared copy: the assembly is not duplicated.

## Tools for modders

All tools are available from the mod settings: **Options → Mod settings → RimUI Framework**.

- **Component showcase** — a live gallery of every framework component, built entirely
  with RimUI itself. Click **Open component showcase**.
- **UI builder** — visually assemble a block grid, then export ready-to-use C# code to
  the clipboard. Click **Open UI builder**.
- **Theme template export** — copies every theme slot and key to the clipboard so you
  can customize the lot. Click **Copy theme template**.

The same three tools are also available under **Debug actions → RimUI** when dev mode is enabled.



## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️