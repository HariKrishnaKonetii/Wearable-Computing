
---

# Dataset & Device Specification

## Concurrent Dataset (actigraph_and_fitbit directory)
- **ActiGraph (Participant 1):**
  - Files used:
    - 1_AG_week1_minute_long.csv
    - 1_AG_week2_minute_long.csv
  - Device: ActiGraph wGT3X+ (hip-worn)

- **ActiGraph (Participant 2):**
  - Files used:
    - 2_AG_week1.csv
    - 2_AG_week2.csv
  - Device: ActiGraph wGT3X+

- **Fitbit (Participant 1):**
  - File used:
    - 1_FB_minuteSteps_minute_long_converted.csv
  - Device: Fitbit (wrist-worn)
  - Original format: Fitabase minuteSteps export (wide per hour), converted to minute-level long format.

## Multiyear Dataset (multiyear directory)
- File used:
  - dailySteps.csv
- Device: Fitbit
- Used for seasonality (Repeated Measures ANOVA).
---

# EE599 Homework 2 — Answers (Application of Functions)
Name: Hari Krishna Koneti

## 1) Daily Steps (ActiGraph)
- Participant 1: arithmetic mean = 9,943.65, harmonic mean = 8,578.75 steps/day
- Participant 2: arithmetic mean = 6,750.24, harmonic mean = 5,314.89 steps/day
- Combined (all subject-days, n=37): arithmetic mean = 8,476.41, harmonic mean = 6,690.89 steps/day

Why different? Harmonic mean weights smaller daily totals more heavily, so it is typically lower than the arithmetic mean when daily steps are skewed or include low-activity days.

## 2) Group Variance (ActiGraph pooled SD across subjects)
- Participant 1 daily SD = 3,571.03 (n=20)
- Participant 2 daily SD = 2,925.92 (n=17)
- Pooled SD across subjects = 3,291.85 steps/day

## 3) Comparing Devices (Fitbit vs ActiGraph, daily totals; participant 1)
- Common aligned days = 19
- ActiGraph mean = 10,045.37 steps/day
- Fitbit mean = 13,344.74 steps/day
- t = -2.056, df = 36, p = 0.0471

Interpretation: If p < 0.05 reject H0 (devices differ). This dataset yields the p-value above. Note: only participant 1 Fitbit steps were available in provided files.

## 4) Weekend Warriors (One-way ANOVA by day-of-week, ActiGraph)
- F = 1.282, df_between = 6, df_within = 30, p = 0.2953

Interpretation: If p < 0.05, steps differ across days of the week.

## 5) Seasonality (Repeated Measures ANOVA on multiyear daily steps)
Approach: treat year as 'subject' (2013 vs 2014) and month as condition (Jan–Oct present in both years).

- F = 0.474, df_conditions = 9, df_error = 9, p = 0.8596

Interpretation: If p < 0.05, there is evidence of seasonality (monthly differences). With only 2 years, power is low; interpret cautiously.
