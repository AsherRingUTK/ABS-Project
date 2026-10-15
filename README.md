# Catcher Value Under ABS: How Robot Umps Reshape the Catcher Market

How much is a catcher worth once pitch framing stops mattering as much? This project estimates how MLB's Automated Ball-Strike (ABS) system changes catcher value. It also uses a salary model to find which catchers are overpaid or underpaid relative to what they produce.

## Overview

Framing has become one of the most valuable catcher skills. Under ABS, part of that value goes away. This analysis:

1. **Re-values catcher defense under ABS.** It keeps the blocking and throwing runs and assumes catchers keep only **45%** of their framing runs (55% is lost).
2. **Compares human-ump value with ABS value** for both defense alone and total value (offense + defense), plotted against 2026 salary.
3. **Builds an expected salary (xSalary) model** to identify market inefficiencies, meaning the most overpaid and underpaid catchers.

## Data

`Catcher_Analytics_2026_.xlsx` has 2026 data for **41 MLB catchers**, including:

- **Playing time:** G, GS, Inn
- **Offense:** BA, OBP, SLG, OPS, OPS+, Rbat
- **Defense:** FRV, Throwing, Blocking, Framing (runs)
- **Overall:** WAR
- **Contract:** 2026 Salary

## Methodology

**Engineered metrics**

| Metric | Formula |
|---|---|
| `Human_Total_Defense` | Throwing + Blocking + Framing |
| `ABS_Total_Defense` | Throwing + Blocking + (0.45 × Framing) |
| `Total_Human_Value` | Rbat + Throwing + Blocking + Framing |
| `Total_ABS_Value` | Rbat + Throwing + Blocking + (0.45 × Framing) |
| `ABS_Value_per_Million` | Total_ABS_Value ÷ Salary ($M) |

**Expected salary model:** an OLS regression on log salary.

```
log(Salary) ~ Rbat + Blocking + Throwing + Framing + WAR + Inn
```

Each catcher's residual (actual salary minus xSalary) ranks the most overpaid and most underpaid players.

## Visualizations

- **Change in Catcher Defensive Value Under ABS:** dumbbell-style scatter showing each catcher's shift from human-ump to ABS defensive value, colored by team
- **Change in Catcher Total Value Under ABS:** the same view with offense included
- **Market Inefficiency (Overpaid / Underpaid):** top 10 bar charts of actual vs. expected salary

*League-minimum players (≤ $0.8M) are left out of the value scatter plots for readability.*

## Tech Stack

Python · pandas · NumPy · statsmodels · matplotlib · seaborn · Jupyter

## How to Run

```bash
pip install pandas numpy statsmodels matplotlib seaborn openpyxl
jupyter notebook "RSM Paper.ipynb"
```

Keep `Catcher_Analytics_2026_.xlsx` in the same directory as the notebook.

## Limitations

- The 45% framing-retention rate is a modeling assumption, not an estimated value.
- One season and a small sample (41 catchers) limit how well the salary model generalizes.
- Salaries reflect contract status (pre-arb, arbitration, free agency), which the model doesn't account for.

## Author

**Asher Ring**, Data Science, University of Tennessee, Knoxville
