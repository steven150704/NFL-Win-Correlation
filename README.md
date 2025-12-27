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
- Normalized team names (case, whitespace, special characters)
- Converted numeric columns to appropriate types

### Identifier Reconciliation
- Used **RapidFuzz** to resolve minor inconsistencies in team naming conventions across datasets
- Prevented data loss during merges and avoided manual corrections

### Data Integration
- Combined AFC and NFC standings
- Merged offensive stats with standings using standardized team names
- Ensured one observation per team for statistical validity

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

- **Score%**, **Yards per Pass Attempt (Yds/PassAtt)**, and **ExpectedPoints** show the strongest positive correlations with team wins (0.76, 0.76, and 0.72, respectively).  
  - **Score%** and **Yds/PassAtt** reflect offensive efficiency.  
  - **ExpectedPoints** represents the total expected points contributed by all offensive plays over the season, showing that teams generating more scoring opportunities across the season tend to win more games.  

- Traditional counting stats like **total rushing yards** and **passing yards** have weak or slightly negative correlations with wins, indicating that volume alone does not strongly predict success.  

- Turnover-related metrics (**TO%, Fumbles Lost, Interceptions**) also show weak negative correlations, consistent with turnovers slightly reducing win likelihood but not dominating outcomes.  

- Many individual metrics such as **Rush TDs**, **Pass TDs**, and **Penalty yards** show small correlations, highlighting that no single statistic guarantees team success.  

- Overall, **efficiency metrics and cumulative scoring contributions** are far more predictive of wins than raw yardage or individual counting stats, reinforcing that effective execution and consistent scoring opportunities matter more than sheer production.
S
---

## Repository Structure

```text
NFL-Win-Correlation/
│
├─ data/
│   ├─ team_offense_2023.csv
│   ├─ afc_standings_2023.xlsx
│   ├─ nfc_standings_2023.xlsx
│
├─ nfl_win_correlation_analysis.ipynb
│
├─ requirements.txt
├─ README.md
