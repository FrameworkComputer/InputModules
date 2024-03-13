# Framework Laptop 16 Input Modules
![InputModules](https://user-images.githubusercontent.com/28994301/226249081-ab193cfe-4da4-4c47-93ec-c6024edf4fbc.png)

This repository includes mechanical and electrical documentation for the Input Module system in the 
Framework Laptop 16.  Input Modules are hot-swappable USB 2.0-interfaced devices that enable deep
customization of the input system on the Framework Laptop 16.  It's also possible to use these as 
standalone USB 2.0 devices with a simple adapter for development purposes.

Input Modules come in three sizes, each of which has the same electrical interface:
 1. Keyboard-sized modules (283.16mm wide)
 2. Numpad-sized modules (67.85mm wide)
 3. Half-sized modules (33.825mm wide)

For reference firmware for different types of modules, check out these additional repositories:
 * [QMK firmware](https://github.com/frameworkcomputer/qmk_firmware) for the keyboard and numpad modules
 * [inputmodule-rs](https://github.com/FrameworkComputer/inputmodule-rs) firmware and application for other input modules

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

### BOARD_ID values

| Module Type           | ID | Pulldown Resistor (on module) |
|-----------------------|----|-------------------------------|
| Short                 | 0  | NA                            |
| Reserved              | 1  | 10000                         |
| Reserved              | 2  | 18000                         |
| Reserved              | 3  | 27000                         |
| Reserved              | 4  | 39000                         |
| Reserved              | 5  | 56000                         |
| Generic full width    | 6  | 68000                         |
| Reserved              | 7  | 82000                         |
| Generic keyboard size | 8  | 120000                        |
| Generic medium size   | 9  | 150000                        |
| Generic small size    | 10 | 180000                        |
| Medium size keypad    | 11 | 270000                        |
| Keyboard              | 12 | 330000                        |
| Touchpad              | 13 | 560000                        |
| Reserved              | 14 | 820000                        |
| Not installed         | 15 | NA                            |

### Power

Each Input Module supports up to 500mA on the 5V rail and 100mA on the 3.3V rail when active.

When SLEEP# is low or USB is in Selective Suspend mode, modules should drop below 500uA on each rail.  This will typically occur when the
system enters an S0ix state.  In S3/S4/S5 or when the laptop lid is closed, the power rails will typically be off.

The Framework Laptop 16 has a protection scheme in place to prevent Input Modules from powering on unless the input deck is fully populated.
Module detection is done using the BOARD_ID pin. It is possible to override this setting on the system, but at the risk of shorting the system
or modules.

## Touchpad Module

This section describes the Touchpad Module connection on the **system** side, including the pin define and the pin map of the connector.

Pins on the connector have ESD protection to meet IEC 61000-4-2 Level 4 protection.

| Pin | Function | Note                                                |
|-----|----------|-----------------------------------------------------|
| 1   | GND      |                                                     |
| 2   | VCC_5V   | 5V, power supply from the system.                   |
| 3   | I2C_SCL  | I2C Serial Clock Line                               |
| 4   | BOARD_ID | Pull to GND through BOARD_ID resistor defined below |
| 5   | I2C_SDA  | I2C Serial Data Line                                |
| 6   | TP_INT_1 | Secondary interrupt for touchpad future features    |
| 7   | VCC_3V3  | 3.3V, power supply from the system.                 |
| 8   | TP_INT_0 | Main interrupt for touchpad                         |

![image](https://github.com/FrameworkComputer/InputModules/assets/28994301/720e272d-1239-40ba-8b62-a537b74ff71a)

### Touchpad Module layout requirements

The contacts on the Touchpad should be designed so that the ground pins engage first when the Touchpad is sliding in.
Pin 7 should be 0.5mm longer than the other pins to ensure it engages first. 

The below picture for pads is shown in perspective. The view angle is on top of the PCB.

Around the signal pads there should be a ground fill keepout to prevent the pogo connectors from shorting the pads
to ground or other signals when sliding in if the solder mask is scraped away.

![image](https://github.com/FrameworkComputer/InputModules/assets/28994301/e8d65dad-9d55-4d5a-b97c-3ccbd33e1fb6)

