# Testing include_last_color Feature

## Configuration Example

Add this to your device configuration to enable the feature:

```json
{
  "devices": [
    {
      "id": "your_light_device_id",
      "converters": {
        "light": {
          "include_last_color": true
        }
      }
    }
  ]
}
```

Or set it as default for all lights:

```json
{
  "defaults": {
    "converters": {
      "light": {
        "include_last_color": true
      }
    }
  }
}
```

## Expected Behavior

### Without include_last_color (default: false)
When you turn on a light from HomeKit:
- MQTT message: `{"state": "ON"}`
- The light turns on with whatever color it was last set to (device memory)

### With include_last_color: true
When you turn on a light from HomeKit:
- MQTT message for color_hs: `{"state": "ON", "color": {"hue": 120, "saturation": 100}}`
- MQTT message for color_xy: `{"state": "ON", "color": {"x": 0.17242, "y": 0.7468}}`
- The light turns on AND explicitly sets the last known color from HomeKit

## Use Case

This is particularly useful for:
- Lights that don't remember their color state when powered off/on
- Ensuring consistent color state between HomeKit and the physical device
- Smart bulbs that reset to white/default color on power cycle

## Debug Logging

When the feature is active, you'll see debug logs like:
```
include_last_color: LightName: including color (hue: 120, saturation: 100)
```
or
```
include_last_color: LightName: including color (x: 0.17242, y: 0.7468)
```
