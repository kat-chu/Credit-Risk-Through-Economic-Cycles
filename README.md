# Credit Risk Through Economic Cycles
### Kathy Chu | March 2026

A data engineering and analysis project that connects three independent data sources — 
personal loan records, credit rating spreads, and macroeconomic indicators — to explore 
how borrower risk and economic conditions interact over time.

---

## Project Overview
This project analyzes LendingClub personal loan data spanning 2007–2018 to explore the 
relationship between borrower risk, credit ratings, and macroeconomic conditions at the 
time of loan issuance. Three separate data sources were cleaned, stored in a SQLite 
relational database, and joined into a single unified dataset for visualization in Tableau.

---

## Data Sources

| Source | Type | Description |
|--------|------|-------------|
| LendingClub | CSV (Flat File) | 2.2M personal loan records with borrower financials, loan terms, and repayment status from 2007–2018 |
| NYU Stern (Damodaran) | Web Scrape (HTML) | Credit rating default spreads mapping letter grades to risk percentages |
| FRED (Federal Reserve) | API (JSON) | Monthly U.S. unemployment rate from 2007–2018 |

---

## Tech Stack
- **Python** — pandas, numpy, requests, sqlite3, kagglehub
- **SQLite** — relational database for storing and joining the three datasets
- **Tableau Public** — interactive data visualizations
- **Jupyter Notebook** — development environment

---

## Project Structure

| Notebook | Description |
|----------|-------------|
| `Milestone 2` | Cleaning the flat file (LendingClub CSV) |
| `Milestone 3` | Cleaning website data (NYU Stern credit ratings) |
| `Milestone 4` | Connecting to the FRED API and cleaning the data |
| `Milestone 5` | Merging all sources into SQLite and visualizing in Tableau |

---

## How It Works

**Step 1 — Data Collection and Cleaning**
Each source required its own cleaning pipeline. The CSV had 145 columns, empty fields, 
duplicates, and income outliers. The HTML table had no headers and required positional 
selection and type coercion. The API returned everything as strings requiring date 
normalization before any join could work.

**Step 2 — Load into SQLite**
All three cleaned datasets were loaded into a SQLite database as individual tables — 
`loans`, `ratings`, and `unemployment`.

**Step 3 — SQL Joins**
A two-step LEFT JOIN merged all three tables into a single unified dataset of 2.2M records:
- `loans` → `ratings` via `grade` / `major_grade`
- `loans` → `unemployment` via `year_month`

**Step 4 — Visualization**
The merged dataset was exported to CSV and visualized in Tableau Public across 5 interactive dashboards.

---

## Visualizations

### 1. Average Interest Rate by Loan Grade
![Average Interest Rate by Loan Grade](https://raw.githubusercontent.com/kat-chu/Credit-Risk-Through-Economic-Cycles/main/images/viz1_interest_rate_by_grade.png)
[View Interactive Version on Tableau Public](https://public.tableau.com/app/profile/kat.chu/viz/AvgInterestRatebyGrade/AvgInterestRatebyGrade)

---

### 2. Loan Volume Over Time
![Loan Volume Over Time](https://raw.githubusercontent.com/kat-chu/Credit-Risk-Through-Economic-Cycles/main/images/viz2_loan_volume_over_time.png)
[View Interactive Version on Tableau Public](https://public.tableau.com/app/profile/kat.chu/viz/LoanVolumeOverTime/LoanVolumeOverTime)

---

### 3. Loan Volume vs Unemployment Rate
![Loan Volume vs Unemployment Rate](https://raw.githubusercontent.com/kat-chu/Credit-Risk-Through-Economic-Cycles/main/images/viz3_loan_volume_vs_unemployment.png)
[View Interactive Version on Tableau Public](https://public.tableau.com/app/profile/kat.chu/viz/LoanVolumevsUnemploymentRate/LoanVolumevsUnemploymentRate)

---

### 4. Average Loan Amount and Interest Rate by Grade and Purpose
![Average Loan Amount and Interest Rate by Grade and Purpose](https://raw.githubusercontent.com/kat-chu/Credit-Risk-Through-Economic-Cycles/main/images/viz4_loan_amount_and_interest_rate_by_grade_and_purpose.png)
[View Interactive Version on Tableau Public](https://public.tableau.com/app/profile/kat.chu/viz/AvgLoanAmountandInterestRatebyGradeandPurpose/LoanPortfolioRisk)

---

### 5. Interest Rate Pricing by Economic Condition
![Interest Rate Pricing by Economic Condition](https://raw.githubusercontent.com/kat-chu/Credit-Risk-Through-Economic-Cycles/main/images/viz5_interest_rate_by_economic_condition.png)
[View Interactive Version on Tableau Public](https://public.tableau.com/app/profile/kat.chu/viz/InterestRatePricingbyEconomicCondition/InterestRatePricingbyEconomicCondition)

---

## Key Findings
- Interest rates rise steadily from grade A (~7%) to grade G (~28%), confirming LendingClub's grading system is strongly tied to risk pricing
- Loan volume grew exponentially from 2007 to 2018 with notable volatility after 2015
- As unemployment peaked during the 2008 financial crisis loan volume was nearly flat, then exploded as the economy recovered — a clear inverse relationship
- Grade D loans carry nearly 19% default risk premium versus less than 1% for grade A
- Even grade A borrowers paid higher rates during crisis-level unemployment, suggesting macroeconomic conditions influence pricing across all risk tiers

---

## How to Run
1. Clone this repository
2. Install dependencies: `pip install pandas numpy requests kagglehub`
3. Get a free FRED API key from [fred.stlouisfed.org](https://fred.stlouisfed.org/docs/api/api_key.html) and replace `YOUR_FRED_API_KEY` in the notebook
4. Run `mile5_DSC540_KChu.ipynb` from top to bottom — it will download the LendingClub dataset automatically via kagglehub
5. View the interactive Tableau visualizations via the links above

---

## Notes
- `lending_merged_final.csv` is not stored in this repository due to file size — it is generated automatically when running the notebook
- The NYU Stern ratings data is scraped live each time the notebook runs
- FRED API data is pulled live and requires a free API key
- Never commit your raw API key to GitHub — replace with `YOUR_FRED_API_KEY` before pushing

---

## Future Work
- Extend analysis using HMDA mortgage data for a more current and geographically granular view of credit risk and economic conditions
- Build a logistic regression model to predict loan default using the merged dataset
- Incorporate state-level unemployment data for more precise geographic analysis
