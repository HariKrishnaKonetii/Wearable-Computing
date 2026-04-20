# PostureSense — Section 5: Analysis

**Course:** EE599 Wearable Computing, Spring 2026  
**Author:** Hari Krishna Koneti  
**Hardware:** Arduino Uno R3 + SparkFun ADXL345, chest-mounted, I²C, ±16 g  

---

## Overview

This folder contains the complete Section 5 Analysis for the PostureSense research project — a single-accelerometer wearable system that classifies four sleep lying positions:

| Posture | Dominant axis | Expected value |
|---|---|---|
| Supine | ay | +1 g |
| Prone | ay | −1 g |
| Left lateral | az | −1 g |
| Right lateral | az | +1 g |

---

## Files

| File | Description |
|---|---|
| `PostureSense_Analysis_EE599.ipynb` | Full analysis notebook — run this in Google Colab or Jupyter |
| `PostureSense_Analysis_EE599.tex` | IEEE-format written analysis paper (compile with pdflatex) |
| `sketch_mar24b.ino` | Arduino firmware used to collect the dataset |
| `Data_set_3min_each_side.xlsx` | Real ADXL345 dataset (330 samples, 4 postures, 0.167 Hz) |

---

## How to Run the Notebook

1. Open [Google Colab](https://colab.research.google.com)
2. Upload `PostureSense_Analysis_EE599.ipynb`
3. Upload `Data_set_3min_each_side.xlsx` to the Colab session (same directory)
4. Run all cells (`Runtime → Run all`)

**Dependencies** (all pre-installed in Colab):
```
pandas, matplotlib, scipy, openpyxl
```

No machine learning libraries (sklearn, tensorflow, etc.) are used.  
All classification algorithms — KNN, threshold classifier, ANOVA — are implemented from scratch.

---

## Dataset

Collected directly from the ADXL345 using `sketch_mar24b.ino`:

| Parameter | Value | Source in firmware |
|---|---|---|
| Sensor range | ±16 g | `adxl.setRangeSetting(16)` |
| Scale factor | 32 LSB/g | Derived from ±16 g range |
| Sampling interval | 6000 ms → 0.167 Hz | `delay(6000)` |
| Serial output | millis(), X, Y, Z | `Serial.print(millis())` |
| Interface | I²C | `ADXL345 adxl = ADXL345()` |

The Excel file columns (`Miili Sec`, `X Axis`, `Y Axis`, `Z Axis`) map directly to the firmware serial output.

**Class distribution:**

| Posture | Samples | % |
|---|---|---|
| Supine | 57 | 17.3% |
| Prone | 103 | 31.2% |
| Left lateral | 98 | 29.7% |
| Right lateral | 72 | 21.8% |
| **Total** | **330** | |

---

## Notebook Structure

| Section | Content |
|---|---|
| 0. Imports | Standard library only — math, random, pandas, matplotlib, scipy |
| 1. Helper Functions | amean, astd, euclidean, knn_predict, segment_sse — all from scratch |
| 2. Load Real ADXL345 Data | Load, scale (÷32), label by dominant axis, class distribution |
| 3. Feature Space Visualisation | Fig 1: 3D scatter plot — Fig 2: time series with transitions |
| 4. Threshold Classifier | Rule-based dominant-axis detection (T = 0.5 g) — Hypothesis 3 |
| 5. k Sensitivity Analysis | Fig 3: KNN tested at k = 1, 3, 5, 7, 9, 11 — justifies k = 5 |
| 6. KNN + 5-Fold CV | Fig 4: confusion matrix — Fig 5: per-class metrics — Fig 6: CV folds |
| 7. SMV ANOVA | Fig 7: one-way ANOVA on Signal Magnitude Vector |
| 8. Centroid Distances | Fig 8: pairwise separation vs sensor noise floor |
| 9. Summary | Hypothesis evaluation table and limitations |

---

## Results

| Metric | Result |
|---|---|
| Threshold accuracy (H3) | **100.0%** (330/330) |
| KNN accuracy — 5-fold CV (H1) | **100.0%** |
| Cohen's kappa (H1) | **1.0000** |
| CV standard deviation | **0.0000** (no overfitting) |
| Min. recall — all classes (H2) | **1.000** |
| SMV ANOVA | F = 184.5, p = 6.3 × 10⁻⁷⁰ |
| Min. centroid separation | 1.33 g (43× sensor noise floor) |

**All three hypotheses are supported.**

The most clinically significant result is **supine recall = 1.000** — the system never misclassifies a supine period, meaning no OSA risk period is ever missed.

---

## Hypotheses

| # | Statement | Criterion | Outcome |
|---|---|---|---|
| H1 | Single-accelerometer system classifies lying positions | Accuracy ≥ 90%, κ ≥ 0.85 | ✓ Supported |
| H2 | Each posture is individually detectable | Recall ≥ 0.90 per class | ✓ Supported |
| H3 | Embedded rule-based classifier is sufficient | Accuracy ≥ 85% | ✓ Supported |

---

## Limitations

- **N = 1** — single subject, controlled static holds only
- **Free-living accuracy** estimated 85–92% based on literature (posture transitions not captured)
- **Sampling rate** (0.167 Hz) does not capture 5–15 s transition dynamics
- **Hardware note:** Earlier sections (RP1, RP2) reference ESP32 + MPU6050. Device Design, Methods Plan, and this Analysis use the finalised hardware: **Arduino Uno R3 + ADXL345**. Earlier sections will be revised in the final paper.

---

## References

1. P. Alinia and H. Ghasemzadeh, "Pervasive lying posture tracking," *Sensors*, vol. 20, no. 20, 2020.
2. P.-Y. Jeng et al., "A wrist sensor sleep posture monitoring system," *Sensors*, vol. 21, no. 1, 2021.
3. J. Razjouyan et al., "Improving sleep quality assessment using wearable sensors," *J. Clin. Sleep Med.*, vol. 13, no. 11, pp. 1301–1310, 2017.
