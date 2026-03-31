# Bayesian Analysis of Argentine Football Attendance

Bayesian modeling project analyzing match attendance data from the **Argentine Primera División** to identify what drives crowd size variation across 240 home games.

## Overview

Why do some matches draw 50,000 fans while others barely reach 15,000? This project builds two competing Bayesian models to answer that question — and uses leave-one-out cross-validation to pick the winner.

**Key finding:** Team identity is the overwhelming driver of attendance. Day-of-week effects exist but are negligible by comparison. A hierarchical model captures this structure far better than a complete-pooling baseline.

## Methods

- **Exploratory Data Analysis** — missingness patterns, team-level distributions, day-of-week effects
- **Model 1 – Complete Pooling** — single Negative Binomial model assuming all games share the same expected attendance
- **Model 2 – Hierarchical (Partial Pooling)** — team and day-of-week random effects with non-centered parametrization to resolve funnel geometry and eliminate MCMC divergences
- **Model Comparison** — Pareto-smoothed LOO-CV (PSIS-LOO); hierarchical model achieves elpd_loo of −2316.6 vs −2335.1, receiving 100% of stacking weight
- **Missing Data Imputation** — posterior predictions for 22 games (9.2%) with missing attendance, using 89% HDI uncertainty intervals

## Technical Highlights

- Justified **Negative Binomial** over Poisson (overdispersion: empirical variance ≈128M >> mean ≈23,650) and over log-normal (avoids left-tail distortion)
- Resolved centered parametrization divergences by switching to **non-centered reparametrization** (z-score trick for team and day effects)
- Designed informative priors from real league data (27,600 avg. attendance in 2023); validated with prior predictive checks
- Partial pooling demonstrates shrinkage: extreme teams (Argentinos Juniors, Vélez Sarsfield) pulled toward the global mean; data-rich clubs (Boca Juniors, River Plate) barely shrink

## Results

| Model | elpd_loo | Stacking Weight |
|---|---|---|
| Hierarchical (partial pooling) | −2316.6 | **1.0 (100%)** |
| Complete pooling | −2335.1 | 0.0 |

Top predicted attendances for missing games: Vélez Sarsfield (~30,500), Racing (~30,000), Huracán (~28,600). Lowest: Argentinos Juniors (~15,200).

## Stack

```
Python · PyMC · ArviZ · pandas · matplotlib · NumPy
```

## Files

| File | Description |
|---|---|
| `bayesian_attendance_analysis.ipynb` | Full analysis: EDA, model building, diagnostics, predictions |
| `attendance-data-report.pdf` | Written report with figures and non-technical summary |
| `requirements.txt` | Python dependencies |

## Setup

```bash
git clone https://github.com/aknursherkhan/bayesian-football-attendance.git
cd bayesian-football-attendance
pip install -r requirements.txt
jupyter notebook bayesian_attendance_analysis.ipynb
```


