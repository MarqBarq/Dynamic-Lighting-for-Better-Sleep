# Dynamic-Lighting-for-Better-Sleep

Here’s a clean, professional README you can drop into GitHub or share with the blueprint. It’s structured, readable, and avoids fluff.

---

# Dynamic Lighting Blueprint

### Lux-Based Circadian Lighting for Home Assistant

## Overview

This blueprint provides a dynamic lighting system that adjusts brightness and color temperature based on real-world conditions rather than fixed schedules.

Instead of relying on time-of-day alone, this system uses ambient light (lux) to determine how bright your lights should be, while still incorporating a circadian rhythm for color temperature throughout the day.

The result is lighting that feels consistent, natural, and responsive to your environment.

---

## Key Features

* Brightness adjusts dynamically based on room light levels (lux)
* Smooth transitions to avoid abrupt lighting changes
* Automatic shift from cool daylight tones to warm evening tones
* Prevents unnecessary lighting when sufficient natural light is present
* Fully customizable to match room preferences and sensor placement

---

## Requirements

### 1. Lux Sensor (Required)

A sensor that reports ambient light levels (illuminance in lux).

Examples:

* Philips Hue Motion Sensor
* Aqara Motion Sensor (with lux)
* Any Zigbee or Z-Wave illuminance sensor

This blueprint depends on real-time lux readings. Without it, the automation will not function as intended.

---

### 2. Tunable White Lights (Required for full functionality)

Lights must support brightness and color temperature control.

Examples:

* Philips Hue White Ambiance
* LIFX bulbs
* IKEA Tradfri (tunable models)
* Other Zigbee tunable white lights

Lights that only support on/off or dimming will not benefit from color temperature adjustments.

---

### 3. Home Assistant

A working Home Assistant instance with:

* Access to entities (lights and sensors)
* Optional helpers (input_number) for advanced tuning

---

## How It Works

The system maps ambient light levels (lux) to brightness output.

* When the room is dark (low lux), lights increase in brightness
* When the room is bright (high lux), lights dim or remain off
* Color temperature shifts throughout the day to match natural lighting patterns

This creates a continuous, adaptive lighting experience rather than discrete scheduled changes.

---

## Configuration Variables

Below are the key inputs used in the blueprint and how they affect behavior.

---

### lux_sensor

The ambient light sensor used to determine room brightness.

This is the primary input driving the automation.

---

### target_lights

The light or group of lights controlled by the blueprint.

Using a light group is recommended for consistent behavior across multiple fixtures.

---

### min_lux

The lux level at which lights reach maximum brightness.

Example:

* If set to 10, any reading at or below 10 lux will result in full brightness

Lower values make the system wait longer before increasing brightness.

---

### max_lux

The lux level at which lights should be off or at minimum brightness.

Example:

* If set to 300, lights will not activate when the room is already bright

Higher values make the system more aggressive in turning lights off.

---

### min_brightness

The minimum brightness level the lights will use.

Prevents lights from turning completely off in low-light conditions unless explicitly desired.

Typical range: 10–30%

---

### max_brightness

The maximum brightness cap.

This prevents lights from becoming uncomfortably bright.

Typical range: 70–90%

---

### transition_time

The duration (in seconds) for brightness and color changes.

Short transitions (2–5 seconds) feel responsive.
Long transitions (30–1200 seconds) create a gradual ambient effect.

---

### color_temp_day

The color temperature used during daytime hours.

Typical values:

* 4000K–5500K (cooler, more alert lighting)

---

### color_temp_night

The color temperature used during evening/night.

Typical values:

* 2200K–2700K (warmer, more relaxed lighting)

---

### sunset_offset (optional)

Adjusts when the system begins transitioning to warmer tones.

Example:

* -60 minutes starts warming before sunset
* +30 minutes delays warming until after sunset

---

## Tuning Guidelines

Initial recommended settings:

* min_lux: 10
* max_lux: 250
* min_brightness: 20
* max_brightness: 80
* transition_time: 10–60 seconds

Adjust based on experience:

* Lights too dim → increase max_brightness
* Lights turning on too often → increase max_lux
* Lights reacting too quickly → increase transition_time
* Room feels too warm/cool → adjust color temperatures

Expect to refine settings over several days.

---

## Design Philosophy

Most lighting automations are time-based, which assumes consistent environmental conditions. In reality, lighting conditions vary due to weather, seasons, and room layout.

This blueprint prioritizes:

* Real-world input over assumptions
* Gradual change over abrupt transitions
* Comfort over strict automation rules

The goal is for lighting to feel natural and unobtrusive.

---

## Limitations

* Requires a reliable lux sensor for accurate behavior
* Sensor placement significantly affects performance
* Not ideal for rooms with highly inconsistent sensor readings (e.g., direct sunlight on sensor)

---

## Recommended Setup

* Place lux sensor at typical eye level or activity height
* Avoid direct sunlight hitting the sensor
* Use light groups instead of individual bulbs where possible
* Test during different times of day (morning, afternoon, evening)

---

## Future Improvements (Optional Ideas)

* Presence detection to disable automation when rooms are empty
* Adaptive profiles for different rooms (office vs bedroom)
* Integration with sleep/wake routines
* Manual override mode

---

## Final Notes

This system is designed to disappear into the background. When configured correctly, it should not draw attention to itself. Instead, it should make your environment feel consistently comfortable throughout the day.

Adjust slowly, observe behavior, and tune based on lived experience rather than strict values.

---

If you want, I can also:

* Add inline comments directly into your YAML blueprint
* Create a “simple mode” version for less technical users
* Or package this with screenshots and example configs for GitHub publishing

