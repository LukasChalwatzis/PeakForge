# Peakforge GPC MWD 1.0.2

**File:** `peakforge-gpc-mwd.1.0.2.html`  
**Date:** 2026-11-24  
**Base:** `peakforge-gpc-mwd.1.0.1.html`

Bugfix release. No new analysis features.

## Fixed

### MWD CSV quoting

The results export in 1.0.1 quoted fields (so commas in trace names stayed in one column). The **MWD export** still joined values with a raw comma.

1.0.2 uses the same `csvRow` / `csvCell` helpers for the MWD file. Trace names containing commas are quoted. UTF-8 BOM and `charset=utf-8` are set as on the results export.

## Not changed

Delete behaviour, calibration, integration, MWD math, light mode, Agilent loaders.
