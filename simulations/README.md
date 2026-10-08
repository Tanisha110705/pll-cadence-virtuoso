# Simulations

Waveform screenshots from Virtuoso Visualization & Analysis XL. The simulations ran in Spectre through ADE L. Each screenshot is cropped to the plot area. Files named `*-hand-drawn*` or `*-hand-written*` are notebook pages (expected waveforms, procedures and calculations), **not** simulator output.

| Block | File | Signals / content | Source |
|---|---|---|---|
| PFD | [pfd/pfd-transient-up-down.jpg](pfd/pfd-transient-up-down.jpg) | `/DOWN`, `/UP`, `/CLKREF`, `/CLKVCO`, 0–1 µs | [R1] p. 4 |
| PFD | [pfd/pfd-inverter-transient.jpg](pfd/pfd-inverter-transient.jpg) | inverter Vin / Vout | [R1] p. 3 |
| PFD | [pfd/pfd-expected-timing-hand-drawn.jpg](pfd/pfd-expected-timing-hand-drawn.jpg) | expected timing (notebook) | [R1] p. 5 |
| VCO | [vco/vco-frequency-vs-vcontrol.jpg](vco/vco-frequency-vs-vcontrol.jpg) | `frequency(Vout)` vs. swept V; marker 107.12 MHz @ 514 mV | [R1] p. 10 |
| VCO | [vco/vco-parametric-sweep.jpg](vco/vco-parametric-sweep.jpg) · [vco/vco-parametric-sweep-zoom.jpg](vco/vco-parametric-sweep-zoom.jpg) | overlaid `Vout` for swept `Vcontrol` | [R1] p. 11 |
| VCO | [vco/vco-transient-oscillation.jpg](vco/vco-transient-oscillation.jpg) · [vco/vco-transient-vcontrol-pulse.jpg](vco/vco-transient-vcontrol-pulse.jpg) | `Vcontrol`, `Vout` | [R1] pp. 11–12 |
| VCO | [vco/vco-inverter-transient.jpg](vco/vco-inverter-transient.jpg) | `vco_inverter` Vin / Vout | [R1] p. 9 |
| VCO | [vco/vco-kvco-calculation-1.jpg](vco/vco-kvco-calculation-1.jpg) · [vco/vco-kvco-calculation-2.jpg](vco/vco-kvco-calculation-2.jpg) | handwritten K_VCO and tuning-range calculation | [R1] pp. 12–13 |
| VCO | [vco/vco-procedure-hand-written.jpg](vco/vco-procedure-hand-written.jpg) | source settings and parametric set-up | [R1] p. 10 |
| Charge pump | [charge-pump/charge-pump-transient.jpg](charge-pump/charge-pump-transient.jpg) | `/Down`, `/Up`, `/Out1`, `/Out2` | [R2] p. 5 |
| Charge pump | [charge-pump/charge-pump-expected-hand-drawn.jpg](charge-pump/charge-pump-expected-hand-drawn.jpg) | expected waveform (notebook) | [R2] p. 3 |
| Divider | [divider/divider-stage-outputs.jpg](divider/divider-stage-outputs.jpg) | `/clk`, `/net1`–`/net11`, `/Q`, 0–5 µs | [R2] p. 11 |
| Divider | [divider/divide-by-128-transient.jpg](divider/divide-by-128-transient.jpg) | `/Q`, `/clk`, `/~Q`, 0–37 µs | [R2] p. 11 |
| Divider | [divider/dff-toggle-transient.jpg](divider/dff-toggle-transient.jpg) | single DFF in toggle mode | [R2] p. 8 |
| Divider | [divider/nand3-transient.jpg](divider/nand3-transient.jpg) | NAND3 A, B, C, Vout | [R2] p. 10 |
| PLL | [pll/pll-control-voltage-vcp.jpg](pll/pll-control-voltage-vcp.jpg) | `/V_CP` acquisition and settling | [R1] p. 30 |
| PLL | [pll/pll-transient-overview.jpg](pll/pll-transient-overview.jpg) | `/Vout`, clock, `/V_CP`, DN, UP | [R1] p. 30 |
| PLL | [pll/pll-result-hand-written.jpg](pll/pll-result-hand-written.jpg) | handwritten result "output frequency = 2.56 GHz" with sketch | [R1] p. 30 |
