# NFL Win Correlation

This project explores the relationship between team-level offensive statistics and regular-season wins in the 2023 NFL season. Using publicly available offensive and standings data, I perform data cleaning, integration, correlation analysis, and visualization to identify which metrics are most associated with winning.

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

- **Team Offensive Statistics (2023)**  
  Season-level offensive metrics including passing, rushing, scoring, efficiency, and penalties.  
  Source: [Pro Football Reference – Team Offense 2023](https://www.pro-football-reference.com/years/2023/)  
  > Data was downloaded and **cleaned and renamed manually** for analysis.

- **AFC and NFC Standings (2023)**  
  Team win totals by conference.  
  Source: [Pro Football Reference – 2023 Standings](https://www.pro-football-reference.com/years/2023/)  

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

## Limitations

- Single season (2023) with n=32 teams — a small sample for ranking 16 predictor variables by correlation strength. Some of the mid-tier correlations shown here could reflect season-specific noise rather than a stable relationship, and no significance testing was performed.
- These are pairwise Pearson correlations, not a controlled multivariate model — several offensive stats are correlated with each other (e.g. PassTD and Score%), so it isn't possible from this analysis alone to say which stats are driving wins independently versus just riding along with a more fundamental one like scoring efficiency.

---

## Repository Structure

```text
NFL-Win-Correlation/
│
├─ data/
│   ├─ team_offense_2023.xlsx
│   ├─ afc_standings_2023.xlsx
│   ├─ nfc_standings_2023.xlsx
│
├─ win_analysis.ipynb
│
├─ requirements.txt
├─ README.md
```
