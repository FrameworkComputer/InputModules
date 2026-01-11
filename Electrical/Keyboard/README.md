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

Please refer to the controller's datasheet for additional information about how to
program it.

###### RGB Keyboard

Please refer to the reference code:

- [LED Mapping](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/ansi/ansi.c)

Connected pins on both ICs:

- SW1-SW9
- SW01-SW18

LEDs are arrange like this (Left Address 0x20, Right Address 0x23)

┌─────┬───┬───┬───┬───┬───┬───┬───┬                                ┬───┬───┬───┬────┬───┬─────┐
│43 31│20 │ 8 │ 2 │14 │26 │37 │49 │                                │ 8 │14 │20 │2 38│32 │445 0│
├───┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬                              ┴─┬─┴─┬─┴─┬─┴─┬──┴┬──┴─────┤
│42 │30 │19 │ 7 │ 1 │13 │25 │36 │48 │                                │26 │25 │37 │31 │43  49  │
├───┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴                              ┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬──────┤
│41 29│18 │ 6 │ 0 │12 │24 │35 │47 │                                │ 7 │13 │19 │ 1 │36 │ 42   │
├─────┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴                             ┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴──────┤
│334522│10 │ 4 │16 │28 │39 │51 │                                │ 6 │12 │18 │ 0 │24 │ 30   48 │
├──────┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴                              ┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─────────┤
│32 21 9 │ 3 │15 │27 │38 │50 │                                │ 9 │15 │21 │ 3 │27 │33 45 51 52│
├────┬───┼───┼───┼───┴───┴───┴        ┬───┼       ┼───┴───┴───┴───┴───┼───┼───┼───┴┬───┬──────┤
│    │   │   │   │                    │   │       │                   │   │   │    │39 │      │
│44 5│   │11 │17 │                    │ 5 │       │11 17 23 10  16  22│ 4 │28 │ 34 ├───┤ 46   │
│    │   │   │   │                    │   │       │                   │   │   │    │40 │      │
└────┴   ┴───┴───┴─                   ┴───┴───┴───┴───────────────────┴───┴───┴────┴───┴──────┘

Most keys have one, large keys have multiple RGB LEDs.
LED 0 is controlled by registers 0, 1, 2. LED 1 by 3, 4, 5 and so on.
LED colors are arranged in the order: Blue, Green, Red.

###### Macropad

- [LED Mapping](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/macropad/macropad.c)

Connected pins:

- SW1-SW4
- CS01-CS18

LEDs are arranged like this:

┌──┬──┬──┬──┐
│13│12│17│16│
├──┼──┼──┼──┤
│ 1│ 0│ 5│ 4│
├──┼──┼──┼──┤
│ 7│ 6│11│10│
├──┼──┼──┼──┤
│19│18│23│22│
├──┼──┼──┼──┤
│ 8│20│21│ 9│
├──┼──┼──┼──┤
│ 2│14│15│ 3│
└──┴──┴──┴──┘

Each key has one RGB LED, so LED 0 is controlled by registers 0, 1, 2. LED 1 by 3, 4, 5 and so on.
LED colors are arranged in the order: Blue, Green, Red.

### Keyboard Matrix

Please refer to the reference code:

- ANSI
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/ansi/ansi.h)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/ansi/keymaps/default/keymap.c)
- ISO
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/iso/iso.h)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/iso/keymaps/default/keymap.c)
- JIS
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/jis/jis.h)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/jis/keymaps/default/keymap.c)
- Numpad
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/numpad/numpad.h)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/numpad/keymaps/default/keymap.c)
- Macropad
    - [Matrix](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/macropad/macropad.h)
    - [Keymap](https://github.com/FrameworkComputer/qmk_firmware/blob/v0.3.1/keyboards/framework/macropad/keymaps/default/keymap.c)

## License
Input Modules © 2023-2026 by Framework Computer Inc is licensed under CC BY 4.0. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/
