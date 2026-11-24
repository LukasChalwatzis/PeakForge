# PeakForge Visualizer

A self-contained browser tool for plotting molecular weight distribution (MWD) data from GPC/SEC experiments.

Open `PeakForge_Visualizer.html` in any modern browser. No installation or server required.

## Main Features

- **Sample list** — Select traces with checkboxes. Drag rows to reorder (legend follows the order).
- **Editable legend names** — Click any sample name in the list to rename it in the legend.
- **Colors & line styles** — Per-sample color picker and line style (solid, dash, dot, dashdot). Official KA strain colors are pre-assigned when sample names match.
- **Plot controls**
  - Gridlines on/off
  - X-axis intermediate labels (numbers between the major log markers) on/off
  - Legend on/off
  - Area-normalize toggle
  - Adjustable left and bottom margins
  - Font size controls for axis titles, tick labels, and legend (line thickness scales with tick size)
  - Legend position (X/Y) and anchor
- **A4 / Word export**
  - Set figure width in cm and DPI.
  - Choose aspect ratio (3:2, 4:3, 16:9, 1:1).
  - “Preview at export size” resizes the on-screen plot to match the relative proportions of the PNG.
  - Download PNG produces a file that, when inserted into Word at the same centimetre width, shows the selected font sizes at the corresponding point size.
- **CSV import** — See below.

## Importing CSV Data

1. Click **Upload CSV** and select a file.
2. A modal appears asking you to map columns.
3. Choose:
   - **Sample / trace name column** (optional — if set to “none”, the filename is used as the sample name).
   - **MW (g/mol) column** (required).
   - **dw/dlogM column** (required).
4. Click **Import**.

The tool auto-guesses columns based on header names, but you can override the mapping.

### Expected CSV Structure

- Plain CSV (comma-separated).
- First row must be a header row.
- One data point per row.
- Multiple samples can be in the same file if a sample-name column is present.

**Required columns** (header names are matched case-insensitively):

| Purpose       | Typical header names                  |
|---------------|---------------------------------------|
| Molecular weight | `MW`, `M_g_per_mol`, `molecular`, `mol` |
| Distribution | `dW_dlogM`, `dw`, `dlog`, `height`, `intensity` |

**Optional column**:

| Purpose       | Typical header names                  |
|---------------|---------------------------------------|
| Sample name   | `sample`, `strain`, `trace`, `name`, `peak` |

### Example CSV

```csv
sample,MW,dW_dlogM
KA27_Inc_7d,500,0.012
KA27_Inc_7d,1000,0.045
KA27_Inc_7d,5000,0.210
KA49_SN_0h,450,0.008
KA49_SN_0h,2000,0.150
```

### Notes on Import

- Extra columns are ignored.
- Files with more than ~400 points per sample are automatically downsampled to ~300 points for performance.
- Imported samples appear in the list and can be styled, reordered, and plotted immediately.
- You can upload multiple CSV files; new samples are added to the existing list.

## Export

- **Download PNG** uses the current figure width (cm), DPI, and aspect ratio.
- When you place the PNG in Microsoft Word and set its width to the same centimetre value you chose in PeakForge, the axis titles, tick labels, and legend appear at the font sizes you selected.
- Use **Preview at export size** to check the relative layout before downloading.

## Tips

- Official KA colors (KA21, KA25, KA27, KA48, KA49) are applied automatically when sample names contain those identifiers.
- The “Apply official colors” preset re-applies them at any time.
- Area-normalize divides each trace by its integral so distributions can be compared on the same scale.
- All settings (colors, styles, names, legend position, margins, font sizes) are kept in the browser session until you reload the page.
