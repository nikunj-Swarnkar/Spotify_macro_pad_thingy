# Spotify marcro-pad thingy

A custom key macropad with two rotating dials, 6 customisable keys, and an OLED screen. Basically my iteration of the Spotify car thing — but for your desk.

<img src="assets/key_mapping.png" alt="Key mapping" width="500"/>

---

## Features

- Two-part case, easy to assemble
- 128×32 OLED display
- 2 × EC11 rotary encoders
- 4 heat-set inserts and bolted case
- 6 mechanical keys

---

## CAD Model

Made in Fusion360. Everything fits together using 4 × M3 bolts and heat-set inserts. The case has two parts: a main body and a front lid.

<img src="assets/hackerpadd.png" alt="CAD model" width="500"/>

---

## PCB

Made in KiCad.

**Schematic**

<img src="assets/schematic.png" alt="Schematic" width="300"/>

**PCB**

<img src="assets/PCB.png" alt="PCB" width="300"/>

Uses MX-style key switches.

---

## Firmware Overview

The Hackerpad 9000 runs **KMK firmware** on a **Seeed XIAO RP2040**, paired with a **Python bridge application** that connects Spotify playback data to the keyboard over USB. The device behaves like a mini StreamDeck dedicated to music control.

### Playback Controls

| Key        | Action          |
| ---------- | --------------- |
| Play/Pause | Toggle playback |
| Next       | Next track      |
| Previous   | Previous track  |
| Shuffle    | Toggle shuffle  |
| Repeat     | Toggle repeat   |
| Mute       | Set volume to 0 |

### Encoder Controls

| Encoder   | Function       |
| --------- | -------------- |
| Encoder 1 | Track timeline |
| Encoder 2 | Volume         |

### Key Layout

```
[ Like ]     [ Shuffle ]    [ Minimize ]
[ Rewind ]   [ Play/Pause ] [ Skip ]
```

---

## OLED Display

### Spotify Screen

```
Spotify
Blinding Lights
████░░░░░░ 1:42
```

Displays song title, playback progress bar, and current playback time.

### Volume Screen

```
Volume
██████░░░░ 60%
```

Displayed when adjusting volume.

### Idle Screen

An animated BongoCat appears when nothing is playing.

---

## Album Art Rendering

Album art is fetched from Spotify and processed by the bridge:

1. Download image
2. Resize to 32×32
3. Convert to monochrome
4. Send bitmap to keyboard

The OLED displays the artwork beside the track information.

---

## System Architecture

```
Spotify App
      ↓
Spotify Web API
      ↓
spotify_bridge.exe (Python)
      ↓
USB Serial
      ↓
KMK Firmware
      ↓
Macropad Hardware
      ↓
OLED Display + Controls
```

The bridge application handles Spotify API communication; the firmware handles hardware interaction.

---

## Serial Protocol

Communication between the firmware and bridge uses a lightweight text protocol:

```
SONG|title|artist|position|duration|tempo
VOL|70
IDLE
COVER|bitmap_data
CMD|NEXT
CMD|PLAY
CMD|SEEK|120
```

---

## Firmware

Written using **KMK (Keyboard Module Kit)** on CircuitPython. Main file: `CIRCUITPY/code.py`

**Responsibilities:**

- Scan key matrix
- Handle rotary encoders
- Communicate with bridge over USB
- Render OLED interface
- Control RGB lighting

### Pin Mapping

**Matrix**

| Function | Pin       |
| -------- | --------- |
| COL 0    | D2 / GP28 |
| COL 1    | D3 / GP29 |
| COL 2    | D10 / GP3 |
| ROW 0    | D7 / GP1  |
| ROW 1    | D8 / GP2  |

**Encoder 1 — Volume**

| Signal | Pin       |
| ------ | --------- |
| A      | D0 / GP26 |
| B      | D1 / GP27 |

**Encoder 2 — Track Scrub**

| Signal | Pin      |
| ------ | -------- |
| A      | D6 / GP0 |
| B      | D9 / GP4 |

**OLED**

| Signal | Pin      |
| ------ | -------- |
| SDA    | D4 / GP6 |
| SCL    | D5 / GP7 |

**RGB LEDs:** DATA → D0 / GP26

---

## Folder Structure

```
.
├── LICENSE
├── README.md
│
├── CIRCUITPY
│   ├── code.py
│   ├── display_manager.py
│   ├── duck_animation.py
│   ├── rgb_manager.py
│   │
│   └── lib
│       ├── adafruit_ssd1306.mpy
│       ├── neopixel.mpy
│       └── kmk/
│
└── spotify_bridge
    ├── spotify_bridge.exe
    ├── README.md
    │
    ├── code
    │   ├── build_exe.py
    │   ├── oled_test_app.py
    │   └── spotify_bridge.py
    │
    └── res
        ├── config.json.eg
        ├── Duck.gif
        └── oled_test_app.exe
```

---

## Installing Firmware

1. Install CircuitPython for the XIAO RP2040:
   https://wiki.seeedstudio.com/XIAO-RP2040-with-CircuitPython/

2. Copy firmware files to the board at `CIRCUITPY/`.

---

## Spotify Bridge

The bridge software connects the keyboard to Spotify.

**Location:** `spotify_bridge/`

**Run:**

```
spotify_bridge.exe
```

Or:

```
python spotify_bridge.py
```

### Spotify API Setup

1. Create an application at https://developer.spotify.com/dashboard
2. Add the redirect URI: `http://localhost:8888/callback`
3. Copy your credentials into `spotify_bridge/res/config.json`

---

## Running the System

1. Plug the macropad into USB
2. Launch `spotify_bridge.exe`
3. Start Spotify
4. Use the macropad to control playback

---

## Future Improvements

- Audio visualizer on OLED
- RGB BPM lighting improvements
- System tray bridge application
- Multi-service support (YouTube Music, Apple Music)
- Enhanced OLED UI

---

## Bill of Materials

| Qty | Item                      |
| --- | ------------------------- |
| 6×  | Cherry MX switches        |
| 6×  | DSA keycaps               |
| 4×  | M3×5×4 heat-set inserts   |
| 4×  | M3×16mm SHCS bolts        |
| 6×  | 1N4148 DO-35 diodes       |
| 1×  | 0.91" 128×32 OLED display |
| 2×  | EC11 rotary encoders      |
| 1×  | Seeed XIAO RP2040         |
| 1×  | Case (2 printed parts)    |

---

## License

MIT License
