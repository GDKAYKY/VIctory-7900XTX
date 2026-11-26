# This is a documentation of each Project Victory run

### How To Use:

### 1. Each time a change is made and tested ingame a file with the following name containing the logs neeeds to be created `RUN{$NUMBER}-{$TIMEDATE}-{$LOGLEVEL}`

### 2. Create a run summary in this file under the following section folllowing the First Example Standard.

# RUNTIME SUMMARY

## EXECUTION `{$NUMBER}`-`{$DATETIME}`

### - Log Level `{$LOGLEVEL}`

### LOGPATH:`RUN{$NUMBER}-{$TIMEDATE}-{$LOGLEVEL}`

---

### Average Frametime Before vs After:

#### Previous Run: `{$PREVIOUS-RUN-FRAMETIME}`

#### Last Run: `{$ACTUAL-RUN-FRAMETIME}`

#### Net Improvement `XX%` | `{$PREVIOUS-RUN-FRAMETIME} -> {$ACTUAL-RUN-FRAMETIME}`

---

### Average Framerate Before vs After:

#### Previous Run: `{$PREVIOUS-RUN-FRAMERATE}`

#### Last Run: `{$ACTUAL-RUN-FRAMERATE}`

#### Net Improvement `XX%` | `{$PREVIOUS-RUN-FRAMERATE} -> {$ACTUAL-RUN-FRAMERATE}`

---

### Average GPU Usage Before vs After:

#### Previous Run: `{$PREVIOUS-RUN-GPU-USAGE%}`

#### Last Run: `{$ACTUAL-RUN-GPU-USAGE%}`

#### Net Improvement `XX%` | `{$PREVIOUS-RUN-GPU-USAGE%} -> {$ACTUAL-RUN-GPU-USAGE%}`

---

### Average VRAM Usage Before vs After:

#### Previous Run: `{$PREVIOUS-RUN-NET-VRAM-USAGE}`

#### Last Run: `{$ACTUAL-RUN-NET-VRAM-USAGE}`

#### Net Improvement `XX%` | `{$PREVIOUS-RUN-NET-VRAM-USAGE} -> {$ACTUAL-RUN-NET-VRAM-USAGE}`

---

### Average Draws per Frame Before vs After:

#### Previous Run: `{$PREVIOUS-RUN-DRAWS-PER-FRAME}`

#### Last Run: `{$ACTUAL-RUN-DRAWS-PER-FRAME}`

#### Net Improvement `XX%` | `{$PREVIOUS-RUND-RAWS-PER-FRAME} -> {$ACTUAL-RUN-DRAWS-PER-FRAME}`

---

## EXECUTION `RUN01-23_50_25_11_2026`

### Log Level

`INFO`

### LOG PATH

[`RUN01-23_50_25_11_2026-INFO.log`](./RUN01-23_50_25_11_2026-INFO.log)

---

### Sample Scope

The log contains approximately 10 seconds of useful runtime. The window was closed during capture; the title did not finish suspending before the timeout and was terminated. The values below are log samples, not averages over a complete session.

---

### Observed Performance

- **Frametime / FPS:** `485.6 ms/frame (2.1 FPS)` → `632.8 ms/frame (1.6 FPS)` 🔻 Worsened.
- **GPU usage:** no system-level GPU usage average was captured. The internal counter reports estimated activity of `13%` → `24%` across the available samples. ⚠️ Inconclusive: higher activity alone does not indicate better or worse performance.
- **Local VRAM:** `7,138 MB / 6,818 MB budget` → `7,163 MB / 6,807 MB budget` 🔻 Worsened. Both samples exceeded the WDDM budget by approximately `320–356 MB`.
- **Texture copies on the GPU:** `1,586 MB` alive in the first accounting sample; cumulative uploaded-texture counter: `2,667 MB`. ⚠️ No comparable second sample; trend is inconclusive.
- **Draws:** `343` → `1,036 draws/frame` ⚠️ Inconclusive. The log also records thousands of draws dropped while their pipelines were still being created.

---

### Memory Findings

The log confirms GPU memory pressure: process usage exceeded the local memory budget Windows assigned to the adapter. The runtime reports no room (`room false`) and stops admitting more bindless texture copies while releasing idle copies. Total usage nevertheless remains above budget, so limiting the texture queue alone does not constrain every allocation, including render targets and buffer copies.

This log **does not contain the periodic `memory:` line with process private memory, available system RAM, or remaining commit**. It also cannot provide a full-run average for GPU/VRAM or a comparison against a previous run. System memory usage (32 GB RAM plus page file) cannot be quantified from this capture.

---

### Previous Run Comparison

Not available: this directory does not contain a documented previous run yet. No percentage improvement is calculated.

---

### Follow-up for the Next Run

1. Capture the startup and several minutes of runtime, including periodic `memory:` lines to measure private memory and commit.
2. Keep `VICTORY_CARD_ACCOUNTING=1` enabled to track local VRAM use and texture copies.
3. Record external RAM, VRAM, and GPU sensor readings at fixed intervals; this log does not provide averages for those sensors.
4. Compare future runs against this reference: `1.6–2.1 FPS` in the available samples and `7,138–7,163 MB` VRAM usage against a `6,807–6,818 MB` budget.
