# Framework Laptop 16 Keyboard

We are shipping 4 different keyboard types:

- RGB ANSI Keyboard
- White backlight ANSI/ISO Keyboard
- White backlight Numpad
- RGB Macropad

All of them are powered by a Raspberry Pi RP2040 microcontroller.

## Pinouts

Below are the pinouts of the mass production units.

### RP2040 MCU

| Pin      | Name       | To            | Direction | Config             | Modules                |
|----------|------------|---------------|-----------|--------------------|------------------------|
| GPIO0    | Sleep      | EC            | IN        | Pullup, low active | All                    |
| GPIO1    | MUX_A      | SGM48751 Mux  | OUT       |                    | All                    |
| GPIO2    | MUX_B      | SGM48751 Mux  | OUT       |                    | All                    |
| GPIO3    | MUX_C      | SGM48751 Mux  | OUT       |                    | All                    |
| GPIO4    | MUX_ENABLE | SGM48751 Mux  | OUT       |                    | All                    |
| GPIO5    | BOOT_DONE  |               |           |                    | All                    |
| GPIO6    | Reserved   | KSI5          | High-Z    | Unused             | All                    |
| GPIO7    | Reserved   | KSI5          | High-Z    | Unused             | All                    |
| GPIO8-23 | KSO1-15    |               |           |                    | All                    |
| GPIO24   | CAPS_LED   | Capslock LED  | OUT       | High Active        | RGB & White Keyboard   |
| GPIO25   | BACKLIGHT  | Backlight LED | OUT       | High Active        | White Keyboard, Numpad |
| GPIO26   | I2C1_SDA   | IS31FL3743A   | IN/OUT    |                    | RGB Keyboard, Macropad |
| GPIO27   | I2C1_SCL   | IS31FL3743A   | OUT       |                    | RGB Keyboard, Macropad |
| GPIO28   | ANALOG_IN  | ISGM48751 Mux | IN        | KSI scan ADC Input | All                    |
| GPIO29   | SDB        | IS31FL3743A   | OUT       | High active        | RGB Keyboard, Macropad |


### IS31FL3743A LED Controller

[Documentation](https://www.lumissil.com/applications/industrial/appliance/major-appliances/range-hood/is31fl3743a)
and [Datasheet](https://www.lumissil.com/assets/pdf/core/IS31FL3743A_DS.pdf)
of this controller are publicly available.

On the RGB keyboard two controllers are needed to drive all LEDs. The Macropad only has a single one.

| Pin       | To          | Direction | Config          |
|-----------|-------------|-----------|-----------------|
| SDA       | RP2040      | IN/OUT    |                 |
| SCL       | RP2040      | IN        |                 |
| SDB       | RP2040      | IN        | High active     |
| ISET      |             |           |                 |
| ADDR      |             |           |                 |
| SW1-SW9   | LED         | OUT       | See table below |
| SW10-SW11 | NC          |           | Unused          |
| CS1-CS18  | LED         | OUT       | See table below |
| SYNC      | IS31FL3743A | IN/OUT    |                 |

I2C Addresses:

- RGB Keyboard: 0x20 and 0x23
- Macropad: 0x20

Below is the table describing how the LEDs are placed on the matrix
and how they are wired up to the LED controller.

- X is the X location in the matrix (from left to right)
- Y is the Y location in the matrix (from bottom to top)
- ID is the numerical identifier of the LED
- I2C is the address of the controller, either 0x20 or 0x23
- Address and Page describe the memory location in the IS31FL3743A's registers
- SW and CS are the pins

Please refer to the controller's datasheet for additional information about how to
program it.

###### RGB Keyboard

Please refer to the reference code:

- [LED Mapping](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/ansi/ansi.c)

###### Macropad Keyboard

- [LED Mapping](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/macropad/macropad.c)

### Keyboard Matrix

Please refer to the reference code:

- ANSI
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/ansi/info.json)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/ansi/keymaps/default/keymap.c)
- ISO
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/iso/info.json)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/iso/keymaps/default/keymap.c)
- JIS
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/jis/info.json)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/jis/keymaps/default/keymap.c)
- Numpad
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/numpad/info.json)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/numpad/keymaps/default/keymap.c)
- Macropad
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/macropad/info.json)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/lotus/keyboards/framework/laptop16/macropad/keymaps/default/keymap.c)

## License
Input Modules © 2023 by Framework Computer Inc is licensed under CC BY 4.0. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/
