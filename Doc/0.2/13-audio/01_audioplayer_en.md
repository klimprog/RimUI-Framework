![RimUI Framework](../../../About/Preview.png)

**RimUI Framework** — core `0.7.9` · mod `0.2.0` · RimWorld `1.6`

---

[Table of contents](../index_en.md) - Audio | Previous: [Gallery](../12-display/03_gallery_en.md) | Next: [Chart - shared chart base](../14-charts/01_chart_en.md) | [Русский](01_audioplayer_ru.md)

---

# AudioPlayer

An audio helper for `.ogg`, `.wav`, and `.mp3` playlists from a mod folder. It exposes play, pause, next, previous, and seek operations. It is not a ready-made player UI; build controls from regular framework components.

`RimUI.Adapter.AudioPlayer` (helper class, not `UiElement`)

## Example

```csharp
var audio = new AudioPlayer();
audio.LoadFolder(System.IO.Path.Combine(MyMod.RootDir, "Audio"));

var playBtn = Button.Make("Play/Pause");
playBtn.OnClick = () => { if (audio.IsPlaying) audio.Pause(); else audio.Play(); };
```

## Parameters

| Parameter | Type | Default | Description |
|---|---|---|---|
| `Count` | `int` | `0` | Playlist length. |
| `Volume` | `float` | - | 0 to 1, multiplied by `Prefs.VolumeGame`. |
| `IsPlaying` | `bool` | `false` | Whether playback is active. |
| `CurrentIndex` | `int` | `-1` | Current track, or `-1`. |
| `CurrentName` | `string` | `null` | Current filename without extension. |
| `PositionSec` | `float` | `0` | Current position in seconds. |
| `DurationSec` | `float` | `0` | Current duration in seconds. |

### Important: blocking 3-second load timeout

`Play(index)` loads clips synchronously by busy-waiting on `UnityWebRequest` with `Thread.Sleep(1)`, up to 3000 ms. A corrupt or unavailable file can therefore block the game's main thread and frame for the full timeout before logging a warning. Clips are cached by index, so this risk applies only to the first load of each track.

## Styles

Not applicable to a nonvisual helper.

## Methods

| Method | Returns | Description |
|---|---|---|
| `AudioPlayer()` | - | Creates an empty player. |
| `LoadFolder(string absDir)` | `void` | Loads `*.ogg`, `*.wav`, and `*.mp3` alphabetically. |
| `Load(IList<string> paths)` | `void` | Loads explicit absolute paths. |
| `ListNames()` | `IList<string>` | Track names, with the current one prefixed by `"> "`. |
| `Play(int index)` | `void` | Starts or resumes an indexed track. |
| `Play()` | `void` | Resumes current track or starts the first. |
| `Pause()` | `void` | Pauses playback. |
| `Next()` | `void` | Advances with wrapping. |
| `Prev()` | `void` | Goes back with wrapping. |
| `Seek(float deltaSec)` | `void` | Seeks and clamps to 0 through duration. |
| `Dispose()` | `void` | Releases `AudioSource` and clips. Call when closing or unloading the player. |

## Events

No callbacks; state is read passively.

## Feature support

| Feature | Supported | Notes |
|---|---|---|
| Style / sprite / animation | Not applicable | Build the UI from normal components. |
| Disabled | No | Calling code disables its own controls. |
| Hover fade | Not applicable | No visual representation. |


---

[Table of contents](../index_en.md) - Audio | Previous: [Gallery](../12-display/03_gallery_en.md) | Next: [Chart - shared chart base](../14-charts/01_chart_en.md) | [Русский](01_audioplayer_ru.md)
## Support the author
If you enjoy RimUI Framework, you can support its development here:

[DonationAlerts](https://www.donationalerts.com/r/klimprog)
[Boosty](https://boosty.to/klimprog/donate)

**USDT (TRC20):** `TA4zq9F4TTrSQXjEESMBk8juQMGNLXX4B5`

Thank you! ❤️
