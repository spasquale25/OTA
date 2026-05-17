# C2S2 RFIC Subteam: 5-Transistor OTA (External Compensation)

## 1. Project Overview
* **Purpose:** To design and implement a highly compact, 1.8 V single-stage operational transconductance amplifier (OTA) for the May 2026 Tiny Tapeout. By utilizing external compensation, this design minimizes silicon area within the Tiny Tapeout tile constraints while providing flexible bandwidth tuning for off-chip loads.
* **Current Status:** Design complete; prepared for May 2026 Tiny Tapeout submission. 
* **Points of Contact:** Santino Pasquale (RFIC Subteam)

## 2. Technical Content & Architecture
Designed for the **Sky130 process**, this amplifier targets a specific 1.5 MHz bandwidth footprint while optimizing for physical space. 

### Circuit Topology
The circuit implements a classic 5-transistor topology:
* **Input Stage:** An NMOS differential pair (M4, M6) provides high transconductance and drives the primary signal.
* **Load Stage:** A PMOS active current mirror (M2, M7) serves as the active load, converting the differential current back to a single-ended output.
* **Tail Current:** An NMOS current source (M1) is biased at 5 µA to establish the operating point for the differential pair.

### Simulation Specifications
Based on schematic-level simulations (1.8 V supply, 5 µA bias, 5 pF load), the amplifier achieves the following performance metrics:
* **DC Gain:** ~36 dB
* **Unity Gain Frequency:** ~1.4 MHz
* **Phase Margin:** ~90°
* **Input Common Mode Range (ICMR):** ~0.9 V to 1.8 V

### Layout & Toolchain
To achieve the minimal footprint, the layout completely omits the on-chip capacitor. 
* **Schematic Capture & Layout:** Cadence Virtuoso
* **Verification:** Pegasus (DRC/LVS)

By keeping the dominant pole external, the amplifier maintains stable operation when driving off-chip loads and gives future testers the flexibility to tune the phase margin post-fabrication.

## 3. Testing & Replication Guide
Future team members or subteam contributors looking to characterize this OTA will need to provide external biasing and compensation to achieve the target performance.

### Required External Hardware
* **Capacitor:** 5 pF - 10 pF (C0G/NP0 ceramic preferred) for frequency compensation and stability.
* **Current Source:** 5 µA precision current source (or a high-value precision resistor/potentiometer) to set the tail current.
* **Bench Equipment:** Oscilloscope, Function Generator, and a DC Power Supply.
* **Interface:** Breadboard or custom carrier PCB to interface the Tiny Tapeout pins with the discrete components.

### Step-by-Step Test Procedure
1.  **Power Delivery:** Apply a clean 1.8 V DC to the **VDPWR** pin and connect **VGND** to the common system ground.
2.  **Biasing:** Inject a 5 µA reference current into the bias pin to activate the tail current source (M1).
3.  **Compensation (Crucial Step):** Connect your 5 pF to 10 pF capacitor directly between the output pin (`ua[0]`) and **VGND**. *Note: Failure to include this external dominant pole will likely result in uncontrolled oscillation.*
4.  **Signal Input:** Apply a differential AC signal from the function generator to the inputs (`ua[1]` and `ua[2]`), ensuring the DC common-mode voltage is maintained within the ICMR of ~0.9 V to 1.8 V.
5.  **Observation:** Monitor the output `ua[3]` on the oscilloscope using a high-impedance active probe to verify open-loop gain (~36 dB), unity gain frequency (~1.4 MHz), and overall phase margin (~90°).

## 4. Future Work & Continuation
For future RFIC members building upon this block:
* **Integration:** This OTA can be utilized as a baseline analog block for more complex RF front-end architectures or filter designs in future Sky130 tapeouts.
* **Optimization:** Further iterations could explore shifting to a folded-cascode topology if higher gain is required, provided the area constraints of the target tapeout allow for the increased transistor count.
