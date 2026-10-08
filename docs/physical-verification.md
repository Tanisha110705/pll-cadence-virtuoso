# Physical Verification (DRC / LVS)

> Evidence: Calibre RVE screenshots in [R1] pp. 6, 14, 20, 26–28, 31 → [`reports/drc/`](../reports/drc)

## 1. DRC

| Item | Value |
|---|---|
| Tool | **Calibre** DRC (results viewed in Calibre RVE v2025.3_34.23; the Calibre menu is present in Virtuoso Layout Suite XL). *Not Assura.* |
| Rule deck | foundry header `_DRC.header_` for SCL 180 nm |
| Runs | 10 (one per cell; list below) |
| Result per run | **1 result, in 1 of 2 checks listed**. The other listed check, `DENSITY_PRINT_FILES`, shows 0. |
| Violations | density/coverage rules only |

| Cell | Flagged rule | Message | Screenshot |
|---|---|---|---|
| inverter (PFD) | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/pfd-inverter-calibre-drc.jpg) |
| `pfd_implement` | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/pfd-calibre-drc.jpg) |
| `vco_inverter_layout` | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/vco-inverter-calibre-drc.jpg) |
| `vco_layout` | `M2.C.1` | M2 coverage less than 0.20 | [link](../reports/drc/vco-calibre-drc.jpg) |
| `charge_pump` | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/charge-pump-calibre-drc.jpg) |
| `nand_2` | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/nand2-calibre-drc.jpg) |
| `nand_3` | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/nand3-calibre-drc.jpg) |
| `d_ff` | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/dff-calibre-drc.jpg) |
| `divided_by_128counter` | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/divide-by-128-calibre-drc.jpg) |
| `pll_arch` | `GC.C.1` | GC coverage less than 0.1 | [link](../reports/drc/pll-top-calibre-drc.jpg) |

The rule-file text shown with each violation reads: *"Violation of this rule will result in an automatic rejection of your gds."*

### Interpretation

- **No spacing, width, enclosure or extension violations are shown in any run.** The report describes this as *"no critical issues during the Design Rule Check"* ([R1] p. 7).
- **Every cell still has one open density/coverage violation.** These rules require a minimum fraction of each layer's area within the check window. Small, sparse cells fall short of that fraction, and fill normally resolves it.
- The violation was **deliberately left unresolved**: *"since it may create unknown parasitics, which may affect the speed of the circuit"* ([R1] p. 7).
- **Accurate status: "geometric rules pass; one foundry density rule open per cell".** This is *not* "DRC-clean", and the layout is not tape-out-ready as is. The rule text itself says the GDS would be rejected. Resolving it would mean adding dummy or fill shapes (and then re-checking performance with extraction) or obtaining a waiver.

## 2. LVS

**Not performed.** Neither report contains LVS evidence: no Calibre LVS or Assura LVS run, and no comparison report. Layout-to-schematic correspondence is therefore **not verified**.

## 3. Parasitic extraction / post-layout simulation

**Not performed.** There are no PEX/RCX results, no `av_extracted` or `calibre` views, and no post-layout waveforms. Every frequency and timing number in this repository is from **schematic-level** simulation.

## 4. Recommended next steps

1. Run **Calibre LVS** on each block and on `pll_arch`, and fix opens, shorts and device-parameter mismatches.
2. Add fill (or use the foundry fill utility) to clear `GC.C.1` and `M2.C.1`, and rerun DRC.
3. Run **Calibre PEX** (or Quantus) and re-simulate:
   - the VCO f–V curve;
   - the divider's maximum toggle frequency;
   - the charge-pump matching;
   - the closed-loop transient.

   Then compare each against the schematic results.
