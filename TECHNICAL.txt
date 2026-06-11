# Technical Documentation

## System Architecture

A real-time system using WiFi CSI (Channel State Information) to detect movement in a room and apply visual distortion effects to video. When someone moves between the WiFi router and an ESP32 sensor, the video responds with authentic CRT-like magnetic interference.

---

## WiFi CSI (Channel State Information)

WiFi CSI measures how radio waves are affected as they pass through a space. When a person moves between transmitter and receiver, the signal is disturbed in measurable ways.

### How It Works

The ESP32-C6 continuously monitors WiFi packets and extracts CSI data from them. This data contains amplitude and phase information for each subcarrier in the WiFi signal. Movement causes variations in these values, which are processed into a single "movement score".

### References
- Wu, K., et al. "WiFi-based human activity recognition" (2017)
- https://github.com/francescopace/espectre

---

## Edge Detection (Sobel Operator)

The algorithm uses the Sobel operator to detect edges in video frames. This creates automatic masks around objects in the image.

### Algorithm

```javascript
// Sobel kernels for gradient in X and Y
const gx = -gray[(y-1)*width+(x-1)] + gray[(y-1)*width+(x+1)] +
           -2*gray[y*width+(x-1)] + 2*gray[y*width+(x+1)] +
           -gray[(y+1)*width+(x-1)] + gray[(y+1)*width+(x+1)];

const gy = -gray[(y-1)*width+(x-1)] - 2*gray[(y-1)*width+x] - gray[(y-1)*width+(x+1)] +
           gray[(y+1)*width+(x-1)] + 2*gray[(y+1)*width+x] + gray[(y+1)*width+(x+1)];

edges[idx] = Math.sqrt(gx*gx + gy*gy);
```

### Source
- Sobel, I. "A 3x3 Isotropic Gradient Operator for Image Processing" (1968)
- https://en.wikipedia.org/wiki/Sobel_operator

---

## CRT Magnetic Distortion

The effect simulates how a magnet affects a CRT screen (cathode ray tube):

1. **Magnetic Warp** - Image bends in wave patterns
2. **RGB Separation** - Color channels separate (chromatic aberration)
3. **Phosphor Scanlines** - Simulates CRT phosphor patterns
4. **Edge Glow** - Edges light up
5. **Horizontal Tearing** - Lines shift horizontally

### Inspiration
- Vintage CRT monitors and televisions
- Datamoshing techniques: https://ompuco.wordpress.com/2018/03/29/creating-your-own-datamosh-effect/

---

## Calibration System

The calibration system measures the baseline CSI values when no movement is present, then sets a threshold for triggering the effect.

### Process

1. User initiates calibration (press C or click Calibrate)
2. System samples CSI values for ~3 seconds
3. Mean value becomes the baseline
4. Threshold is set to baseline + 0.5
5. Movement above threshold triggers visual effects

### Parameters

- **Baseline**: Average CSI score during stillness
- **Threshold**: Margin above baseline to trigger (default +0.5)
- **Normalization**: Full effect at +2 above baseline

---

## What Is Original vs External

### Original Work
- `csi_datamosh_FINAL_BEST.html` - Main visualization
- Sobel edge detection in JavaScript
- CRT magnetic distortion effect
- Auto-mask system
- Threshold calibration

### External
- **ESPectre firmware** - https://github.com/francescopace/espectre (GPL-3.0)
- **Sobel algorithm** - Mathematical formula (public domain)

---

## API

### ESP32 Endpoint

```
GET http://ESP32_IP/sensor/movement_score
```

Response:
```json
{
  "id": "sensor-movement_score",
  "value": 4.135574,
  "state": "4.14"
}
```

### Polling

The browser polls the ESP32 every 30ms for low-latency response.

---

## Future Development

- [ ] Support for multiple ESP32 nodes
- [ ] WebGL shader version for better performance
- [ ] Record processed video output
