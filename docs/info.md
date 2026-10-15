# C2S2 RFIC Subteam: 5-Transistor OTA (External Compensation)

## 1. Project Overview
* **Purpose:** To design and implement a compact, 1.8 V single-stage operational transconductance amplifier (OTA) for the May 2026 Tiny Tapeout. By utilizing external compensation, this design minimizes silicon area within the Tiny Tapeout tile constraints while allowing the bandwidth to be tuned for off-chip loads.
* **Current Status:** Submitted to the May 2026 Tiny Tapeout shuttle; in fabrication.
* **Point of Contact:** Santino Pasquale (C2S2 RFIC Subteam)

## 2. Technical Content & Architecture
Designed for the **SKY130 process**, this amplifier targets a unity-gain frequency of roughly 1.4 MHz while minimizing silicon area.

### Circuit Topology
The circuit implements a classic 5-transistor topology:
* **Input Stage:** An NMOS differential pair (M4, M6) provides the transconductance and receives the input signal.
* **Load Stage:** A PMOS active current mirror (M2, M7) serves as the active load, converting the differential current to a single-ended output.
* **Tail Current:** An NMOS current source (M1), mirrored from the external reference on `ua[0]`, is biased at 5 µA to set the operating point of the differential pair.

### Pinout
| Pin | Signal | Description |
|---|---|---|
| `ua[0]` | Ibias | 5 µA reference current input (diode-connected bias device) |
| `ua[1]` | Vin1 | Differential input |
| `ua[2]` | Vin2 | Differential input |
| `ua[3]` | Vout | Single-ended output |
| VDPWR / VGND | — | 1.8 V supply / ground |

### Simulation Specifications
Based on schematic-level simulations (1.8 V supply, 5 µA bias, 5 pF load), the amplifier achieves the following:
* **DC Gain:** ~36 dB
* **Unity-Gain Frequency:** ~1.4 MHz
* **Phase Margin:** ~90°
* **Input Common-Mode Range (ICMR):** ~0.9 V to 1.8 V

### Layout & Toolchain
To achieve the minimal footprint, the layout omits the on-chip compensation capacitor.
* **Schematic Capture & Layout:** Cadence Virtuoso
* **Verification:** Pegasus (DRC/LVS)
* **Footprint:** 1x2 Tiny Tapeout tile

Because the dominant pole is set by the external capacitor at the output, the amplifier remains stable when driving off-chip loads, and testers can adjust the bandwidth and phase margin after fabrication by changing the capacitor value.

## 3. Testing & Replication Guide
To characterize this OTA, testers must provide external biasing and compensation.

### Required External Hardware
* **Capacitor:** 5 pF – 10 pF (C0G/NP0 ceramic preferred) for frequency compensation and stability.
* **Current Source:** 5 µA precision current source (or a high-value precision resistor/potentiometer) to set the tail current.
* **Bench Equipment:** Oscilloscope, function generator, and DC power supply.
* **Interface:** Breadboard or custom carrier PCB connecting the Tiny Tapeout pins to the discrete components.

### Step-by-Step Test Procedure
1. **Power Delivery:** Apply a clean 1.8 V DC to **VDPWR** and connect **VGND** to the common system ground.
2. **Biasing:** Inject a 5 µA reference current into **`ua[0]`** (Ibias) to activate the tail current source (M1).
3. **Compensation (Crucial Step):** Connect the 5 pF – 10 pF capacitor between the output **`ua[3]`** and **VGND**. *Without this external dominant pole, the amplifier will likely oscillate.*
4. **Signal Input:** Apply a differential AC signal from the function generator to **`ua[1]`** and **`ua[2]`**, keeping the DC common-mode voltage within the ICMR (~0.9 V to 1.8 V).
5. **Observation:** Monitor **`ua[3]`** on the oscilloscope with a high-impedance active probe.
   * **Gain:** Sweep the input frequency at small amplitude and compare output to input amplitude to measure the low-frequency gain (~36 dB) and the frequency where gain falls to 1 (~1.4 MHz).
   * **Stability:** Configure the OTA as a unity-gain buffer (output tied to the inverting input) and apply a small step. A clean step response with little or no overshoot is consistent with the high simulated phase margin; ringing indicates reduced margin.

## 4. Future Work & Continuation
For future RFIC members building upon this block:
* **Integration:** This OTA can serve as a baseline analog block for more complex RF front-end architectures or filter designs in future SKY130 tapeouts.
* **Optimization:** Further iterations could explore a folded-cascode topology if higher gain is required, provided the area constraints of the target tapeout allow for the increased transistor count.
