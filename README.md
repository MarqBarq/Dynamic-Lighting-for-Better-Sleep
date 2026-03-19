# Dynamic-Lighting-for-Better-Sleep

Here’s your updated README section with the 
### Lux-Based Circadian Lighting for Home Assistant

## Overview

This blueprint provides a dynamic lighting system that adjusts brightness and color temperature based on real-world conditions rather than fixed schedules.

Instead of relying on time-of-day alone, this system uses ambient light (lux) to determine how bright your lights should be, while still incorporating a circadian rhythm for color temperature throughout the day.

It also accounts for a critical real-world factor often overlooked in lighting automation: **perceived brightness**.

A room can technically measure as “bright enough” in lux, but still *feel* dim due to cloud cover, storms, or indirect sunlight. This blueprint allows you to tune for that difference.

The result is lighting that feels consistent, natural, and responsive to both measured and perceived conditions.

---

## Key Features

* Brightness adjusts dynamically based on room light levels (lux)
* Smooth transitions to avoid abrupt lighting changes
* Automatic shift from cool daylight tones to warm evening tones
* Prevents unnecessary lighting when sufficient natural light is present
* Adjustable thresholds to compensate for cloudy or low-contrast lighting conditions
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

### Accounting for “Perceived Darkness”

Lux readings alone do not always reflect how a space feels.

For example:

* A sunny room with direct light may feel bright at 250 lux
* A cloudy or stormy day may also read 250 lux, but feel noticeably dim

This blueprint allows you to compensate for that by adjusting your thresholds so that:

* Lights can activate earlier on overcast days
* Rooms maintain a consistent “feel,” not just a consistent measurement

This is achieved through tuning `max_lux`, `min_lux`, and brightness ranges to match your environment and preferences.

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

The lux level at which lights reach maximum brightness. (Anything under XXX Lux, I want the lights fully on)

Example:

* If set to 10, any reading at or below 10 lux will result in full brightness

Lower values make the system wait longer before increasing brightness.

---

### max_lux

The lux level at which lights should be off or at minimum brightness. (Anything over XX Lux, the lights should be off)

This is also your primary control for handling cloudy or overcast conditions.

Example:

* Lower value (200–250): lights activate sooner on cloudy days
* Higher value (300–400): lights stay off longer, even if the room feels dim

---

### min_brightness

The minimum brightness level the lights will use. 

This is because some lights, like the HUE do not really have fine control until 20%, and so that is the where the low starts

Typical range: 10–30%

---

### max_brightness

The maximum brightness cap.

This prevents lights from becoming uncomfortably bright. Not everyone wants the lights at 100% after dark

Typical range: 70–90%

---

### transition_time

The duration (in seconds) for brightness and color changes.

Short transitions (2–5 seconds) feel responsive but is quite distracting. 
Long transitions (30–1200 seconds) create a gradual ambient effect.

---

### color_temp_day

The color temperature used during daytime hours. This will have a more bright sunny quality.

Typical values:

* 4000K–5500K (cooler, more alert lighting)

---

### color_temp_night

The color temperature used during evening/night. More warm and better for long term sleep quality

Typical values:

* 2200K–2700K (warmer, more relaxed lighting)

---

### sunset_offset (optional)

Adjusts when the system begins transitioning to warmer tones. This is simply when do you want it to start it's transition from cool to warm

Example:

* -60 minutes starts warming before sunset
* +30 minutes delays warming until after sunset

---

## Tuning Guidelines

Initial recommended settings:

* min_lux: 1000
* max_lux: 2500
* min_brightness: 20
* max_brightness: 80
* transition_time: 30 seconds

### Adjusting for Weather and Room Feel

* Room feels too dark on cloudy days → lower `max_lux`
* Lights staying off when you want them on → lower `max_lux`
* Lights turning on too aggressively → raise `max_lux`
* Room feels flat or dim even when lights are on → increase `max_brightness`

Expect to refine settings over several days, especially across different weather conditions.

---

## Design Philosophy

Most lighting automations are time-based, which assumes consistent environmental conditions. In reality, lighting conditions vary due to weather, seasons, and room layout. The system builds a dimming profile based on your `max_lux` and `min_lux`. This means if your MAX (no lights on above) 2500, and your MIN was set at 1000 (Max Brightness), and the lux reading was 2000, that is 33% below the Lus curve (1000 - 2500), and so the system would turn on the lights at 33% of your MIN Brightness and MAX Brightness. If a storm rolls in, and it pushes the lux to 1200, that is 86% from lights off, the system would set the lights to 86%. As soon as the storm passed it would ramp them back to off. As the sun sets, it moves the lights from a cool white to a warm white. 

The automation start and stop times are designed for mornings starts and after dark, when other automations may be used to turn off or change the lights this controls. 

This blueprint prioritizes:

* Real-world input over assumptions
* Perceived comfort over raw sensor values
* Gradual change over abrupt transitions

The goal is for lighting to feel natural and unobtrusive, regardless of external conditions.

---

## Limitations

* Requires a reliable lux sensor for accurate behavior
* Sensor placement significantly affects performance
* Does not directly integrate weather APIs (adjustment is done through tuning)
* Not ideal for rooms with highly inconsistent sensor readings (e.g., direct sunlight on sensor)

---

## Recommended Setup

* Place lux sensor at typical eye level or activity height
* Avoid direct sunlight hitting the sensor
* Use light groups instead of individual bulbs where possible
* Test during different times of day and weather conditions

---

## Future Improvements (Optional Ideas)

* Direct weather condition integration (cloud cover, rain)
* Presence detection to disable automation when rooms are empty
* Adaptive profiles for different rooms (office vs bedroom)
* Manual override mode

---

## Final Notes

This system is designed to disappear into the background. When configured correctly, it should not draw attention to itself. Instead, it should make your environment feel consistently comfortable throughout the day.

The addition of perceived brightness tuning is what elevates this beyond a standard lux-based system. It allows your lighting to adapt not just to measurements, but to how a space actually feels to the people in it.

Adjust slowly, observe behavior, and tune based on lived experience.
