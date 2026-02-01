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

## License
Input Modules © 2023-2026 by Framework Computer Inc is licensed under CC BY 4.0. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/

## Firmware and Software

### Keyboards

All keyboards (ANSI, ISO, Numpad, Macropad) run QMK firmware.
Find the latest binary releases and source code in our [FrameworkComputer/qmk_firmware](https://github.com/FrameworkComputer/qmk_firmware/releases) repository.

To configure keybindings, use our hosted VIA fork, which has the right configurations built in at https://keyboard.frame.work.
Or the [qmk_hid](https://github.com/FrameworkComputer/qmk_hid) native Python or Rust software GUI and console applications.

HALs for RTOS or baremetal environments:

- [rust-embedded](https://github.com/rp-rs/rp-hal-boards/tree/main/boards/framework16-keyboard)
  - [capslock Sample](https://github.com/rp-rs/rp-hal-boards/blob/main/boards/framework16-keyboard/examples/capslock.rs)
  - [white_backlight Sample](https://github.com/rp-rs/rp-hal-boards/blob/main/boards/framework16-keyboard/examples/white_backlight.rs)
- [Zephyr](https://docs.zephyrproject.org/latest/boards/framework/laptop16_keyboard/doc/index.html)

IS31FL3743 LED Controller Drivers:

- [QMK](https://docs.qmk.fm/drivers/is31fl3743a)
- [rust-embedded](https://docs.rs/is31fl3743a/latest/is31fl3743a/)
- [CircuitPython](https://github.com/FrameworkComputer/CircuitPython_IS31FL3743)

### LED Matrix

Official firmware that ships with the modules is available in source and binary at [FrameworkComputer/inputmodule-rs](https://github.com/FrameworkComputer/inputmodule-rs).
That same repository also contains a Rust commandline utility and Python scripts to control the matrix.

HALs for RTOS or baremetal environments:

- [rust-embedded](https://github.com/rp-rs/rp-hal-boards/tree/main/boards/framework-ledmatrix)
  - [ledtest Sample](https://github.com/rp-rs/rp-hal-boards/blob/main/boards/framework-ledmatrix/examples/ledtest.rs)
- [Zephyr](https://docs.zephyrproject.org/latest/boards/framework/framework_ledmatrix/doc/index.html)

IS31FL3741 LED Controller Drivers:

- [QMK](https://docs.qmk.fm/drivers/is31fl3741)
- [rust-embedded](https://docs.rs/is31fl3741/latest/is31fl3741)
- [CircuitPython](https://github.com/adafruit/Adafruit_CircuitPython_IS31FL3741)

Third party applications that interact with the official firmware:

- [https://github.com/jpadgett314/led-matrix-vocab](jpadgett314/led-matrix-vocab) - A japanese vocab trainer

Third party firmware projects that run on the official hardware:

- [https://github.com/sigroot/FW_LED_Matrix_Firmware](sigroot/FW_LED_Matrix_Firmware) - Arduino based firmware with an alternative host protocol
- [https://github.com/vddCore/sparkle-fw16](vddCore/sparkle-fw16) - Pico SDK based firmware with an alternative host protocol

### CircuitPython

CircuitPython can run on all modules.

Sample Code:

- [github.com/FrameworkComputer/Framework_Inputmodule_CircuitPython](https://github.com/FrameworkComputer/Framework_Inputmodule_CircuitPython)

Libraries:

- [Keyboard RGB Controller Adafruit_CircuitPython_IS31FL3741](https://github.com/adafruit/Adafruit_CircuitPython_IS31FL3741)
- [Keyboard RGB Controller - Framework CircuitPython_IS31FL3743](https://github.com/FrameworkComputer/CircuitPython_IS31FL3743)

## Mechanical

In the Mechanical folder, we have 2D drawings of the different Input Module sizes.  Note that there are two version of each:
 1. A version with full mechanical structure with a separate interface PCB and brackets.  This is what our production modules are based on.
 2. A simplified version that allows the PCB to be used as the mechanical structure of the module, making it much cheaper and easier to make.
    Note that to make the PCB attach securely in the system, you'll need to solder SMT nuts like Keystone	24929 or to adhere a steel plate to attract to
    the magnets in the system.

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

### Module Type Detection

Embedded Controller firmware detects location, type and size of modules by
measuring the value of a pull down resistor on each connector.
This detection is only used for diagnostics and power sequencing (see below).
USB functionality is enabled on any connector no matter the module type or presence.

| ID | Row    | Module Type          | Size | Module Pulldown Resistor |
|----|--------|----------------------|------|--------------------------|
| 0  |        | Short                |      | NA                       |
| 1  |        | Reserved             |      | 10000                    |
| 2  |        | Reserved             |      | 18000                    |
| 3  |        | Reserved             |      | 27000                    |
| 4  |        | Reserved             |      | 39000                    |
| 5  |        | Reserved             |      | 56000                    |
| 6  | Top    | Generic Full Width   | Full | 68000                    |
| 7  |        | Reserved             |      | 82000                    |
| 8  | Top    | Generic A size       | A    | 120000                   |
| 9  | Top    | Generic B size       | B    | 150000                   |
| 10 | Top    | Generic C size       | C    | 180000                   |
| 11 | Top    | Numpad/Macropad      | B    | 270000                   |
| 12 | Top    | Keyboard             | A    | 330000                   |
| 13 | Bottom | Touchpad             | B    | 560000                   |
| 14 |        | Reserved             |      | 820000                   |
| 15 | Both   | Not installed        |      | NA                       |

The LED matrix and plastic spacers are both "Generic C size".

### Power

Each Input Module supports up to 500mA on the 5V rail and 100mA on the 3.3V rail when active.

The Framework Laptop 16 has a protection scheme in place to prevent Input Modules from powering on unless the input deck is fully populated.
Module detection is done using the `BOARD_ID` pin. It is possible to override this setting on the system through BIOS settings, but at the risk of shorting the system or modules.

System firmware expects that pogo connector where the module presents the board
ID, is at the very left of the module. It uses this assumption to calculate
size and position of modules and detect if the input deck is fully populated
and all pogo pins are covered.

The following top-row combinations are accepted by firmware:

| Conn 1 | Conn 2 | Conn 3 | Conn 4 | Conn 5 |
|--------|--------|--------|--------|--------|
| Full   |        |        |        |        |
|        | Full   |        |        |        |
|        |        | Full   |        |        |
|        |        |        | Full   |        |
|        |        |        |        | Full   |
| A      |        |        | B      |        |
| A      |        |        | C      | C      |
| C      | A      |        |        | C      |
| B      |        | A      |        |        |
| C      | C      | A      |        |        |

#### `SLEEP#` pin behavior

| Platform              | BIOS | `SLEEP#`        |
|-----------------------|------|-----------------|
| AMD Ryzen 7040 Series | 3.XX | Lid and Suspend |
| AMD Ryzen 7040 Series | 4.XX | Lid state       |
| AMD AI 300 Series     | Any  | Lid state       |

On the first generation Framework 16 with BIOS 3.XX the `SLEEP#` pin is low
whenever the system is in S0ix (suspend) state or the lid is closed.

On the 2nd gen or 1st gen with BIOS 4.XX the `SLEEP#` pin is only low if the lid is closed.
This change was made because the keyboard and touchpad firmware couldn't decide between

When SLEEP# is low or USB is in Selective Suspend mode, modules should drop below 500uA on each rail.  This will typically occur when the
system enters an S0ix state. In S3/S4/S5 or when the laptop lid is closed, the power rails will typically be off.

F2 on boot > Setup Utility > Advanced > Force Power for Input Modules:

- Force Off: Power always off
- Require Modules (default): Power on only if all modules are present
- Force On: Power always on

In the case of Force On, there is a risk of damage when the pins are exposed and come into contact with conductive material.

## Touchpad Module

This section describes the Touchpad Module connection on the **system** side, including the pin define and the pin map of the connector.
There are three connectors for the touchpad, to allow moving it around. They are all shorted together on the same I2C bus with the same interrupt lines.

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

### Touchpad I2C Protocol

The I2C bus from the touchpad is connected to the CPU for HID over I2C and
implements the [Precision Touchpad Protocol](https://learn.microsoft.com/en-us/windows-hardware/design/component-guidelines/touchpad-protocol-implementation).

It's also connected to the EC. This is used when booting without a touchpad.
If the Windows driver tries to connect to an I2C device but finds it does not respond, it will disable that device.
So we make the [EC pretend](https://github.com/FrameworkComputer/EmbeddedController/commit/9d49389919c36e44e451514b8278b9eb7ee6ed1e)
to be the touchpad and send the same HID report descriptor as the touchpad would.

## One Key Module

See [One Key Module](/OneKeyModule).
