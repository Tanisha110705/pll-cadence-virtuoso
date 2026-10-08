# Results

Every number below is traced to a source. The **Evidence** column uses:

- **[R1] p. N** — the project report (see [evidence-audit.md](evidence-audit.md))
- **[R2] p. N** — the charge-pump/divider lab report
- **screenshot** — read from the named image in this repository
- **calc** — arithmetic on values from the other columns

Values marked *design* are specifications, not measurements. Values marked *visual* were read by eye from low-resolution screenshots.

## Summary table

| Block | Metric | Result | Evidence |
|---|---|---:|---|
| Technology | process | SCL 180 nm CMOS (`ts018_scl_prim`) | [R1] pp. 2, 7, 8; layout palette in screenshots |
| PFD | architecture | transistor-level dynamic CMOS, shared NMOS reset | [R1] pp. 2–4 |
| PFD | functional transient | UP wide pulses / DOWN narrow reset pulses (CLKREF leading) | [R1] p. 4, [screenshot](../simulations/pfd/pfd-transient-up-down.jpg) |
| PFD | detection range | 2π (*design aim only*) | [R1] p. 2 |
| VCO | topology | 3-stage current-starved ring, 16 schematic instances | [R1] pp. 8–9 |
| VCO | inverter device widths | PMOS 100 µm, NMOS 50 µm | [R1] pp. 8, 15 |
| VCO | f_min | 107.12 MHz @ 0.514 V | [R1] pp. 10, 12 |
| VCO | f_max | 4.2067 GHz @ 1.8 V | [R1] p. 12 |
| VCO | K_VCO (average, end-point) | 20.03 × 10⁹ rad/s/V (≈ 3.19 GHz/V) | [R1] p. 12; Hz/V by calc |
| VCO | centre frequency | 2.15691 GHz | [R1] p. 13 |
| VCO | tuning range | 190.067 % | [R1] p. 13 |
| Charge pump | instances | 12 MOSFETs + 1 ideal current source | [R2] p. 4 |
| Charge pump | I_CP | 100–150 µA (*design*) | [R1] p. 29 |
| Charge pump | behaviour | linear charge (UP), linear discharge (DOWN), hold otherwise; symmetric after W/L tuning | [R1] p. 19, [R2] p. 5 |
| Charge pump | ramp crossing | ≈ 0.9 V, near mid-supply (*visual*) | [screenshot](../simulations/charge-pump/charge-pump-transient.jpg) |
| Loop filter | topology | R + C1 in series, ‖ C2 (2nd-order passive) | [R1] p. 29 |
| Loop filter | values | R = 1 kΩ, C1 = 227 pF, C2 = 35 pF (*design*) | [R1] p. 29 |
| Loop filter | zero / pole | 701 kHz / 5.25 MHz | calc |
| Divider | stages | 7 toggle DFFs (6 NAND gates each) | [R1] p. 25, [R2] pp. 10–11 |
| Divider | division ratio | **128** (= 2⁷) | [R1] p. 25 |
| Divider | output duty cycle | 50 % | [R1] p. 25 (reported); consistent with [screenshot](../simulations/divider/divide-by-128-transient.jpg) |
| PLL | F_ref | 20 MHz (*design*) | [R1] p. 29 |
| PLL | target output | 2.5 GHz (*design*) | [R1] p. 29 |
| PLL | N × F_ref | 2.56 GHz | calc |
| PLL | recorded output frequency | 2.56 GHz (handwritten result) | [R1] p. 30 |
| PLL | V_CP start / peak / settled | ≈ 0.60 V / ≈ 0.96 V / ≈ 0.9 V (*visual*) | [screenshot](../simulations/pll/pll-control-voltage-vcp.jpg) |
| PLL | lock time | **not measured** | — |
| Layout / DRC | cells laid out | 10 (4 leaf, 5 block, 1 top) | [R1] |
| Layout / DRC | Calibre result | 1 density/coverage result per cell (`GC.C.1` ×9, `M2.C.1` ×1); no geometric violations shown | [R1]; [reports/drc](../reports/drc) |
| Layout / DRC | LVS | **not performed** | — |

## PFD results

- The UP/DOWN behaviour matches the expected three-state PFD operation ([pfd.md](pfd.md)).
- Layout DRC: one `GC.C.1` density result, intentionally left open.
- No quantitative PFD metric (dead zone, reset-pulse width, maximum frequency) was measured.

## VCO results

- A sustained oscillation is shown in transient.
- The parametric sweep shows frequency rising with `Vcontrol`.
- The f–V curve spans 107.12 MHz to 4.2067 GHz.
- Reported average K_VCO is 20.03 Grad/s/V and tuning range 190.067 %.
- The curve is strongly non-linear, so the small-signal K_VCO at the operating point differs from the average ([vco.md](vco.md)).

## Charge-pump results

- Out1 and Out2 staircase up and down symmetrically and cross near 0.9 V.
- The report notes mismatch sensitivity, switching spikes (charge sharing) and minor leakage.
- No current values were measured.

## Divider results

- ÷2 per stage, confirmed visually across all seven stage outputs.
- ÷128 overall and 50 % duty cycle, as reported.
- Ripple (clock-to-Q accumulation) is discussed qualitatively and not measured.

## PLL results

- Closed-loop transient shows V_CP acquiring from 0.6 V, overshooting, and settling at about 0.9 V. The recorded output frequency is 2.56 GHz.
- No quantified lock time, phase error, jitter, phase noise or loop bandwidth.

## Layout / DRC results

- Full custom layout of every block and of the top level.
- Calibre DRC: geometric rules pass in every run shown, with one open density/coverage rule per cell ([physical-verification.md](physical-verification.md)).
- No LVS, no extraction.

## Not measured (do not quote)

The following were **not measured**, and no value is claimed for any of them:

- power;
- area;
- jitter and phase noise;
- lock or settling time;
- loop bandwidth, phase margin and damping;
- small-signal K_VCO at lock;
- I_up/I_dn mismatch numbers;
- dead zone;
- maximum divider frequency;
- post-layout frequency;
- PVT or Monte Carlo behaviour.
