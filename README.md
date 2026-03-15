# Smart Trial Targeting: Predicting Football Transfer Fees Using Market Analytics & Player Quality Metrics

## University of California, Irvine | DATA 210P | Statistical Methods in Machine Learning | March 2026

---

### Project Overview

This project builds a two-phase machine learning pipeline that predicts football transfer fees and generates actionable trial-targeting recommendations for unsigned players (aged 21-24).

**Phase 1:** Market-level features from 9 European leagues (177,000+ transfer records, 1992-2022)  
**Phase 2:** FIFA player ratings (FIFA 15-22, ~142,000 player-year records) providing squad quality metrics

**Best Model:** Gradient Boosting Regression with Test R² = 0.5549, explaining 55% of variance in log-transformed transfer fees.

---

### Problem Statement

Every year, thousands of young unsigned footballers spend significant money attending open trials at professional clubs, with no data on whether a club is likely to sign someone with their profile. This project helps unsigned players answer:

1. **Which clubs want my position?** Club hiring heatmaps reveal position-specific signing trends.
2. **Which leagues value my age?** League-level analysis shows where 21-24 year olds are most actively signed.
3. **What fee would someone like me command?** The trained model predicts expected transfer fees.

---

### Repository Structure

```
├── README.md
├── DATA_Football_Prediction.ipynb    # Main notebook (run in Google Colab)
├── data/
│   ├── transfers/                    # Transfer data (9 leagues)
│   │   ├── premier-league.csv
│   │   ├── serie-a.csv
│   │   ├── 1-bundesliga.csv
│   │   ├── primera-division.csv
│   │   ├── ligue-1.csv
│   │   ├── championship.csv
│   │   ├── eredivisie.csv
│   │   ├── liga-nos.csv
│   │   └── premier-liga.csv
│   └── fifa/                         # FIFA player ratings
│       ├── players_15.csv
│       ├── players_16.csv
│       ├── players_17.csv
│       ├── players_18.csv
│       ├── players_19.csv
│       ├── players_20.csv
│       ├── players_21.csv
│       └── players_22.csv
├── reports/
│   └── Transfer_Fee_Report.docx      # Final project report
└── requirements.txt
```

---

### Data Sources

| Source | Description | Records |
|--------|-------------|---------|
| Transfermarkt | Transfer records across 9 European leagues (1992-2022) | 177,412 |
| EA Sports FIFA 15-22 (Kaggle: stefanoleone992) | Player ratings, attributes, club data | ~142,000 |

**Transfer Leagues:** Premier League, Serie A, Bundesliga, La Liga, Ligue 1, Championship, Eredivisie, Liga Nos, Russian Premier Liga

---

### Models and Results

| Model | CV R² | Test R² | MAE | RMSE |
|-------|-------|---------|-----|------|
| Ridge Regression (Phase 1) | 0.4821 | 0.5024 | 0.4910 | 0.6255 |
| Ridge Regression (Phase 2: +FIFA) | 0.4857 | 0.5071 | 0.4891 | 0.6226 |
| Random Forest (Phase 1) | 0.5107 | 0.5419 | 0.4645 | 0.6002 |
| Random Forest (Phase 2: +FIFA) | 0.5081 | 0.5420 | 0.4635 | 0.6001 |
| **Gradient Boosting (Phase 1) ★** | **0.5174** | **0.5549** | **0.4533** | **0.5916** |
| Gradient Boosting (Phase 2: +FIFA) | 0.5163 | 0.5509 | 0.4536 | 0.5943 |

**Key Finding:** Gradient Boosting Phase 1 achieved the best performance. FIFA club-level aggregates provided marginal improvement for Ridge but were largely redundant with market spending features for tree-based models. This motivates future work with individual player-level matching.

---

### How to Run

1. Open `DATA_Football_Prediction.ipynb` in [Google Colab](https://colab.research.google.com/)
2. Upload all files from `data/transfers/` and `data/fifa/` to the Colab runtime
3. Run all cells: `Runtime → Run all`
4. The prediction tool at the end allows you to input any age + position and get club recommendations

---

### Key Features Engineered

- `club_spend_3yr`: Rolling 3-year mean spending per club (lagged to prevent leakage)
- `club_tier`: Historical mean log(fee) per club (prestige indicator)
- `pos_exact_demand`: Mean fee per exact position per year
- `league_yr_avg`: League-specific annual fee average
- `market_infl`: Global market inflation index
- `age²`: Quadratic age term capturing non-linear age-value curve
- `squad_overall`: FIFA squad average rating (Phase 2)
- 13 one-hot encoded position dummies

---

### Requirements

- Python 3.10+
- See `requirements.txt` for full dependency list

---

### License

This project was completed as part of academic coursework at UC Irvine. Data sourced from Transfermarkt and Kaggle under their respective terms of use.

---

### Note on Large Files

The FIFA CSV files (13MB each) may exceed GitHub's file size recommendations. If you encounter upload issues:

**Option 1: Use Git LFS**
```bash
git lfs install
git lfs track "*.csv"
git add .gitattributes
```

**Option 2: Upload data separately**
Keep only the notebook, README, requirements.txt, and report in the repo. Add data download instructions instead.

**Option 3: Zip the data folder**
```bash
cd data
zip -r transfers.zip transfers/
zip -r fifa.zip fifa/
```
Then upload the zips and unzip in Colab.
