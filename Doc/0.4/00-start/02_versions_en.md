![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.9.3` · mod `0.4.0` · RimWorld `1.6, 1.7`

---

[Table of contents](../index_en.md) - Getting started | Previous: [Getting started](01_start_en.md) | Next: [Grid](../01-grid/01_grid_en.md) | [Русский](02_versions_ru.md)

---

# Game versions

RimUI Framework runs on **RimWorld 1.6** and **RimWorld 1.7**. Both versions have the same features:
every component, charts, `NodeCanvas` links, animations, cursors, OS fonts and audio.

## Where the assembly lives

Each game version has its own framework assembly:

| Game version | Assembly |
|--------------|----------|
| RimWorld 1.6 | `1.6/Assemblies/RimUI.dll` |
| RimWorld 1.7 | `1.7/Assemblies/RimUI.dll` |

The game loads the folder of its own version by itself. Themes, textures, translations and
documentation live in the mod root and are shared by both versions. There is no `Assemblies` folder
in the mod root.

## Referencing it in your project

Reference `RimUI.dll` from the folder of the game version you build your mod for. Public types and
their members are the same in both assemblies: the same code of your mod compiles against either of
them.

## How the versions differ

**Text.** RimWorld 1.7 renders and measures text differently from 1.6, so the width and height of the
same label differ slightly between the versions. The framework sizes elements itself from the actual
text. If you set a size for text by hand (`Width` or `Height` in pixels), check it in both versions.


---

[Table of contents](../index_en.md) - Getting started | Previous: [Getting started](01_start_en.md) | Next: [Grid](../01-grid/01_grid_en.md) | [Русский](02_versions_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
