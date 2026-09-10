# Peakforge GPC MWD 1.0.1

**File:** `peakforge-gpc-mwd.1.0.1.html`  
**Date:** 2026-09-09  
**Base:** `peakforge-gpc-mwd.html` (1.0.0 / experimental MWD build)

Bugfix release. No new analysis features.

## Fixed

### Delete no longer touches hidden traces

The Delete tool picked the nearest standard (or peak) by retention time across **all** open traces, including traces that were unchecked in the viewer. A mark on an invisible chromatogram could be removed when you clicked a visible one at a similar RT.

1.0.1 only considers **visible** traces, prefers the trace under the click, and only deletes if the hit is within 0.4 min. Hidden traces keep their standards and peaks.

### CSV export headers

With many calibration marks and sample peaks, Excel sometimes treated the first rows as comments and showed a header **without** `trace`, while the data rows still had that column. Trace names that contain a comma also split a row and shifted every field.

Export now:

- UTF-8 BOM so Excel detects encoding
- RFC 4180 quoting of fields
- Explicit headers that always include `trace` (`section,cal_peak,trace,…` and `section,peak,trace,…`)
- No `#` comment lines
- MWD CSV also includes a `trace` column

## Not changed

Calibration, integration, MWD math, light mode, Agilent `.ch` / `.D` loaders.
