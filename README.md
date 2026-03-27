# Industrial_Machine_Sensor_Analysis_January
Data analysis project exploring industrial machine sensor behaviour, focusing on RPM trends, vibration patterns, and operational stability. Discovered that instability peaks between 540–600 RPM and that high RPM does not always correlate with maximum vibration. Built using Microsoft Excel (Pivot Tables, Aggregations, and Visual Analysis).
**Tool:** Microsoft Excel | **Dataset:** 4,608 sensor readings | **Domain:** Manufacturing / Predictive Maintenance

> This is the second instalment of an ongoing machine performance analysis series. The January dataset introduces two new variables, RPM (Revolution Per Minute) across three axes and Shift classification (Day/Night), enabling deeper operational analysis beyond the original dataset.

---

## 📌 Project Overview

This project analyses January 2026 industrial machine sensor data to investigate shift-based performance differences, RPM behaviour across operating conditions, and the relationship between machine speed and mechanical stress.

The dataset contains timestamped readings across the following variables:

| Variable | Description |
|---|---|
| X/Y/Z Velocity (mm/s) | Vibration levels per axis |
| X/Y/Z Frequency (Hz) | Operating frequency per axis |
| X/Y/Z RPM | Rotational speed per axis |
| Duty Cycle (%) | Machine operating load |
| Shift | Day or Night shift classification |
| Battery (%) | Power level |
| Voltage (V) | Operating voltage |
| Signal Strength | Wireless signal quality |
| Date / Time | Timestamp of each reading |

> ⚠️ **Data Quality Note:** Column1 in the raw dataset contains no values and no label. This has been flagged as a data collection gap for review.

---

## 🔍 Analytical Approach

All analysis was conducted in Microsoft Excel using the following methodology:

- Data cleaned and structured prior to analysis
- Pivot Tables used for aggregation and cross-variable comparison
- Metrics applied: AVERAGE (for trend analysis) and MAX (for spike and anomaly detection)
- Dual-method verification applied throughout, formula results validated against Pivot Table outputs

---

## 📊 Analysis Questions, Results & Insights

---

### Q1. Are day and night shifts running at different speeds (RPM)?

**Approach:** Grouped RPM data by Shift using a Pivot Table. Calculated average RPM per axis for Day and Night shifts.

| Shift | Avg X_RPM | Avg Y_RPM | Avg Z_RPM |
|---|---|---|---|
| Day | 454.11 | 465.81 | 497.58 |
| Night | 478.20 | 486.48 | 511.30 |

**Finding:** Night shifts operate at consistently higher RPM across all three axes compared to day shifts.

**Business Insight:** The machine runs faster during night shifts across every axis. This could reflect different production targets, reduced supervision, or operational protocols that differ between shifts. Operations management should investigate whether night shift speed settings are intentional or represent unmonitored behaviour that could accelerate mechanical wear.

---

### Q2. Does the night shift also experience higher vibration?

**Approach:** Calculated average velocity (mm/s) per axis grouped by Shift using a Pivot Table.

| Shift | Avg X Velocity (mm/s) | Avg Y Velocity (mm/s) | Avg Z Velocity (mm/s) |
|---|---|---|---|
| Day | 11.51 | 15.05 | 15.87 |
| Night | 12.67 | 16.34 | 17.14 |

**Finding:** Night shifts show higher average vibration levels across all axes, consistent with the higher RPM findings in Q1.

**Business Insight:** Higher speed during night shifts directly translates into higher mechanical stress. This dual pattern — faster speeds and greater vibration, suggests night shift operations place greater strain on the machine. Maintenance schedules should reflect this disparity, with more frequent checks applied to night shift periods.

---

### Q3. Are RPM spikes associated with low duty cycle?

**Approach:** Grouped Z-axis RPM by duty cycle percentage using a Pivot Table. Calculated both average and maximum Z_RPM per duty cycle band.

| Duty Cycle (%) | Avg Z_RPM | Max Z_RPM |
|---|---|---|
| 10 | 1,230 | 3,900 |
| 20 | 5,420 | 8,820 |
| 30 | 660 | 660 |
| 100 | 836 | 1,860 |

**Finding:** The most extreme RPM spikes occur at low duty cycles, particularly at 20%, not at full load. At 100% duty cycle, RPM is significantly lower and more stable.

**Business Insight:** Counter-intuitively, the machine is most mechanically erratic when operating at low load, not high load. This suggests instability during startup, idle, or transitional operating states. Engineers should investigate whether RPM control mechanisms function correctly at low duty cycles, as uncontrolled speed spikes at low load can cause premature component wear.

---

### Q4. Which axis is most unstable during low load conditions?

**Approach:** Compared maximum RPM per axis across low duty cycle bands (10–20%) using a Pivot Table.

| Axis | Max RPM (Low Load) |
|---|---|
| X | 3,120 |
| Y | 6,600 |
| Z | 8,820 |

**Finding:** The Z-axis produces the highest RPM spikes under low load conditions, making it the most mechanically unstable axis during these periods.

**Business Insight:** The Z-axis consistently emerges as the highest-risk component — a finding that aligns with the original dataset analysis where Z-axis vibration was also the highest. Maintenance prioritisation should focus on Z-axis components, particularly during low duty cycle operations where instability is most pronounced.

---

### Q5. Are the highest RPM spikes responsible for the highest vibration?

**Approach:** Cross-referenced maximum Z-axis RPM and maximum Z-axis velocity by duty cycle to test whether speed and vibration correlate directly.

| Duty Cycle (%) | Max Z_RPM | Max Z Velocity (mm/s) |
|---|---|---|
| 20 | 8,820 | 11.7 |
| 100 | 1,860 | 46.8 |

**Finding:** The answer is NO. The highest RPM (8,820 at 20% duty cycle) does not produce the highest vibration. The highest vibration (46.8 mm/s) occurs at 100% duty cycle where RPM is considerably lower at 1,860.

**Business Insight:** RPM alone is not a reliable predictor of mechanical stress. Vibration is more strongly influenced by operating load than by rotational speed. This is a critical finding for maintenance planning, monitoring RPM as a sole indicator of machine health would miss the periods of highest mechanical stress, which occur at full load. Vibration monitoring at high duty cycle is a more reliable early warning system.

---

### Q6. Do the X and Y axes also reach extreme vibration at full duty cycle?

**Approach:** Compared maximum velocity per axis at 100% duty cycle using a Pivot Table.

| Axis | Max Velocity at 100% Duty Cycle (mm/s) |
|---|---|
| X | 32.7 |
| Y | 46.4 |
| Z | 46.8 |

**Finding:** All three axes experience their highest vibration levels at 100% duty cycle. Y and Z axes are particularly severe, both exceeding 46 mm/s.

**Business Insight:** Full load operation is the highest stress condition for the entire machine not just the Z-axis. The near-identical peak values of Y (46.4) and Z (46.8) suggest that both axes are operating at or near their mechanical limits during full load. This increases the risk of simultaneous multi-axis failure. Operational protocols should consider load limits and mandatory rest periods to reduce cumulative stress.

---

### Q7. At what RPM does the machine transition from stable to unstable behaviour?

**Approach:** Grouped average Z-axis velocity by RPM band to identify the threshold where vibration increases sharply and becomes inconsistent.

| RPM | Avg Z Velocity (mm/s) |
|---|---|
| 480 | 27.25 |
| 540 | 31.65 |
| 600 | 30.52 |
| 660 | 28.91 |
| 720 | 28.49 |

**Finding:** Vibration increases sharply between 480 and 540 RPM, peaking at 31.65 mm/s at 540 RPM before gradually declining at higher speeds. This identifies 540–600 RPM as the critical instability zone.

**Business Insight:** The machine exhibits a clear instability threshold around 540–600 RPM. Operating within this range should be minimised or monitored closely. Engineers should investigate whether this threshold corresponds to a known resonance frequency of the machine's components if so, RPM control systems could be configured to pass through this range quickly rather than operating steadily within it.

---

## 💡 Key Findings Summary

| # | Finding | Business Implication |
|---|---|---|
| 1 | Night shifts run at higher RPM across all axes | Investigate night shift operating protocols |
| 2 | Night shifts produce higher vibration | Adjust maintenance schedules for night shift wear |
| 3 | RPM spikes occur at LOW duty cycle, not high | Monitor machine behaviour during startup and idle |
| 4 | Z-axis is most unstable at low load | Prioritise Z-axis maintenance |
| 5 | High RPM ≠ High vibration load is the driver | Use vibration not RPM as primary health indicator |
| 6 | All axes peak at 100% duty cycle | Introduce load limits to reduce multi-axis stress |
| 7 | Critical instability zone: 540–600 RPM | Configure RPM controls to avoid sustained operation in this range |

---

## 🛠️ Tools & Techniques Used

- **Microsoft Excel** — Data cleaning, structuring, and analysis
- **Pivot Tables** — Aggregation and cross-variable comparison
- **Functions:** AVERAGE, MAX, MIN
- **Dual-method verification** — All findings validated using both formula calculations and Pivot Table outputs
- **Shift-based segmentation** — New analytical dimension introduced in this dataset

---

## 📌 Final Conclusion

This analysis reveals that machine instability is not driven solely by RPM magnitude, but by specific operational ranges and load conditions. 

The most critical risk occurs between 540–600 RPM, suggesting a resonance-like behaviour. Additionally, maximum vibration occurs at full load (100% duty cycle), not necessarily at peak RPM.

Shift-based differences further indicate operational inconsistencies that may require process standardisation.

Overall, machine health monitoring should prioritise vibration metrics over RPM alone.

## 🔗 Related Project

This dataset is the second instalment in an ongoing analysis of the same industrial machine.
👉 [View the original sensor analysis](https://github.com/TawannahAnalytics/Industrial_Machine_Sensor_Analysis)

---

*Dataset provided as part of a supervised internship project. Company identity withheld for confidentiality. Raw data not included in this repository.*

