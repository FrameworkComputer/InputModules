# Framework Laptop 16 Input Modules
This repository includes mechanical and electrical documentation for the Input Module system in the Framework Laptop 16.

For reference firmware for different types of modules, check out these additional repositories:
 * TODO
 
Input Modules are hot-swappable USB 2.0-interfaced devices that enable deep customization of the input system on the
Framework Laptop 16.  It's also possible to use these as standalone USB 2.0 devices with a simple adapter for development
purposes.

Input Modules come in three sizes, each of which has the same electrical interface:
 1. Keyboard-sized modules (283.16mm wide)
 2. Numpad-sized modules (67.85mm wide)
 3. Half-sized modules (33.825mm wide)

**Warning:** the documentation here is pretty early, so there may be minor adjustments in the mechanical or electrical designs
before the Framework Laptop 16 launches.  We'll let you know when the design is locked for production.
 
## License
Input Modules © 2023 by Framework Computer Inc is licensed under CC BY 4.0. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/

## Mechanical

In the Mechanical folder, we have 2D drawings of the different Input Module sizes.  Note that there are two version of each:
 1. A version with full mechanical structure with a separate interface PCB and brackets.  This is what our production modules are based on.
 2. A simplified version that allows the PCB to be used as the mechanical structure of the module, making it much cheaper and easier to make.

## Electrical

In the Electrical folder, we have a reference version of a minimal RP2040-based Input Module, made in KiCAD.

### Pinout

| Pin | Function | Note                                                              |
|-----|----------|-------------------------------------------------------------------|
| 1   | GND      |                                                                   |
| 2   | VCC_5V   | 5V, power supply from the system.                                 |
| 3   | USB_DN   | USB D-                                                            |
| 4   | BOARD_ID | Pull to GND through BOARD_ID resistor defined below               |
| 5   | USB_DP   | USB D+                                                            |
| 6   | SLEEP#   | 3.3V if the host is in S0.  0V if the host is in S0ix, S3, S4, S5 |
| 7   | GND      |                                                                   |
| 8   | VCC_3V3  | 3.3V, power supply from the system.                               |

Viewed from top:

![image](https://user-images.githubusercontent.com/28994301/223607129-ab8e1dcf-dd1f-49f1-9e67-03e9ca072348.png)

### Power

Each Input Module supports up to 500mA on the 5V rail and 100mA on the 3.3V rail when active.

When SLEEP# is low or USB is in Selective Suspend mode, modules should drop below 500uA on each rail.  This will typically occur when the
system enters an S0ix state.  In S3/S4/S5 or when the laptop lid is closed, the power rails will typically be off.

The Framework Laptop 16 has a protection scheme in place to prevent Input Modules from powering on unless the input deck is fully populated.
Module detection is done using the BOARD_ID pin. It is possible to override this setting on the system, but at the risk of shorting the system
or modules.
