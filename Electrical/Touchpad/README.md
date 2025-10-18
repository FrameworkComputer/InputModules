# Framework 16 Touchpad

For the pinout, check the hubboard's pinout, documented on the main [README](../../README.md).

## Silkscreen

| Label | Type      | Description    |
|-------|-----------|----------------|
| ID    | Testpoint | Board ID       |
| TG    | Testpoint | Ground         |
| TI    | Testpoint | I2C Interrupt  |
| LID   | Testpoint | Reserved       |
| TD    | Testpoint | I2C SDA        |
| TV    | Testpoint | 3.3V           |
| TC    | Testpoint | I2C SCL        |
| TB1   | Testpoint | Button         |
| J1    | Connector | FPC Connector  |
| U4    | IC        | Pixart PCT3854 |
| R1    | Resistor  | Board ID 560k  |
| R2    | Resistor  | I2C SDA Pullup |
| R3    | Resistor  | I2C SCL Pullup |

The I2C pullup resistors are unpopulated, they are present on the system side instead of the touchpad.
If you want to use the touchpad outside of the chassis, you can populate them.
