# Framework One Key Module
![OneKeyModule](/OneKeyModule/Assets/framework_one_key_module_create.png)

The [One Key Module](https://frame.work/products/one-key-module) is a modular building block for the Framework Laptop 16 input system. It allows developers to create custom keyboard layouts, macropads, or individual input triggers using a standardized mechanical and electrical footprint.
This module is designed to be mounted underneath a rigid carrier PCB, with electrical and mechanical connections made via castellated pads soldered upwards into the lattice and secured using four mechanical solder lugs.

> [!CAUTION]
> This documentation is subject to change before the One Key Module launches.

## License
One Key Modules © 2025 by Framework Computer Inc is licensed under CC BY 4.0. To view a copy of this license, visit http://creativecommons.org/licenses/by/4.0/

## Technical Specifications

### Mechanical
<div style="display: flex; gap: 10px;">
   <img src="/OneKeyModule/Assets/one_key_module_layout.png" alt="OneKeyModuleSize" height="300"/>
   <img src="/OneKeyModule/Assets/framework_one_key_module_front.jpg" alt="OneKeyModuleSize" height="300"/>
   <img src="/OneKeyModule/Assets/framework_one_key_module_back.jpg" alt="OneKeyModuleSize" height="300"/>
</div>

* **Key Pitch/Area:** 19.20mm * 19.20mm
* **Keycap Size:** 15.76mm * 15.76mm
* **Key Travel:** 1.50mm +/- 0.2mm
* **PCB Thickness:** 0.15mm
* **Backlight:** 1x White mini-LED
* **Max Z-Height:** 3.65mm (including Mylar).
    * Note: To ensure the laptop closes correctly, the total height of the switch and PCB must not exceed 3.70mm. Please refer to [here](/Mechanical/Small/Module/c1_design_space_ref.pdf).
* **3D Reference:** `One_Key_Module_CAD.stp`

### Electrical
* **Switch Type:** Membrane Switch.
* **Backlight:** Integrated LED support with a dedicated 5V rail and PWM-capable brightness control (`BL_LOW`).
* **Connectivity:** Designed for **Signal Chaining**

### Pinout and Signal Mapping
<img src="/OneKeyModule/Assets/one_key_module_interface.png" alt="OneKeyModulePinout" height="400"/>

The module features a 10-pin interface designed for daisy chaining multiple keys into a larger keyboard matrix.

| Pin | Signal | Description |
| :--- | :--- | :--- |
| 1 | **COL in** | Matrix Column Input |
| 2 | **GND** | Ground |
| 3 | **ROW in** | Matrix Row Input |
| 4 | **ROW Out** | Matrix Row Pass-through |
| 5 | **LED_5V** | 5V Backlight Power |
| 6 | **LED_5V** | 5V Backlight Power |
| 7 | **BL_LOW** | Backlight Control |
| 8 | **BL_LOW** | Backlight Control |````
| 9 | **COL Out** | Matrix Column Pass-through |
| 10 | **GND** | Ground |

## Mounting & Assembly
<img src="/OneKeyModule/Assets/one_key_module_mounting.png" alt="OneKeyModuleMounting" height="400"/>

### Direct-Solder Lattice Method
This module is designed for a **PCB-over-module** assembly:

1.  **Alignment:** The module is positioned underneath a rigid carrier PCB. 
2.  **Mechanical Retention:** Four **mechanical solder lugs** lock into the carrier board to provide structural stability.
3.  **Soldering:** The 10 castellated signal pads and the 4 mechanical lugs(optional) are soldered "upwards." This creates a shared electrical and mechanical bond.
4.  **Signal Chaining:** Use the `ROW Out` and `COL Out` signals to daisy-chain multiple modules across the carrier PCB to form a complete keyboard scanning matrix. LEDs will be parallel.
