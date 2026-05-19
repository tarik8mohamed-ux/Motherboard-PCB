# Hyperloop Motherboard PCB
Hyperloop Motherboard PCB Design (KiCad Schematics, component footprints, layouts, and diagrams)

Cornell Hyperloop SP'26
ECC:   Tarik, Peri, Farah, and Vinny

## Introduction: 
This project is a central control PCB designed to serve as the primary communication and motor control hub for Cornell Hyperloop's MiniPod. The board integrates multiple communication protocols to interface with a variety of peripherals: CANopen for servo motor control via the EPOS2 70/10, Step/Dir for stepper motor control via the TB6600, I2C for a distributed sensor network of temperature and current monitors, and PWM for fan speed control. The modular architecture supports hot-pluggable expansion, allowing sensors, MCU, and Multiplexers to be swapped or added without disrupting system operation. The board is ~265mm x 135mm and consists of 4 Layers: Signal-GND-PWR-Signal, with the PWR plane seperated into 3.3V,5.0V, and 12V sections to support different components. The most up to date files are in the Hyperloop Organized PCB folder.

<img width="2447" height="1292" alt="Hyperloop PCB 3D View 2" src="https://github.com/user-attachments/assets/914e8a3b-c1fa-4918-a67b-c81d40fb82c3" />
<img width="3022" height="1585" alt="Hyperloop PCB 3D View" src="https://github.com/user-attachments/assets/1b097068-f752-4173-912d-62b8ce4cb672" />
<img width="3095" height="1633" alt="Hyperloop PCB Planes View" src="https://github.com/user-attachments/assets/27436f15-97ba-4f9e-87ac-6d6f4236aeae" />

## Components:
Cornell Hyperloop's MiniPod is ~7ft, meaning that sesnors and motors must be controlled by the motherbaord from a distance. This is why most components will be connected to the PCB via Cable/Wire connectors.
#### MCU:
The motherboard is controlled with an Arduino Nano R4
<table>
  <tr>
    <td align="center"><b>Arduino Nano R4</b></td>
  </tr>
  <tr>
    <td><img width="200" height="200" alt="1782_sml" src="https://github.com/user-attachments/assets/c92ee6d2-184c-49f4-b817-950f5ef4ed6a"/></td>
  </tr>
</table>

#### Sensors & Fan Control:
Overheating and current spikes are critical concerns on the Pod, so temperature and current sensors are distributed throughout. Temperature sensors use 8-pin Molex connectors (2 power, 2 communication, 1 interrupt, 3 alert pins), while current sensors use 4-pin Molex connectors (2 power, 2 communication pins). Vin+ and Vin− are connected across the points being measured, not the board, and I²C addresses are permanently hardset on the board. Additionally, a TCA9548A I2C multiplexer was integrated to expand the single I2C bus into up to 8 independent channels. 4-pin cooling fans are also distributed across the Pod, represented as 4-pin Molex connectors.
<table>
  <tr>
    <td align="center"><b>Temperature Sensor: MCP9808 </b></td>
    <td align="center"><b>Current Sensor: INA260</b></td>
    <td align="center"><b>Fan: Arctic P12</b></td>
    <td align="center"><b>Multiplexer: TCA9548A</b></td>
  </tr>
  <tr>
    <td><img width="200" height="200" alt="4226" src="https://github.com/user-attachments/assets/b8602e0a-7933-4f16-ba07-426b1ad53a9e"/></td>
    <td><img width="200" height="200" alt="1782_sml" src="https://github.com/user-attachments/assets/ad92f344-274d-4a18-9b53-3f03cfa20f65"/></td>
    <td><img width="200" height="200" alt="1782_sml" src="https://github.com/user-attachments/assets/143efd9a-66ff-4b10-af5a-e45ea73f2d75"/></td>
    <td><img width="200" height="200" alt="1782_sml" src="https://github.com/user-attachments/assets/656c99bf-d054-4f5e-9c2b-1e8aadbedd3d"/></td>
  </tr>
</table>

#### Motor Control:
The Pod interfaces with a TB6600 stepper driver and an EPOS2 70/10 servo controller. The TB6600 is controlled via Step/Dir/Enable signals through a Phoenix Contact screw terminal, and the EPOS2 communicates over CANopen through a Molex Micro-Fit 3.0 4-pin connector.
<table>
  <tr>
    <td align="center"><b>4-pin Pheonix Contact Screw Terminal</b></td>
    <td align="center"><b>Molex Micro-Fir 3.0 4-pin connector</b></td>
  </tr>
  <tr>
    <td><img width="150" height="150" alt="4226" src="https://github.com/user-attachments/assets/6826eae9-7ae4-49f2-a993-ab4b9ddf6feb"/></td>
    <td><img width="150" height="150" alt="1782_sml" src="https://github.com/user-attachments/assets/3028fa0f-1594-4b02-9cf4-479ce2899ac8"/></td>
  </tr>
</table>


#### GPIO Expander:
An MCP23017 I2C expander is included to provide additional GPIO pins for the Pod as needed. Breakout headers expose all 16 I/O pins for flexible peripheral connections.
The motherboard is controlled with an Arduino Nano R4
<table>
  <tr>
    <td align="center"><b>MCP23017 I2C Expander:</b></td>
  </tr>
  <tr>
    <td><img width="100" height="100" alt="1782_sml" src="https://github.com/user-attachments/assets/8368d997-eb95-4fba-876e-13397dab67d7"/></td>
  </tr>
</table>

#### Power, Resistors, Capacitors:
The PCB contains 3.3V,5.0V, and 12.0V sections in the power plane. XT30 connectors will be used to connect external battery packs to drive the power plane.
Resistors typically take on the role of pull-ups in this design. Through-Hole Resistors are used for ease of soldering. On the other hand, capacitors are typically used for bypass or decoupling. In this context, we picked SMD capacitors despite the extra difficulty in soldering because they significantly outperform THT.
<table>
  <tr>
    <td align="center"><b>XT30 Connector:</b></td>
  </tr>
  <tr>
    <td><img width="100" height="100" alt="1782_sml" src="https://github.com/user-attachments/assets/4813de63-e524-4217-bf15-1756d1bc4d30"/></td>
  </tr>
</table>

## Communication Systems:
### I2C Sensor Network & I2C Expander:
The I2C network, located in the top left of the PCB, is managed by a TCA9548A multiplexer which expands the microcontroller's single I2C bus into 4 independent channels, each with its own pull-up resistors to maintain signal integrity. Each channel hosts sensor peripherals, with every device supported by a dedicated decoupling capacitor for noise suppression. A series resistor on the main I2C line dampens inductive ringing before the signal reaches the multiplexer. MCP9808 temperature sensors feature an active-low alert pin connected to a microcontroller GPIO, enabling interrupt-driven temperature monitoring without continuous polling. An MCP23017 I2C I/O expander is also connected to the bus, currently populated with breakout headers for flexible future peripheral connections.

### CAN EPOS2 70/10:
CANopen communication is implemented using a TCAN332 transceiver, which converts the microcontroller's TX/RX signals into differential CANH/CANL signals. A 120Ω termination resistor is placed at the output connector to prevent signal reflections. The bus connects to an EPOS2 70/10 servo controller via a Molex Micro-Fit 3.0 cable. It is located below the MCU.

### Step/Dir TB6600:
The TB6600 stepper driver is controlled via three GPIO signals — PUL+, DIR+, and ENA+ — using a common-cathode wiring configuration. Series resistors on each line dampen signal ringing before reaching the Phoenix Contact screw terminal output connector. It is located just above the MCU.

### PWM Fan CTRL:
Two pairs of fans are controlled via PWM signals from the microcontroller, with each pair sharing a single PWM output to conserve GPIO pins. One tachometer signal per pair is routed back to a microcontroller GPIO, providing RPM feedback for monitoring fan speed. Each fan connector includes dedicated power and ground connections for reliable operation. It is located on the right side of the board.

## Future Updates
-INA260 Alert Pin: The INA260 current sensor connector will be expanded from 4 to 5 pins to expose the ALERT pin, enabling interrupt-driven current monitoring in place of polling across the I2C channels and significantly improving CPU efficiency.    

-TCAN332 Connector Orientation: The CAN connector will be changed from horizontal to vertical to improve accessibility and PCB layout clarity.

-Component Density: Component placement will be optimized for a smaller board footprint, reducing fabrication cost.
