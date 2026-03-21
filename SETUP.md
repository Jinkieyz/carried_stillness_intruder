# Hardware Setup Guide

## Hardware Requirements

- 4x ESP32-C6 development boards
- Two USB-C ports on each: "CH343" (UART) and "ESP32C6" (direct USB)
- Use the "ESP32C6" port for flashing

## Firmware

- **ESPectre** - WiFi CSI-based motion detection
- Source: https://github.com/francescopace/espectre
- Version: 2.2.0
- Framework: ESP-IDF

---

## Node Configuration

| Node | Name | Fallback AP |
|------|------|-------------|
| 1 | espectre-node1 | CSI-Node1-Fallback |
| 2 | espectre-node2 | CSI-Node2-Fallback |
| 3 | espectre-node3 | CSI-Node3-Fallback |
| 4 | espectre-node4 | CSI-Node4-Fallback |

---

## Configuration Files

Edit the YAML files to set your network credentials:

- `espectre-node1.yaml`
- `espectre-node2.yaml`
- `espectre-node3.yaml`
- `espectre-node4.yaml`

### Placeholders to Replace

| Placeholder | Description |
|-------------|-------------|
| `LIMINAL_VEIL` | Your WiFi network name (SSID) |
| `whispered_frequency` | Your WiFi password |
| `ethereal_passage` | OTA update password |
| `threshold_one` (etc) | Fallback AP passwords |
| `PHANTOM_NODE_IP` | ESP32 IP address |

---

## ESPHome Environment

```bash
# Create virtual environment
python3 -m venv esp_venv
source esp_venv/bin/activate

# Install ESPHome
pip install esphome

# Compile firmware
esphome compile espectre-node1.yaml

# Flash to device
esphome upload espectre-node1.yaml --device /dev/ttyACM0
```

---

## Verification

After flashing, the node should:
- Connect to your WiFi network
- Provide a web interface on port 80
- Expose the movement score API at `/sensor/movement_score`

### Test the API

```bash
curl http://YOUR_ESP32_IP/sensor/movement_score
```

Expected response:
```json
{"id":"sensor-movement_score","value":1.23,"state":"1.23"}
```

---

## Troubleshooting

### Node Not Connecting to WiFi
- The node automatically enters fallback AP mode
- Connect to the fallback network and configure via web interface

### No CSI Data
- Ensure the node is within range of the router
- Check that the router supports the required WiFi standards

### First Compilation Takes Long Time
- Initial build downloads ESP-IDF (~5.7GB)
- Subsequent builds take ~6 seconds

### No Power LED
- Many ESP32-C6 boards have no power LED - this is normal behavior

---

## Notes

- No Raspberry Pi required - browser connects directly to ESP32 via HTTP REST API
- Polling interval: 30ms for responsive motion detection
