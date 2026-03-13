# Hackerpad 9000( spotify desk thing )

hacker pad 9000 is a custom key macro pad with two rotating dials and 6 customisable keys with a OLED screen 
made with love , basicly my ittration for spotify car thing for ur desk !.

<img src=assets/key_mapping.png alt="Schematic" width="500"/>

-------------------------------------------------------------------------------------------------------------
## Features:
- deal part case w easy to assemble.
- 128x32 OLED Display
- 2 EC11 Rotary encoder for whatever you want
- 4 heat threaded inserts and bolt it tightly 
- 6 Keys

-------------------------------------------------------------------------------------------------------------
## CAD Model:
Everything fits together using 4 M3 Bolts and heatset inserts. 4 for the case
it has two part cover with main body and front lid

<img src=assets/hackerpadd.png alt="Schematic" width="500"/>

Made in Fusion360.


-------------------------------------------------------------------------------------------------------------
## PCB
Here's my PCB! It was made in KiCad.

Schematic
<img src=assets/schematic.png alt="Schematic" width="300"/>

PCB
<img src=assets/PCB.png alt="Schematic" width="300"/>

MX styled keys used 

-------------------------------------------------------------------------------------------------------------
## Firmware Overview
Spotify Macropad with OLED Display :control_knobs:
A custom Spotify control macropad built using the Seeed XIAO RP2040, running KMK firmware, and powered by a Python bridge application that connects Spotify playback data to the keyboard over USB.
The device acts as a hardware Spotify controller + display, showing live song information, album art, and animations on a small OLED screen.
The project behaves like a mini StreamDeck dedicated to music control.

Features
Playback Control
Play / Pause
Next Track
Previous Track
Shuffle toggle
Repeat toggle
Hardware mute
Encoder Controls
Encoder 1 → Volume
Encoder 2 → Track scrubbing
OLED UI
Current song title
Artist name
Playback progress bar
Album artwork
Animated BongoCat idle screen
Lighting
16 SK6812 RGB LEDs
BPM-reactive lighting
System
Two-way serial communication
Python Spotify bridge application
Automatic Spotify authentication
Token caching
Auto reconnect

System Architecture
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
The bridge application handles Spotify API communication, while the firmware handles hardware interaction.

Hardware Components
Controller
Seeed XIAO RP2040
Inputs
6 × MX mechanical switches
2 × EC11 rotary encoders
Display
0.91" SSD1306 OLED (128×32)
Lighting
16 × SK6812 MINI-E RGB LEDs
Other Components
20 × 1N4148 diodes
DSA keycaps
screws + heat-set inserts

Key Layout
[ Like ]     [ Shuffle ]      [ Minimize ]

[ Rewind ]   [ Play/Pause ]   [ Skip ]
Encoder Functions
Left Encoder  → Track Timeline
Right Encoder → Volume

OLED Display
Spotify screen
Spotify
Blinding Lights
████░░░░░░ 1:42
Displays
Song title
Playback progress
Current playback time

Volume screen
Volume
██████░░░░ 60%
Displayed when adjusting volume.

Idle Screen
Animated BongoCat appears when nothing is playing.

Album Art Rendering
Album art is fetched from Spotify and processed by the bridge:
1. Download image
2. Resize to 32×32
3. Convert to monochrome
4. Send bitmap to keyboard
The OLED displays the artwork beside track information.

Serial Protocol
Communication between the firmware and the bridge uses a lightweight text protocol.
Example messages:
SONG|title|artist|position|duration|tempo
VOL|70
IDLE
COVER|bitmap_data
CMD|NEXT
CMD|PLAY
CMD|SEEK|120
This protocol enables two-way communication.

Firmware
Firmware is written using KMK (Keyboard Module Kit) running on CircuitPython.
Responsibilities:
Scan key matrix
Handle rotary encoders
Communicate with bridge over USB
Render OLED interface
Control RGB lighting
Main firmware file:
CIRCUITPY/code.py

Pin Mapping
Matrix
FunctionPinCOL0D2 / GP28COL1D3 / GP29COL2D10 / GP3ROW0D7 / GP1ROW1D8 / GP2
Encoders
Encoder 1 (Volume)
SignalPinAD0 / GP26BD1 / GP27
Encoder 2 (Track Scrub)
SignalPinAD6 / GP0BD9 / GP4
OLED
SignalPinSDAD4 / GP6SCLD5 / GP7
RGB LEDs
DATA → D0 / GP26

Folder Structure
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

Installing Firmware
1. Install CircuitPython for the XIAO RP2040
https://wiki.seeedstudio.com/XIAO-RP2040-with-CircuitPython/
2. Copy firmware files to the board
CIRCUITPY/

Spotify Bridge
The bridge software connects the keyboard to Spotify.
Location:
spotify_bridge/
Run:
spotify_bridge.exe
or
python spotify_bridge.py

Spotify API Setup
Create an application at
https://developer.spotify.com/dashboard
Add redirect URI
http://localhost:8888/callback
Copy credentials into
spotify_bridge/res/config.json

Running the System
1. Plug the macropad into USB
2. Launch spotify_bridge.exe
3. Start Spotify
4. Use the macropad to control playback

Future Improvements
Potential upgrades
audio visualizer on OLED
RGB BPM lighting improvements
system tray bridge application
multi-service support (YouTube Music, Apple Music)
enhanced OLED UI

License
MIT License
-------------------------------------------------------------------------------------------------------------
## BOM:
Here should be everything you need to make this hackpad
  
- 6x Cherry MX Switches
- 6x DSA Keycaps
- 4x M3x5x4 Heatset inserts
- 4x M3x16mm SHCS Bolts
- 6x 1N4148 DO-35 Diodes.
- 1x 0.91" 128x32 OLED Display
- 2x EC11 Rotary Encoder
- 1x XIAO RP2040
- 1x Case (2 printed parts)

