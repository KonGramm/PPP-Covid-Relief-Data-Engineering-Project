# PPP COVID-19 Financial Aid — Data Engineering Analysis

An end-to-end data engineering pipeline and exploratory analysis of the U.S. **Paycheck Protection Program (PPP)** — a $1 trillion federal business loan program launched in 2020 to help businesses and sole proprietors continue paying employees during COVID-19. The project simulates a real analyst workflow: efficiently ingesting large multi-file government datasets, cleaning and typing them correctly, merging in external population data, and producing a geographic/demographic/industry breakdown of financial aid distribution.

## Overview

Built as a data engineering exercise, this project focuses less on advanced modeling and more on the *engineering* challenges of working with large, messy, real-world government data: optimizing memory usage, handling inconsistent formatting, sampling large files efficiently, and merging datasets that key on inconsistent geographic identifiers (2-letter state codes vs. full state names).

The final output answers: **how was PPP financial aid distributed across states, cities, business types, and demographics — and which states received the most funding relative to their population?**

## What's in this repo

| File | Description |
|---|---|
| `PPP_Covid_Relief_Data_Engineering_Code_Analysis.ipynb` | Full pipeline: data type optimization, column selection, random sampling & merging of all PPP files, data cleaning, EDA, and population-normalized analysis |
| `PPP_Covid_Relief_Data_Engineering_Report.pdf` | Written report with all figures, tables, methodology, and conclusions |
| `PPP_Covid_Relief_Project_Description` | Original homework assignment/task description |

## Methodology

### 1. Load Data
- Loaded a sample of rows from one PPP file to inspect column structure and data types.
- Identified columns with predominantly missing values (e.g. `NonProfit`, `SBAGuarantyPercentage`) and excluded them.
- Built a `dtype_mapping` dictionary to enforce correct, memory-efficient data types on load (most columns were incorrectly typed by default).
- Selected only the columns needed for the analysis to reduce memory footprint.
- Iterated through all raw PPP files, drew a random sample from each, and concatenated the samples into a single working DataFrame — a strategy to keep the full multi-file dataset analysis-ready without exceeding memory limits.
- Loaded supplementary datasets: [Opportunity Insights](https://www.opportunityinsights.org/data/) job postings data and [U.S. Census Bureau](https://www.census.gov/programs-surveys/popest.html) county population estimates.

### 2. Clean Data
- Standardized inconsistent text fields (e.g. city names in mixed case such as `"Houston"` vs `"HOUSTON"`) by uppercasing and grouping to avoid duplicate categories.
- Handled missing/unanswered demographic fields explicitly rather than dropping them, to preserve the ability to report on data completeness.

### 3. Analyze Data
Exploratory analysis covering:
- Top 20 borrower cities by number of loans
- Loan count and total loan amount by state
- Loan distribution by business type (Sole Proprietorship, LLC, Corporation, etc.)
- Total jobs reported by business type
- Loan distribution by race, ethnicity, and gender
- **Loan Amount per 100k Residents** — merged PPP totals with Census population estimates (via a state-code-to-state-name mapping dictionary) to normalize loan amounts by state population, revealing which states received disproportionately more/less aid relative to size

### 4. Output Data
- Cleaned, merged datasets and the final per-capita loan amount table exported for reporting and visualization.

## Key findings

- **California, Texas, and Florida** received the highest total number of loans and loan amounts, consistent with their large economies and business density.
- **Chicago, Houston, Miami, and Los Angeles** emerged as the top borrower cities.
- **Sole Proprietorships** received the highest number of loans, reflecting the program's emphasis on small/independent businesses, while **Corporations and LLCs** accounted for the largest share of reported jobs.
- A large share of demographic fields (race, ethnicity, gender) were unanswered/unknown, limiting equity analysis — among reported data, White and male-owned businesses received the most loans.
- When normalized by population, smaller states like **North Dakota and South Dakota** received the highest loan amount per 100k residents — a very different picture than the raw totals suggest.

See the full [report](PPP_Data_Engineering_Report.pdf) for all figures, tables, and detailed commentary.

## Tech stack

- Python
- `pandas` — data loading, dtype optimization, cleaning, merging, aggregation
- `matplotlib`, `seaborn` — visualization
- Jupyter Notebook / Google Colab

## Data sources

- [SBA PPP Loan Data](https://data.sba.gov/dataset/ppp-foia)
- [Opportunity Insights — Economic Tracker (Job Postings)](https://www.opportunityinsights.org/data/)
- [U.S. Census Bureau — County Population Estimates](https://www.census.gov/programs-surveys/popest.html)

## How to run

```bash
git clone https://github.com/KonGramm/<repo-name>.git
cd <repo-name>
pip install pandas matplotlib seaborn jupyter
jupyter notebook PPP_Data_Engineering_Analysis.ipynb
```

> Note: raw PPP/Census/Opportunity Insights source files are not included in this repo due to file size — download them from the links above before running the notebook.

## Author

Konstantinos Grammenos — MSc in Statistics, Athens University of Economics and Business (AUEB)
Course: Data Engineering — Prof. S. Kehagias
