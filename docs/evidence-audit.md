# Evidence Audit and Provenance

This page records where every claim in the repository comes from, which inconsistencies were found in the sources, and what must not be claimed.

## Sources

| Key | Document | Date | Pages |
|---|---|---|---|
| **[R1]** | *CMOS Analog IC Design — Theory DA-2: Layout of PLL* (Winter Semester 2025-26, VIT), project report | 16 Apr 2026 | 31 |
| **[R2]** | *CMOS Analog IC Design — Lab DA-5: Implementation of charge pump and divided-by-N counter architecture* | 4 Apr 2026 | 12 |

[R2] is a subset of [R1] covering the charge pump and divider. It contains higher-resolution copies of the same screenshots, which are used here where available.

The original PDFs are **not committed**. Their screenshots include the institution's EDA server hostname and per-user home-directory paths. Every image in this repository was cropped from the PDFs to the schematic, layout or waveform canvas, which removes title bars, status bars and path strings. No pixels inside a canvas were altered.

## Page map for [R1]

| Section | Pages |
|---|---|
| PFD (aim, circuit, schematics, transient, expected waveform, layouts, DRC, inferences) | 2–7 |
| VCO (aim, circuit, schematics, procedure, f–V, sweeps, K_VCO calculation, layouts, DRC, inferences) | 8–15 |
| Charge pump (aim, circuit, test circuit, procedure, schematics, transient, results, layout, DRC) | 16–20 |
| Divide-by-128 (aim, circuits, procedure, NAND/DFF schematics, transients, results, layouts, DRC) | 21–28 |
| PLL (aim/specification, circuit, schematic, transients, result, layout, DRC, conclusions) | 29–31 |

## Confirmed by evidence

| Claim | Evidence |
|---|---|
| Technology is SCL 180 nm | aims ([R1] pp. 2, 8); `ts018_scl_prim` in every layout palette; [R1] p. 7 |
| Virtuoso Schematic Editor L, ADE L, Visualization & Analysis XL, Layout Suite XL | window titles in the screenshots |
| Spectre simulator | ADE L status bar "Simulator: spectre" |
| Calibre DRC (not Assura) | Calibre RVE windows, for example "Calibre – RVE v2025.3_34.23" |
| VCO numbers (f_min, f_max, K_VCO, tuning range) | f–V screenshot marker and handwritten calculation, [R1] pp. 10–13 |
| 7-stage ÷128 divider, 50 % duty | [R1] p. 25; schematic with 7 `d_ff` instances |
| PLL specification (F_ref, R, C1, C2, I_CP) | handwritten aim, [R1] p. 29 |
| Closed-loop V_CP acquisition and settling | V_CP transient screenshot, [R1] p. 30 |
| One density rule per DRC run | 10 Calibre RVE screenshots |

## Inconsistencies found in the sources

| # | Inconsistency | How it is handled here |
|---|---|---|
| 1 | The charge-pump procedure and results text says **"gpdk180"** ([R1] pp. 17, 19; [R2] pp. 3, 5). Everything else, including the PFD and VCO aims and every layout palette, says **SCL 180 nm** (`ts018_scl_prim`). | The technology is stated as SCL 180 nm. The gpdk180 wording is treated as procedure-template text. |
| 2 | [R1] p. 7 says the PFD consists of "D-Flip Flops and resets", but the drawn circuit is a transistor-level dynamic PFD with no explicit DFF cells. | Described as a dynamic PFD that is *functionally* equivalent to two state elements plus reset. |
| 3 | The report says there were "no critical issues" in DRC, yet every Calibre run shows one open density violation whose rule text says the GDS would be rejected. | Reported as "geometric rules pass; one density rule open per cell", never as "DRC clean". |
| 4 | The PLL target is 2.5 GHz, while N × F_ref = 128 × 20 MHz = 2.56 GHz, which is also the recorded result. | Both numbers are shown and the 2.4 % difference is explained. |
| 5 | The charge pump is described as "linear", while the waveform is a staircase. | Explained: each pulse adds an equal step, so the envelope is linear. |
| 6 | In the 5 µs divider plot the final Q makes only one transition. | The 50 % duty-cycle claim is attributed to the report and to the 37 µs run. |
| 7 | The VCO K_VCO is an end-point average over a non-linear curve. | Labelled "average"; small-signal K_VCO is listed as not measured. |

## Results that must NOT be claimed

| Item | Why |
|---|---|
| LVS clean / LVS performed | no LVS evidence |
| Post-layout (extracted) performance | no PEX or post-layout simulation |
| "DRC clean" | one density violation per cell |
| Lock time / settling time | only a visual waveform; no measurement |
| Measured PLL output frequency from simulation | the 2.56 GHz value is handwritten; no `frequency()` measurement is shown |
| Phase noise, jitter, reference spurs | not simulated |
| Loop bandwidth, phase margin, damping | not computed (small-signal K_VCO and I_CP at the operating point unknown) |
| Small-signal K_VCO at lock | only the average K_VCO exists |
| Power, area | not reported |
| PFD dead zone or detection range | not measured; 2π is only the design aim |
| Charge-pump I_up/I_dn values or mismatch % | not measured |
| Divider maximum frequency, ripple delay | not measured |
| PVT, Monte Carlo, silicon results | none |

## Approximate visual readings used in the docs

These were read by eye from screenshots of about 700–800 px width and are marked as approximate wherever they appear:

- **V_CP:** starts at about 0.60 V, peaks at about 0.96 V, settles at about 0.9 V.
- **Charge-pump ramps:** cross at about 0.9 V.
- **VCO f–V curve:** about 2.5–2.6 GHz at 0.9 V.
