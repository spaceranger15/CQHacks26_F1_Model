# 🏎️ F1 2025 Season Intelligence Dashboard

An interactive machine learning dashboard that predicts Formula 1 race outcomes using Random Forest and Neural Network models, built on 26,692 laps of 2025 season data.

![F1 Dashboard](https://img.shields.io/badge/F1-2025%20Season-e94560?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0id2hpdGUiIGQ9Ik0yMSAxNkg4bC00IDRILDFWOGE0IDQgMCAwIDAgNCA4aDE2YTQgNCAwIDAgMCA0LTRWOGE0IDQgMCAwIDAtNC00eiIvPjwvc3ZnPg==)
![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Plotly](https://img.shields.io/badge/Plotly-Interactive-3F4F75?style=for-the-badge&logo=plotly&logoColor=white)

---

## 📸 Preview

The dashboard features a dark, F1-inspired UI with:
- Animated intro screen with rev sound and F1 car animation
- Per-driver hero panels with team livery and car illustrations
- Interactive championship battle chart (top 8 drivers)
- Race-by-race strategy cards with position and points tracking
- RF vs Neural Network prediction comparison charts

---

## 🚀 Features

**Machine Learning Models**
- Walk-forward Random Forest Regressor — trained only on past races to simulate real pre-race predictions
- Neural Network (MLPRegressor) — trained on the same walk-forward basis for comparison
- Feature importance analysis revealing what actually determines finishing position

**Per-Driver Dashboard**
- Season hero panel with driver photo, team livery, and car illustration
- RF vs NN accuracy badges per driver
- Race strategy scroll — every race as a card showing grid, finish, position gain, pit stops, and cumulative points
- Prediction vs actual finish chart (RF, NN, and actual overlaid)
- Positions gained/lost bar chart per race
- Full race-by-race table with tyre compound, status, and error metrics

**Welcome Screen**
- Championship battle chart — cumulative points race for the top 8
- Feature importance breakdown — what matters most to win a race
- NN accuracy by driver comparison bars

---

## 📁 Project Structure

```
f1-dashboard/
├── f1Model.ipynb          # Main notebook — data processing, ML, dashboard generation
├── RaceResults.csv          # 2025 season race results (479 rows × 27 columns)
├── LapTimes.csv             # 2025 lap-by-lap telemetry (26,692 rows × 35 columns)
├── f1_dashboard.html        # Generated interactive dashboard (open in browser)
└── README.md
```

---

## 🛠️ Setup & Usage

### Requirements

```bash
pip install pandas numpy scikit-learn plotly ipywidgets
```

### Running in Google Colab (recommended)

1. Upload `RaceResults.csv` and `LapTimes.csv` to your Google Drive
2. Open the notebook in Colab
3. Mount your Drive and update the file paths in Cell 3:
   ```python
   race_results = pd.read_csv('/content/drive/MyDrive/RaceResults.csv')
   lt = pd.read_csv('/content/drive/MyDrive/LapTimes.csv')
   ```
4. Run all cells in order
5. The final cell generates and downloads `f1_dashboard.html`

### Viewing the Dashboard

Open `f1_dashboard.html` in **Google Chrome** for the best experience. The dashboard is fully self-contained — no server needed.

---

## 🤖 How the Models Work

### Features Used

| Feature | Description | Importance |
|---|---|---|
| `GridPosition` | Starting position on the grid | ~56% |
| `QualiSecs` | Qualifying lap time in seconds | ~17% |
| `RecentForm` | Avg finish over last 3 races | ~14% |
| `TeamStrength` | Team's avg points per race | ~10% |
| `TyreEnc` | Starting tyre compound (encoded) | ~3% |

### Walk-Forward Validation

The models use walk-forward prediction — for each race, the model is trained **only on all previous races**, never on future data. This simulates a real pre-race prediction scenario and prevents data leakage.

```
Race 1  → No prediction (insufficient history)
Race 2  → Train on Race 1 → Predict Race 2
Race 3  → Train on Races 1–2 → Predict Race 3
...
Race 24 → Train on Races 1–23 → Predict Race 24
```

### Accuracy Metric

A prediction is considered **correct** if it falls within ±2 finishing positions of the actual result. Overall season accuracy:

- Random Forest: ~40% within ±2 positions (MAE ≈ 3.4 positions)
- Neural Network: ~33% within ±2 positions (MAE ≈ 4.0 positions)

---

## 📊 Dataset

The notebooks expect two CSV files exported from the [FastF1](https://github.com/theOehrly/Fast-F1) Python library:

**RaceResults.csv** — one row per driver per race, including:
`Round`, `Location`, `Abbreviation`, `FullName`, `TeamName`, `GridPosition`, `Position`, `Points`, `Status`, `Q1`, `Q2`, `Q3`, `HeadshotUrl`, `DriverNumber`, etc.

**LapTimes.csv** — one row per lap per driver per race, including:
`Round`, `Driver`, `LapNumber`, `LapTime`, `Compound`, `Stint`, `Position`, `Location`, etc.

---

## 🏗️ Tech Stack

| Tool | Purpose |
|---|---|
| Python / Pandas | Data processing and feature engineering |
| scikit-learn | Random Forest + Neural Network models |
| Plotly | Interactive charts embedded in the dashboard |
| HTML / CSS / JS | Dashboard UI with animations and interactivity |
| Google Colab | Recommended runtime environment |
| FastF1 | Source library for F1 telemetry data |

---

## 📝 Notebook Cell Guide

| Cell | Purpose |
|---|---|
| 1 | Install dependencies and imports |
| 2 | Mount Google Drive |
| 3 | Load `RaceResults.csv` and `LapTimes.csv` |
| 4 | Feature engineering (RecentForm, TeamStrength, QualiSecs, TyreEnc) + Random Forest walk-forward prediction |
| 5 | Team colors and driver mappings |
| 6 | Interactive per-driver explorer (Colab widget) |
| 7 | Overperformers bar chart |
| 8–12 | Additional analysis charts (optional) |
| 13 | Debug/check cell — verifies all variables are ready |
| 14 | **Main cell** — Neural Network, pit stop processing, and full dashboard HTML generation |
| 15 | Display dashboard inline in Colab |

---

## ⚠️ Known Limitations

- Predictions are based on historical form and qualifying — unexpected events (safety cars, crashes, weather) are not modelled
- ±2 position accuracy of ~40% reflects the inherent unpredictability of F1 racing
- The Neural Network (`MLPRegressor`) consistently underperforms the Random Forest on this dataset size

---

## 🙌 Acknowledgements

- [FastF1](https://github.com/theOehrly/Fast-F1) for the F1 telemetry data library
- [Formula 1](https://www.formula1.com) for the sport
- [Plotly](https://plotly.com) for the charting library

---

## 📄 License

MIT License — free to use, modify, and share with attribution.
