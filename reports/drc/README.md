# Calibre DRC results

Calibre RVE result windows (v2025.3_34.23). Each run lists **1 result in 1 of 2 checks**. The flagged check is a foundry density/coverage rule; `DENSITY_PRINT_FILES` shows 0.

| Cell | Flagged rule | File |
|---|---|---|
| inverter (PFD) | `GC.C.1` (GC coverage < 0.1) | [pfd-inverter-calibre-drc.jpg](pfd-inverter-calibre-drc.jpg) |
| `pfd_implement` | `GC.C.1` | [pfd-calibre-drc.jpg](pfd-calibre-drc.jpg) |
| `vco_inverter_layout` | `GC.C.1` | [vco-inverter-calibre-drc.jpg](vco-inverter-calibre-drc.jpg) |
| `vco_layout` | `M2.C.1` (M2 coverage < 0.20) | [vco-calibre-drc.jpg](vco-calibre-drc.jpg) |
| `charge_pump` | `GC.C.1` | [charge-pump-calibre-drc.jpg](charge-pump-calibre-drc.jpg) |
| `nand_2` | `GC.C.1` | [nand2-calibre-drc.jpg](nand2-calibre-drc.jpg) |
| `nand_3` | `GC.C.1` | [nand3-calibre-drc.jpg](nand3-calibre-drc.jpg) |
| `d_ff` | `GC.C.1` | [dff-calibre-drc.jpg](dff-calibre-drc.jpg) |
| `divided_by_128counter` | `GC.C.1` | [divide-by-128-calibre-drc.jpg](divide-by-128-calibre-drc.jpg) |
| `pll_arch` | `GC.C.1` | [pll-top-calibre-drc.jpg](pll-top-calibre-drc.jpg) |

**Status:** no geometric (width, spacing, enclosure) violations are shown in any run. One density rule per cell was intentionally left open, so the layouts are **not DRC-clean for tape-out**. See [../../docs/physical-verification.md](../../docs/physical-verification.md).
