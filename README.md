# Well & Reservoir Classification — SPE Africa DSEATS Datathon 2025

Reservoir engineering analysis pipeline built for the **SPE Africa DSEATS Datathon 2025**. Given raw well production data and reservoir properties, this notebook cleans the data and derives a set of engineering diagnostics used to classify well and reservoir behavior.

## What it does

Starting from three raw datasets (well production history, reservoir properties, and classification parameters), the pipeline:

1. **Cleans the data** — strips comma-formatted numeric strings, converts to proper dtypes, parses production dates, and checks for missing values.
2. **Reservoir saturation status** — compares initial reservoir pressure to bubble point pressure to classify each reservoir as *Saturated* or *Undersaturated*.
3. **Well-to-reservoir assignment (Task 1)** — for each well, takes the maximum bottomhole flowing pressure (BHP) over its production history and assigns it to the reservoir whose current average pressure is within ±200 psi.
4. **Well EDA** — per-well plots of oil/gas/water production rates, gas-oil ratio (GOR), and watercut over time.
5. **Productivity Index (PI) trend** — computes daily oil rate, pressure drawdown, and PI (STB/day/psi), then uses linear regression to classify each well's PI trend as Flat, Increasing, or Decreasing.
6. **Watercut trend** — same regression-based classification approach applied to watercut over time.
7. **Reservoir barrels produced** — converts cumulative stock-tank oil production to reservoir barrels using each reservoir's formation volume factor (FVF), then aggregates by reservoir.
8. **Well type classification** — classifies wells as Naturally Flowing (NF) or Gas Lifted (GL) based on annulus pressure.
9. **Formation GOR trend** — computes formation gas-oil ratio (FGOR) using cumulative gas production and the reservoir's solution GOR (Rs).

## Data

The notebook expects three CSV files (originally uploaded via Colab's file picker):

| File | Description |
|---|---|
| `spe_africa_dseats_datathon_2025_wells_dataset.csv` | Well-level daily production data (cumulative oil/water/gas, BHP, annulus pressure, dates) |
| `reservoir_info.csv` | Reservoir properties (initial/current pressure, bubble point, FVF, solution GOR) |
| `classification_parameters.csv` | Reference parameters used for classification tasks |

> Data files are not included in this repo — add them locally or update the notebook's `pd.read_csv` paths before running.

## Requirements

```
numpy
pandas
matplotlib
seaborn
scipy
```

Install with:

```bash
pip install numpy pandas matplotlib seaborn scipy
```

## Usage

The notebook was written for Google Colab (see the "Open in Colab" badge at the top), but runs the same locally in Jupyter:

```bash
jupyter notebook SPE_datathon.ipynb
```

If running outside Colab, replace the `google.colab.files.upload()` cell with local file paths pointing to the three CSVs above.

## Notes

- All monetary/pressure/production columns arrive as comma-formatted strings and are cleaned before analysis — if you swap in new data, check the same columns need the same treatment.
- Trend classification (PI, watercut) uses a simple linear regression slope threshold; thresholds may need tuning for wells with noisy or sparse data.
- 1 STB oil is assumed ≈ 1 BBL for watercut calculations unless otherwise adjusted.

## Author

Kuzayet — Federal University of Technology, Minna
