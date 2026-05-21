# ESC Firmware

The esc runs on AM32, an open source ESC firmware which manages motor control, communication via dshot, and telemetry.

## Bootloader

First of all, we need to install the bootloader onto each of the chips. For each chip, connect via the exposed swd/io interface, as outlined in the following table, and flash the bootloader hex file (AM32_F421_BOOTLOADER_PB4_V[VERSION].hex)

| MCU | SWDIO | SWDCLK |
|-----|-------|--------|
| 1   | TP1   | TP2    |
| 2   | TP3   | TP4    |
| 3   | TP6   | TP5    |
| 4   | TP8   | TP7    |

## Firmware

Next connect the flight controller (which should be flashed before this step),
visit [am32 configurator](https://am32.ca/) and upload the firmware (AM32_FISHPV_15A_F421_[VERSION].hex)