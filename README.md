# Peakforge GPC/SEC (MWD build)

Single-file browser workbench for **GPC/SEC** chromatograms: open Agilent data, mark standards, fit log₁₀ M(t), integrate peaks, and plot **dW/dlog₁₀M** on a log₁₀ molecular-weight axis.

No installer, no license server, no Windows registry. Open the HTML file in Firefox or Chrome.

**This file:** `peakforge-gpc-mwd.html`  
Experimental fork of the GPC peak picker. It is the feature-complete GPC build (calibration + MWD + light theme). It is **not** the HPLC sibling.

## What it does

- Read **Agilent ChemStation / OpenLab** `*.ch` (LC versions 30 and 130: RID, DAD, …)
- Ingest a **`.D` folder**, a parent directory of `.D` folders, a **`.zip`** of the same, or exported text (`txt` / `csv` / `tsv`)
- Catalog first, then open individual channels (folders stay collapsed)
- Manual **baseline integration** (click-drag; live fill)
- **Calibration:** click standard peaks → enter Mp → polynomial fit of log₁₀ M vs retention time (default degree 4)
- Slice **Mn, Mw, Mz, Đ** on each integrated sample window (RI/signal height as mass weight)
- **MWD plot:** M (g/mol), log₁₀ axis, vs dW/dlog₁₀M, only inside the integrated window(s)
- Overlay **several** integrated peaks; export MWD as CSV
- Hide the calibration pane or the MWD pane
- **Light mode** for slides and screenshots (darker trace colours, lighter buttons)

It does **not** talk to the LC, run sequences, or provide 21 CFR Part 11 audit trails. Acquisition still belongs on the instrument PC.

## Quick start

1. Download `peakforge-gpc-mwd.html`.
2. Open it in a current Chromium or Firefox browser (needs `File` + drag-and-drop).
3. First load needs the network once if Plotly / JSZip are pulled from the CDN. After that, keep the tab; data never leaves the machine.
4. **Add folder** / drop a `.D` or a zip → expand the group → **Open** the RID (or other) channel.
5. Switch to **Mark standard** on a standards run, click each peak apex, enter Mp (g/mol), **Fit**.
6. Open the sample, **Integrate**, drag left shoulder → right shoulder.
7. The MWD pane fills for that peak. Tick several peaks to overlay. **Export MWD CSV** if you need the numbers in Origin/Python.

Remove a channel or a whole folder from the catalog with **Remove** / **Remove folder**. **Clear catalog** drops everything.

## Calibration and moments

Standards: narrow-Đ Mp vs retention time.

```
log₁₀ M(t) = a₀ + a₁ t + a₂ t² + a₃ t³ + a₄ t⁴
```

Degree is selectable; you need at least degree+1 points. R² is shown under the cal plot.

On an integrated window, slice mass weight is |hᵢ| (baseline-corrected signal). Then:

- Mn = Σh / Σ(h/M)
- Mw = Σ(h·M) / Σh
- Mz = Σ(h·M²) / Σ(h·M)
- Đ = Mw / Mn

Signal height is treated as **proportional to mass concentration**.

## MWD

For each slice in the **integrated** range only:

- wᵢ = |hᵢ| Δt  
- dlog₁₀Mᵢ = |d(log₁₀M)/dt| · Δt  
- dW/dlog₁₀M = (wᵢ / Σw) / dlog₁₀Mᵢ  

The curve is normalized so ∫ dW = 1 over that window. X is M (g/mol) with Plotly `type: log` (log₁₀, not ln).

CSV columns: `peak, t_min, M_g_per_mol, log10M, height_minus_baseline, mass_weight, dlog10M, dW_dlogM`.

## UI notes

| Control | Role |
|---|---|
| Integrate | Drag a baseline under the sample peak |
| Mark standard | Click apex, type Mp |
| Pan / zoom | Box-zoom; scroll zoom |
| Y scale | Full / clip outliers / baseline region / manual min–max |
| Hide cal plot / Hide MWD plot | Give the chromatogram the vertical space |
| Light mode | White paper, darker lines, pale buttons |
| Export CSV | Peak table + calibration points |
| Export MWD CSV | Distribution points for ticked peaks |

Peaks and standards drawn on a chromatogram are only those that belong to the **visible** traces.

## Files it understands

| Input | Behaviour |
|---|---|
| `*.ch` | Parsed in-page (delta-encoded ChemStation LC) |
| Folder / `webkitdirectory` | Finds `*.ch` under `.D` trees |
| `*.zip` | Same, via JSZip |
| `*.txt`, `*.csv`, `*.tsv`, `*.dat` | Two numeric columns: time (min), signal |

Unsupported `.ch` versions throw a short error. If a channel looks empty, check that you opened the RID/DAD file and not a method stub.

## Accuracy

Compare Mn, Mw, Đ and one MWD overlay against ChemStation (or another trusted package) on a standard mix **before** you publish numbers. Different baseline choices move moments more than the polynomial degree does.

## Limitations

- Offline CDN: if Plotly/JSZip must work with no internet, download those two scripts and point the `<script src>` tags at local copies.
- One HTML file on purpose. Forks (HPLC-only, etc.) are separate files, not plugins.
- No auto-peak picking. Integration is manual so you see the baseline.

## License / use

Written as an institute analysis helper. No warranty. Do not treat it as validated software for regulated release testing unless you validate it yourself.

Instrument control, sequences, and compliance remain Agilent’s (or whoever sold you the LC).
