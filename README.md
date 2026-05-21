# F~ish~PV

![Render](/hardware/cad/render/render-2.png)
_Tinywhoop drone with custom ESC, FC, and frame._

## What?

F~ish~PV is a open source, custom stack, 75mm tinywhoop drone design.
It utilises AM32 for the ESC firmware and Betaflight for the flight controller. 

Custom components include a from-scratch frame, designed for 3d printing (in HP-12-CF filament), a fully custom 15A ESC, and a custom FC. 

**The ESC:**
- 15A Continuous
- 20A Peak for 5s (unverified :O)
- 2S Battery (only)
- AM32 Firmware
- DSHOT Communication + Voltage telemetry
- Current Sense
  
**The FC:**
- STM32F405
- OSD: MAX7456
- IMU: ICM-42688-P
- 128Mbit blackbox
- USB DFU
- 2S Battery (only)
- BEC: 5V/0.5A Max
- Mounting: 25.5x25.5
- Dimensions: 28.3x28.3x10
- UART: 1, 4, 5
- Buzzer!

## Why?

To be honest a lot of this project was just for my own learning, but I also wanted to make a drone that could be generally cheaper than buying prebuilt components. I've mostly achieved this, with a net cost of ~X USD. Unfortunately, some parts I wanted to design from scratch proved to be infeasible within the scope of this project (namely vtx and vrx) so that has driven the total cost up slightly.

Aside from that, drones are cool, and tinywhoops especially so. The main benefit in my opinion is the crashability. The size makes it much more feasible to use alternative manufacturing options. As I mentioned earlier, I designed this with HP-12-CF filament in mind, but HP-12 MJF Nylon is also a very feasible option that could potentially be quite durable (I have simulated both, but it is worth noting that the CF simulations will not be entirely accurate due to the weakness along layer lines not being modelled).

## How to use?

The drone assembly should be reasonably straightforward, I have provided a wonderful exploded diagram that should help significantly:

![Exploded Diagram](https://placehold.co/1280x720?text=Exploded+Diagram&font=opensans)

Aside from the physical assembly, the FC and ESC must be flashed as outlined below or in the respective folders in **firmware/**. As I didn't want to do a bunch of pull requests for a likely one off design, I have provided compiled versions of the firmware. If you want to compile yourself, check out my forks of [Betaflight](https://github.com/cookiemonsternz/betaflight) and [AM32](https://github.com/cookiemonsternz/AM32). AM32 was built using github actions, and Betaflight was built locally using WSL as outlined in [the documentation](https://betaflight.com/docs/development#development).

### ESC Firmware

The esc runs on AM32, an open source ESC firmware which manages motor control, communication via dshot, and telemetry.

#### Bootloader

First of all, we need to install the bootloader onto each of the chips. For each chip, connect via the exposed swd/io interface, as outlined in the following table, and flash the bootloader hex file from **firmware/esc/bootloader/**

| MCU | SWDIO | SWDCLK |
|-----|-------|--------|
| 1   | TP1   | TP2    |
| 2   | TP3   | TP4    |
| 3   | TP6   | TP5    |
| 4   | TP8   | TP7    |

#### Firmware

Next connect the flight controller (which should be flashed before this step),
visit [am32 configurator](https://am32.ca/) and upload the firmware from **firmware/esc/**

---

### FC Firmware

Installation follows the same procedure as pretty much any flight controller.
Visit [Betaflight Configurator](https://app.betaflight.com/) and select firmware flasher. 

At the bottom of the screen, press load local:
![Configurator](https://cdn.hackclub.com/019e4968-55de-7d24-863c-0b865c099ec0/paste-1779348032040.png)
Upload **betaflight_2026.6.0-alpha_STM32F405_FISHPV_F405** and then hit flash!

That's it, should be all configured (maybe) but I'd reccomend going through and doing the regular adjustments and calibration.

## Renders

![Render 1](./hardware/cad/render/render.png)
![Render 2](https://placehold.co/1280x720?text=Render&font=opensans)
![Render 3](https://placehold.co/1280x720?text=Render&font=opensans)

## Zine Page

## Simulations

I ran a handful of simulations in FreeCAD, and these are just a small sample of the results I got. 

First up, a collision where the battery is impacted. Think the drone just dropping onto the ground. This is for a collision at approximately 50km/h assuming a collision time of 25ms and a net mass of around 250g (so probably a bit extreme), to be specific, I modelled a force of 150N.
![Battery collision](https://cdn.hackclub.com/019e4a19-c032-76d2-89ff-21970fd752ff/paste-1779359660120.png)
As you can see there is even deflection across all four mounting posts for the FC/ESC stack, which will result in minimal tensile strain (which is the typical cause of damage)

Another type of collision I modelled is just a collision directly to the prop guards. Again, a 150N force which would be ~50km/h as mentioned above.
![Prop guard collision](https://cdn.hackclub.com/019e4a1c-1d99-70b0-8190-ee19058ad92c/paste-1779359815752.png)
There is a lot of displacement on the prop guard but it is effectively dissipated and won't have much impact on the electronics, which is the main thing to be worried about. 
For this specific impact, I would be reasonably worried about damage to the frame, but for most crashes (which are probably not at 50km/h) it should be perfectly fine.

Many more simulations at different loads and also modelling the HP-12 MJF material can be found [here](https://github.com/cookiemonsternz/drone/blob/main/journals/journal-15-05-2026.md).

## BOM

## Cad Links

All of the CAD source files can be found on [Onshape](https://cad.onshape.com/documents/5b69b7ad5dc2bcb1a562cae6/w/28cfd389aaff8b3000581458/e/21531bda6c88e5e5db7731e6?configuration=List_q74UD91e5eSlmt%3DDefault&renderMode=0&uiState=6a005b50d3c7c0f6100b1ec5). 
The physics simulations can be found in the FreeCAD project under **hardware/cad/sim/**

## Directory Structure

- **firmware/**
  - **esc/** - ESC Firmware (AM32)
  - **fc/** - FC Firmware (Betaflight)
- **hardware/**
    - **bom/** - BOM Files (CSV and LibreOffice Calc)
    - **cad/** - CAD Files
      - **render/** - Files for rendering (inc. gltf models)
      - **sim/** - FreeCAD files for FEM Analysis
    - **esc/** - ESC KiCad Project
      - **production/** - Production files (Gerber, etc.)
    - **fc/** - FC KiCad Project
      - **production/** - Production files (Gerber, etc.)
    - **lib** - Shared KiCad libraries
- **journals/**
- **zine/** - Zine page :)

## References
[Betaflight Documentation](https://betaflight.com/docs/development)
[AM32 Documentation](https://wiki.am32.ca/general/docs.html) - Especially re [Hardware Design](https://wiki.am32.ca/development/Hardware-Design.html)
[AcciFPV FC](https://www.pcbway.com/project/shareproject/Acci_FPV_Flight_Controller_Betaflight_STM32F405_AT32F435_48fb0ecb.html)
[Hack Club Blueprint - FC Guided Project](https://blueprint.hackclub.com/starter-projects/flightcontroller)
[JustFPV ESC Design](https://www.youtube.com/watch?v=TwAmmPxOpTM)
AM32 & Betaflight Discords
And various others which I have forgotten.