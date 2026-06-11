# Carried Stillness Intruder

WiFi-reactive video installation. Movement distorts the image.

Part of *Hard Copy*, graduation project at Konstfack 2026.

**Nadeschda Barenje** - [vetroal.se](https://vetroal.se)

![Movement](media/move.jpg)

![Still](media/still.jpg)

## What is CSI?

**Channel State Information** is data about how WiFi signals move through a room. The signals bounce off walls, furniture, bodies. Every object creates a pattern of interference. When something moves, the pattern changes.

By approaching signals as a body that does not see, but is seen without eyes, defined in the resistance between source and receiver, I try to look into another world.

The video glitches to show that my presence alone affects the space around me. Regardless of what I consciously intend to do, my presence is perceived.

## Demo

[![Watch on YouTube](media/still.jpg)](https://www.youtube.com/watch?v=qWLisjFUW7U)

[Watch on YouTube](https://www.youtube.com/watch?v=qWLisjFUW7U)

## How it works

```
[Router] <--- WiFi ---> [ESP32] ---> [Browser] ---> [Glitchy video]
```

The ESP32 monitors WiFi signal strength. When you move between the router and the ESP32, the signal changes. The browser polls the ESP32 every 30ms and applies CRT-like distortion effects to the video.

## Requirements

- 1-4x ESP32-C6 development boards
- WiFi router
- Modern browser (Chrome/Firefox)
- Python 3 + ESPHome (for flashing)

## Quick Start

1. Install ESPHome:
   ```bash
   pip install esphome
   ```

2. Edit `espectre-node1.yaml` with your WiFi credentials

3. Flash the ESP32:
   ```bash
   esphome upload espectre-node1.yaml
   ```

4. Note the ESP32's IP address from the serial output

5. Edit `PHANTOM_NODE_IP` in `gui/index.html` to match

6. Open `gui/index.html` in your browser

7. Drag and drop a video file

8. Move around the room

## Controls

| Key | Action |
|-----|--------|
| C | Calibrate (stand still for 3 seconds) |
| H | Hide/show UI |
| F | Fullscreen |

## Files

```
gui/index.html       <- main application
espectre-node*.yaml  <- ESP32 configurations (edit WiFi here)
media/               <- images and demo video
SETUP.txt             <- detailed hardware setup guide
TECHNICAL.txt         <- how the effects work
```

## Documentation

- [SETUP.txt](SETUP.txt) - Hardware setup, flashing, troubleshooting
- [TECHNICAL.txt](TECHNICAL.txt) - CSI theory, Sobel edge detection, CRT effects

## Soundtrack

Dopplereffekt - Cerebral (1995)

## License

MIT
