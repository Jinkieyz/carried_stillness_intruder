# Carried Stillness Intruder

**WiFi-reactive video installation using Channel State Information**

Part of the *in silico* graduation project at Konstfack University of Arts, Crafts and Design, 2026.

---

## Overview

Carried Stillness Intruder is a real-time system that detects human presence through WiFi signal disturbances and translates this invisible data into visual artifacts on video. When someone moves through the space between a WiFi router and an ESP32 sensor, the video responds with authentic CRT-like magnetic interference patterns.

The work explores the liminal space between presence and absence - the carried stillness that intrudes upon radio waves, leaving traces in the electromagnetic spectrum that surrounds us.

---

## How It Works

```
                    WiFi CSI
    [Router] <-------------------> [ESP32-C6]
                                        |
                                   HTTP REST API
                                        |
                                        v
                                  [Web Browser]
                                  [Visualization]
```

**Channel State Information (CSI)** measures how radio waves are affected as they pass through a space. When a person moves between transmitter and receiver, the signal is disturbed in measurable ways. This project captures those disturbances and transforms them into visual noise.

---

## Components

### Hardware
- ESP32-C6 development board(s)
- WiFi router (any standard router)

### Firmware
- [ESPectre](https://github.com/francescopace/espectre) v2.2.0 - WiFi CSI motion detection firmware

### Visualization
- Browser-based real-time video processor
- Sobel edge detection for automatic masking
- CRT magnetic distortion effects

---

## Visual Effects

The distortion simulates how a magnet affects a CRT screen:

1. **Magnetic Warp** - Image bends in wave patterns
2. **RGB Separation** - Color channels separate (chromatic aberration)
3. **Phosphor Scanlines** - Simulates CRT phosphor patterns
4. **Edge Glow** - Detected edges illuminate
5. **Horizontal Tearing** - Lines shift horizontally

---

## Installation

1. Flash ESP32-C6 with ESPectre firmware
2. Configure WiFi credentials in `espectre-nodeX.yaml`
3. Update `PHANTOM_NODE_IP` in `gui/index.html` with your ESP32's IP
4. Open `gui/index.html` in a browser
5. Drop a video file onto the page
6. Move through the space to trigger the effect

### Calibration

Press `[C]` or click "Calibrate" while sitting still to set the baseline for your environment.

### Controls

| Key | Function |
|-----|----------|
| `H` | Toggle UI |
| `C` | Calibrate |
| `Space` | Play/Pause |
| `F` | Fullscreen |

---

## Configuration

Edit the ESP32 IP in the HTML file:

```javascript
const ESP32_IP = 'PHANTOM_NODE_IP';  // Replace with your ESP32's IP
```

### Typical Score Values
- **Still:** 0.5 - 2
- **Light movement:** 3 - 8
- **Strong movement:** 10+

---

## File Structure

```
carried_stillness_intruder/
├── README.md               # This file
├── TECHNICAL.md            # Technical documentation
├── SETUP.md                # Hardware setup guide
├── gui/
│   └── index.html          # Main visualization
├── espectre-node1.yaml     # ESP32 node configs
├── espectre-node2.yaml
├── espectre-node3.yaml
├── espectre-node4.yaml
└── img/                    # Installation photos
```

---

## Attribution

### Original Work
- Visualization code and CRT effects
- Sobel edge detection implementation
- Calibration system

### External Dependencies
- **ESPectre firmware** - Francesco Pace (GPL-3.0) - https://github.com/francescopace/espectre
- **Sobel operator** - Mathematical formula (public domain)

---

## References

- Wu, K., et al. "WiFi-based human activity recognition" (2017)
- Sobel, I. "A 3x3 Isotropic Gradient Operator for Image Processing" (1968)

---

## License

Original code is released under MIT License.
ESPectre firmware is GPL-3.0.

---

*Carried Stillness Intruder - detecting the invisible presence that ripples through the electromagnetic field.*
