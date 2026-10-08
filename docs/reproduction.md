# Reproduction Guide

> **Status: documentation only.** This repository contains no Cadence library database (OpenAccess cells), netlists, ADE states or Calibre rule decks. Those require the licensed Cadence and Calibre tools and the SCL 180 nm PDK, which cannot be redistributed. The steps below rebuild the project from the documented circuits. Nothing here was re-run as part of preparing this repository.

## Tools used (as seen in the screenshots)

| Purpose | Tool |
|---|---|
| Schematic entry | Virtuoso Schematic Editor L |
| Simulation set-up | Virtuoso Analog Design Environment L (ADE L) |
| Simulator | **Spectre** (ADE status bar: "Simulator: spectre") |
| Waveform viewing / calculator | Virtuoso Visualization & Analysis XL |
| Layout | Virtuoso Layout Suite XL |
| DRC | Calibre (RVE result browser) |
| Technology | SCL 180 nm PDK, primitive library `ts018_scl_prim` |

## Build order

The order follows the dates in the project ([design-flow.svg](../assets/design-flow.svg)).

### 1. Inverter and PFD
1. Create a library attached to the SCL 180 nm technology, then create an `inverter` cell and its symbol.
2. Verify the inverter with a transient run: `vpulse` input, observe Vout.
3. Build `pfd_implement` from the hand-drawn circuit ([pfd-circuit-hand-drawn.jpg](../schematics/pfd/pfd-circuit-hand-drawn.jpg)): precharge PMOS pairs, input NMOS devices, INV1–INV4 and the N1–N4 reset network.
4. Testbench: two `vpulse` clocks (`CLKREF`, `CLKVCO`) at slightly different frequencies. Run a transient (the report shows 0–1 µs) and plot `UP`, `DOWN`, `CLKREF` and `CLKVCO`.

### 2. VCO
1. Build a `vco_inverter` cell (PMOS W = 100 µm, NMOS W = 50 µm; L is not documented, so choose the minimum or the PDK default).
2. Build `kvco_implement`: three `vco_inverter` instances in a ring, with 4 PMOS and 4 NMOS current-starving and bias devices. `Vcontrol` drives the input NMOS ([vco-schematic.jpg](../schematics/vco/vco-schematic.jpg)).
3. Transient: `Vcontrol` from `vpulse`, with V1 = 0, V2 = 1.8 V, period = 10 ns, width = 10 ns, rise/fall = 10 ps, delay = 0. Stop time 1 µs.
4. Parametric: replace V2 with a design variable `V` and sweep 0 → 1.8 V (the report used "auto, 10 steps").
5. In the ADE calculator, plot `frequency(getData("/Vout" ?result "tran"))` against `V`. Read f_min and f_max, then compute:
   - K_VCO = 2π(f_max − f_min)/(V_max − V_min)
   - f_c = (f_max + f_min)/2
   - tuning range = (f_max − f_min)/f_c

### 3. Divide-by-128
1. Build `nand_2` and `nand_3` at transistor level, plus symbols, and verify each with a transient run.
2. Build `d_ff` from six NAND gates (positive-edge-triggered). Tie D to Q̅ and verify that it toggles at half the clock rate.
3. Build `divided_by_128counter`: seven `d_ff` instances, each output clocking the next.
4. Transient with a `vpulse` clock, and plot every stage output and the final Q (the report used 5 µs and about 37 µs windows).

### 4. Charge pump
1. Build `charge_pump`: a reference current source, PMOS and NMOS current mirrors, a PMOS source branch switched by `UP` (through an inverter) and an NMOS sink branch switched by `DOWN`.
2. Testbench: two instances fed the same pulse train, one on `Up` and the other on `Down`, each with an output capacitor. Compare the `Out1` and `Out2` ramps, and tune the mirror W/L until the slopes match.

### 5. PLL
1. Build `pll_arch`: PFD → charge pump → loop filter (R = 1 kΩ in series with C1 = 227 pF, plus C2 = 35 pF to ground) → VCO → divider → back to the PFD.
2. Reference: 20 MHz `vpulse`. Set an initial condition of 600 mV on the filter node.
3. Run a long transient (the report shows about 7.6 µs and about 16.5 µs windows) and plot `V_CP`, `Vout`, `UP` and `DN`.
4. Recommended additional measurements, which are *not* in the report:
   - settling time;
   - `frequency(Vout)` over the final microseconds;
   - edge-to-edge phase error between `CLKREF` and `FB`.

### 6. Layout and DRC
1. Lay out the leaf cells, then the blocks, then `pll_arch` in Layout Suite XL. Fold large devices into fingers and keep clock and delay paths symmetric.
2. Run Calibre DRC on every cell. Expect density/coverage rules (`GC.C.1`, `M2.C.1`) on small cells unless fill is added.
3. Not done in the project, but next: Calibre LVS, parasitic extraction, and post-layout re-simulation.
