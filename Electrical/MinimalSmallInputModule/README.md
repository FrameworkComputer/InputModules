# Microcontroller Input Module
![image](https://user-images.githubusercontent.com/28994301/224575207-4239e2c2-694d-4eab-8f43-b383c9460ddc.png)

This is a basic Raspberry Pi RP2040 based Input Module with a Feather header.  This 
module is designed as a PCB with no other mechanical structure other than SMT 
screw bosses that match the magnets on the host side to pull the module onto the pogo
pins.

The pinout follows the Adafruit Feather RP2040 (https://www.adafruit.com/product/4884)
for simplicity of prototyping.  Note that if you use a Feather module on the board, 
you will of course not be able to close the lid on it.  The intent is to use this board
for prototyping and then design a proper version with SMT components that fit within the 
height limits of the Input Module system.

## License
Input Modules © 2023 by Framework Computer Inc is licensed under CC BY 4.0. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/

## Fabrication and Assembly
We recommend using either a 0.8mm or 1.0mm PCB for Input Modules.

With the exception of the buttons on the top side, all other items are SMT on the bottom side
of the board.

## BOM
You can find alternatives for just about each of these components except for the microcontroller
and the weird reverse-mount LED.

| Item # | Designator                                         | Qty | Manufacturer                        | Mfg Part #               | Description / Value | Package/Footprint                                    | Type |
|--------|----------------------------------------------------|-----|-------------------------------------|--------------------------|---------------------|------------------------------------------------------|------|
| 1      | C1, C2,                                            | 2   | Murata                              | GRM1555C1H150JA01D       | 15pF                | Capacitor_SMD:C_0402_1005Metric                      | SMT  |
| 2      | C3, C4, C5, C7, C9, C10, C11, C12, C13, C14, C17,  | 11  | Samsung                             | CL05B104KO5NNNC          | 100nF               | Capacitor_SMD:C_0402_1005Metric                      | SMT  |
| 3      | C6, C8,                                            | 2   | Samsung                             | CL05A105KO5NNNC          | 1uF                 | Capacitor_SMD:C_0402_1005Metric                      | SMT  |
| 4      | C15, C16,                                          | 2   | TDK                                 | C1608X5R1A106M080AC      | 10uF                | Capacitor_SMD:C_0603_1608Metric                      | SMT  |
| 5      | D1, D2, D3, D4, D5,                                | 5   | Littelfuse                          | SP0402B-ULC-01ETG        | D_TVS               | Diode_SMD:D_0402_1005Metric                          | SMT  |
| 6      | D6,                                                | 1   | DONGGUAN OCCIDENTAL OPTOELECTRONICS | SK6812MINI-E             | SK6812MINI-E        | InputModule:LED_SK6812MINI-E_PLCC4_3.5x3.5mm_P1.75mm | SMT  |
| 7      | R1, R3,                                            | 2   | Yageo                               | RC0402FR-071KL           | 1k                  | Resistor_SMD:R_0402_1005Metric                       | SMT  |
| 8      | R5, R6,                                            | 2   | Panasonic                           | ERJ-2RKF27R0X            | 27                  | Resistor_SMD:R_0402_1005Metric                       | SMT  |
| 9      | R7,                                                | 1   | Yageo                               | RC0402FR-07180KL         | 180k                | Resistor_SMD:R_0402_1005Metric                       | SMT  |
| 10     | U1,                                                | 1   | Winbond                             | W25Q16JVUXIQ             | W25Q16JVUXIQ        | InputModule:SON-8-1EP_3x2mm_P0.5mm_EP0.2x1.6mm       | SMT  |
| 11     | U2,                                                | 1   | Raspberry Pi                        | RP2040TR7                | RP2040              | Package_DFN_QFN:QFN-56-1EP_7x7mm_P0.4mm_EP3.2x3.2mm  | SMT  |
| 12     | Y1,                                                | 1   | Abracon                             | ABM8G-12.000MHZ-18-D2Y-T | 12.000MHz           | Crystal:Crystal_SMD_3225-4Pin_3.2x2.5mm              | SMT  |
| 13     | H1, H2                                             | 2   | Keystone                            | 24929                    | 24929               | InputModule:MountingHole_3.7mm_Pad_24929             | SMT  |
| 14     | SW1, SW2                                           | 2   | C&K                                 | KXT3                     | KXT311LHS           | Button_Switch_SMD:SW_SPST_CK_KXT3                    | SMT  |
