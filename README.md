# PulseDock

A floating eww music player widget that reads live metadata from YouTube Music (or any browser-based player) via MPRIS/playerctl.

![widget](.github/preview.png)

---

## Dependencies

| Tool | Purpose |
|---|---|
| `eww` | Widget renderer |
| `playerctl` | MPRIS media control / metadata |
| `pactl` | Volume and audio output control |
| `curl` | Cover art download |

Install on Arch:
```bash
pacman -S playerctl libpulse curl
yay -S eww
```

---

## Setup

**1. Clone the repo**
```bash
git clone https://github.com/guywithxm5/audplay ~/.config/eww/audplay
```

**2. Make scripts executable**
```bash
chmod +x ~/.config/eww/audplay/scripts/*.sh
chmod +x ~/.config/eww/audplay/launch_pulsedock
```

**3. Enable MPRIS in your browser**

- **Chromium / Chrome / Brave** — MPRIS is enabled by default on Linux
- **Firefox** — go to `about:config` and set `media.hardwaremediakeys.enabled = true`

Open YouTube Music in the browser and start playing something, then verify playerctl can see it:
```bash
playerctl -l
playerctl metadata title
```

---

## i3 keybind

Add this line to your `~/.config/i3/config` to toggle the widget with `$mod+m`:

```
bindsym $mod+m exec --no-startup-id ~/.config/eww/audplay/launch_pulsedock
```

Reload i3 after:
```
$mod+Shift+r
```

---

## Manual usage

```bash
# Open the widget
./launch_pulsedock

# Or control eww directly
eww -c ~/.config/eww/audplay open music-player
eww -c ~/.config/eww/audplay close music-player
```

---

## Project structure

```
audplay/
├── eww.yuck           # Widget layout and variable definitions
├── eww.scss           # Styles
├── launch_pulsedock   # Launcher / toggler script
└── scripts/
    ├── listen-status.sh   # deflisten: emits play/pause state
    ├── get-cover.sh       # Fetches and caches cover art locally
    ├── get-progress.sh    # Calculates playback progress percentage
    ├── toggle-play.sh     # Play / pause
    ├── prev.sh            # Previous track
    ├── next.sh            # Next track
    ├── toggle-shuffle.sh  # Toggle shuffle
    ├── toggle-repeat.sh   # Cycle repeat mode (None → Track → Playlist)
    ├── toggle-like.sh     # Toggle like (UI state only)
    ├── seek.sh            # Seek to position
    ├── set-volume.sh      # Set system volume
    └── change-output.sh   # Cycle audio output sinks
```
