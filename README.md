# NFL Win Correlation

This project explores the relationship between team-level offensive statistics and regular-season wins in recent NFL regular seasons. Using publicly available offensive and standings data, I perform data cleaning, integration, correlation analysis, and visualization to identify which metrics are most associated with winning.

The project emphasizes reproducible data analysis, handling real-world messy datasets, and statistical reasoning.

---

## Project Goals

- Clean and standardize real-world sports datasets
- Merge offensive performance data with team standings
- Quantify relationships between offensive metrics and team wins
- Visualize correlations clearly
- Demonstrate reproducible data analysis in Python

---

## Data Sources

- **Team Offensive Statistics (2021)**  
  Season-level offensive metrics including passing, rushing, scoring, efficiency, and penalties.  
  Source: [Pro Football Reference – Team Offense 2021](https://www.pro-football-reference.com/years/2021/)  
  > Data was downloaded and **cleaned and renamed manually** for analysis.

- **AFC and NFC Standings (2021)**  
  Team win totals by conference.  
  Source: [Pro Football Reference – 2021 Standings](https://www.pro-football-reference.com/years/2021/)  

- **Team Offensive Statistics (2022)**  
  Season-level offensive metrics including passing, rushing, scoring, efficiency, and penalties.  
  Source: [Pro Football Reference – Team Offense 2022](https://www.pro-football-reference.com/years/2022/)  
  > Data was downloaded and **cleaned and renamed manually** for analysis.

- **AFC and NFC Standings (2022)**  
  Team win totals by conference.  
  Source: [Pro Football Reference – 2022 Standings](https://www.pro-football-reference.com/years/2022/)  

- **Team Offensive Statistics (2023)**  
  Season-level offensive metrics including passing, rushing, scoring, efficiency, and penalties.  
  Source: [Pro Football Reference – Team Offense 2023](https://www.pro-football-reference.com/years/2023/)  
  > Data was downloaded and **cleaned and renamed manually** for analysis.

- **AFC and NFC Standings (2023)**  
  Team win totals by conference.  
  Source: [Pro Football Reference – 2023 Standings](https://www.pro-football-reference.com/years/2023/)  

- **Team Offensive Statistics (2024)**  
  Season-level offensive metrics including passing, rushing, scoring, efficiency, and penalties.  
  Source: [Pro Football Reference – Team Offense 2024](https://www.pro-football-reference.com/years/2024/)  
  > Data was downloaded and **cleaned and renamed manually** for analysis.

- **AFC and NFC Standings (2024)**  
  Team win totals by conference.  
  Source: [Pro Football Reference – 2024 Standings](https://www.pro-football-reference.com/years/2024/)  
---

## Methodology

### Data Cleaning & Standardization
- Removed Excel artifacts
- Dropped non-team summary rows (`Avg Team`, `League Total`, `Avg Tm/G`) included in the raw export before any matching or merging, so they can't get fuzzy-matched onto a real team
- Normalized team names (case, whitespace, special characters)
- Converted numeric columns to appropriate types

### Identifier Reconciliation
- Used **RapidFuzz** to resolve minor inconsistencies in team naming conventions across datasets
- Prevented data loss during merges and avoided manual corrections

### Data Integration
- Combined AFC and NFC standings
- Merged offensive stats with standings using standardized team names
- Asserted exactly 32 teams survive the merge, as a sanity check against silent duplicate or mismatched rows

### Correlation Analysis
- Used **Pearson correlation** to assess linear relationships between offensive metrics and wins
- Removed constant or non-informative variables
- Ranked metrics by strength of association with team success

### Visualization
- Bar plots comparing correlations across offensive metrics
- Highlighted the top correlations for clarity
- Included axis labels and numeric annotations

---

## Key Findings

- **Score%**, **Yards per Pass Attempt (Yds/PassAtt)**, and **ExpectedPoints** show the strongest positive correlations with team wins (0.77, 0.77, and 0.72, respectively).
  - **Score%** and **Yds/PassAtt** reflect offensive efficiency.
  - **ExpectedPoints** represents the total expected points contributed by all offensive plays over the season, showing that teams generating more scoring opportunities across the season tend to win more games.

- Passing production is also meaningfully associated with wins: **PassYds** (0.66) and **PassTD** (0.63) both show moderately strong positive correlations, as does **Pass 1stD** (0.55).

- Rushing production shows a positive but weaker relationship: **RushTD** (0.57) and **RushYds** (0.41) correlate positively with wins, while **RushYds/Att** (0.20) is only weakly associated — suggesting rushing volume and scoring matter more than per-carry efficiency.

- Turnover-related metrics (**TO%, Fumbles Lost, Interceptions**) show weak negative correlations, consistent with turnovers slightly reducing win likelihood but not dominating outcomes.

- Penalty-related metrics (**Penalties, PenYds, 1stDByPen**) show weak positive correlations, which is likely incidental rather than causal — teams that run more plays and sustain more drives may simply accumulate more penalties along the way.

- Overall, **offensive efficiency and cumulative scoring contributions (Score%, Yds/PassAtt, ExpectedPoints)** show the strongest association with wins, with passing production close behind. No single statistic dominates, and rushing efficiency in particular shows only a weak relationship with winning.

---

## Attempt to Controll for Collinearity

The findings above are pairwise Pearson correlations, which don't account for offensive stats moving together (e.g. more passing yards tends to come with a higher Score%). A multiple regression on `Score%`, `Yds/PassAtt`, `RushYds`, and `TO%` (R²=0.64) shows that once `Score%` and `Yds/PassAtt` are in the model, `RushYds` and `TO%` add essentially no independent explanatory power (p=0.72 and p=0.98).

A partial-correlation check confirms this directly — controlling for `Score%` collapses the raw correlations substantially:

| Stat | Raw r | Partial r (controlling for Score%) |
|---|---|---|
| PassYds | 0.66 | 0.20 |
| PassTD | 0.63 | 0.08 |
| RushYds | 0.41 | 0.07 |
| RushTD | 0.57 | 0.22 |

In other words, most of what looks like a "passing volume matters" or "rushing matters" signal in the raw correlations is largely explained by teams that score efficiently also happening to accumulate more yards and touchdowns along the way — not by yardage or touchdowns independently driving wins.

## Multi-Season Analysis 

A single season (2023, n=32) is a thin sample for ranking 16 predictor variables by correlation strength. Extending the same pipeline to 2021, 2022, and 2024 brings the sample to n=128 team-seasons.

| Stat | 2023 only (n=32) | 2021–2024 (n=128) |
|---|---|---|
| Score% | 0.77 | 0.77 |
| ExpectedPoints | 0.72 | 0.77 |
| Yds/PassAtt | 0.77 | 0.71 |
| PassTD | 0.63 | 0.64 |
| PassYds | 0.66 | 0.58 |
| RushTD | 0.57 | 0.53 |
| TO% | -0.09 | -0.43 |
| TO | -0.13 | -0.43 |
| Int | -0.13 | -0.39 |
| Penalties | 0.22 | -0.07 |
| PenYds | 0.34 | 0.00 |

The top-line finding holds up at scale: **Score%, ExpectedPoints, and Yds/PassAtt remain the strongest predictors of wins.** Two things change meaningfully with more data:

- **Turnovers matter more than the single-season data suggested.** `TO%`, `TO`, and `Int` all show a much stronger negative relationship with wins once the sample grows (roughly -0.13 → -0.43). The 2023-only sample was too small to pick this up reliably.
- **The penalty correlations were noise.** `Penalties` and `PenYds` looked weakly positive in 2023 alone but flatten to roughly zero across four seasons, consistent with the incidental relationship suspected in the single-season findings — teams that run more plays picking up more penalties along the way, rather than penalties themselves being linked to winning.

---

## Limitations

- The single-season findings above are based on n=32 teams — a small sample for ranking 16 predictor variables by correlation strength, which is part of why the multi-season extension was added.
- The multi-season analysis treats each team-season as an independent observation, which isn't strictly true (the same team across different years isn't fully independent of itself). This is a standard simplification for this kind of project, but worth keeping in mind rather than treating this as 128 truly independent samples.
- These are still Pearson correlations (and one small OLS regression) rather than a fully controlled multivariate model — the regression addresses collinearity between the top few stats, but wasn't extended to the full 16-variable set or run on the multi-season data.

---

## Repository Structure

```text
NFL-Win-Correlation/
│
├─ data/
│   ├─ team_offense_2021.csv
│   ├─ team_offense_2022.csv
│   ├─ team_offense_2023.xlsx
│   ├─ team_offense_2024.csv
│   ├─ afc_standings_2021.csv
│   ├─ afc_standings_2022.csv
│   ├─ afc_standings_2023.xlsx
│   ├─ afc_standings_2024.csv
│   ├─ nfc_standings_2021.csv
│   ├─ nfc_standings_2022.csv
│   ├─ nfc_standings_2023.xlsx
│   ├─ nfc_standings_2024.csv
│
├─ win_analysis.ipynb
│
├─ requirements.txt
├─ README.md
```
