# Impact of AI on Work

A data science study of AI exposure risk across 655 U.S. occupations, combining labor market outcomes (BLS), occupational skill profiles (O\*NET), and expert AI exposure scores (Felten AIOE) to understand how AI affects employment and wages.

## Research Questions

1. Which occupations face the highest AI exposure risk, and what skills drive that exposure?
2. Do high-exposure occupations show different employment and wage trajectories (2019–2024)?
3. Can an unsupervised O\*NET-derived index replicate expert AI exposure ratings?

## Key Findings

- **High-exposure occupations pay more** — knowledge workers (legal, finance, tech) score highest on AIOE and earn ~30% more on average
- **Employment growth favors high-exposure roles** — top quartile grew +8.9% vs −6.5% for the bottom quartile (2019–2024)
- **Wages tell the opposite story** — lowest-exposure occupations gained the most in real wages (+5.6% vs −1.5% for highest)
- **Pre- vs. post-GenAI inflection** — employment trends for high-exposure roles accelerated in the 2022–2024 period
- **UAIOE validates strongly** — the unsupervised index correlates r = 0.94 with the expert Felten AIOE, confirming that O\*NET skill dimensions capture AI exposure

## Project Structure

```
.
├── data/
│   ├── raw/                    # Original, unmodified source files
│   │   ├── bls/                # BLS OEWS national estimates (2019, 2022, 2024)
│   │   ├── onet/               # O*NET Abilities, Work Activities, Occupation Data
│   │   └── felten/             # Felten AIOE appendix
│   ├── processed/              # Cleaned and merged outputs
│   │   ├── bls/                # CPI-deflated wage data in 2024 dollars
│   │   ├── onet/               # Standardized 93-feature skill matrix
│   │   ├── felten/             # Cleaned AIOE scores
│   │   └── merged/             # Master occupation table (655 occupations × 123 columns)
│   └── external/
│       └── cpi_deflators.csv   # BLS CPI-U factors for wage inflation adjustment
├── notebooks/
│   ├── eda/
│   │   ├── 01_bls.ipynb        # BLS data exploration and quality checks
│   │   ├── 02_onet.ipynb       # O*NET feature distributions and validation
│   │   └── 03_felten.ipynb     # AIOE score distributions
│   ├── preprocessing/
│   │   ├── 01_bls.ipynb        # Clean and CPI-deflate BLS wages
│   │   ├── 02_onet.ipynb       # Standardize O*NET ability/activity scores
│   │   ├── 03_felten.ipynb     # Clean Felten AIOE appendix
│   │   └── 04_merge_master_table.ipynb  # Inner join across all three sources
│   ├── 04_merge_datasets.ipynb          # Data integration and validation
│   ├── 05_cross_dataset_EDA.ipynb       # AIOE exposure patterns and economic outcomes
│   └── 06_uaioe_index.ipynb             # PCA, UAIOE construction, and clustering
├── pyproject.toml
└── requirements.txt
```

## Datasets

### BLS Occupational Employment and Wage Statistics (OEWS)

Three cross-sections of national occupational employment and wage estimates.

- **Source:** [https://www.bls.gov/oes/tables.htm](https://www.bls.gov/oes/tables.htm)
- **Files:** `data/raw/bls/oesm19nat.xlsx` (May 2019), `oesm22nat.xlsx` (May 2022), `oesm24nat.xlsx` (May 2024)

### O\*NET 30.1

Occupational skill profiles covering 52 abilities and 41 work activities for 774 occupations.

- **Source:** [https://www.onetcenter.org/db_releases.html](https://www.onetcenter.org/db_releases.html)
- **Files:** `data/raw/onet/Abilities.xlsx`, `Work Activities.xlsx`, `Occupation Data.xlsx`

### Felten AIOE

Expert-rated AI Occupational Exposure index for 774 occupations, ranging from −2.1 (low exposure) to +1.5 (high exposure).

- **Source:** [https://github.com/AIOE-Data/AIOE](https://github.com/AIOE-Data/AIOE)
- **File:** `data/raw/felten/AIOE_DataAppendix.xlsx`

### CPI Deflators

BLS CPI-U factors used to convert all wages to real 2024 dollars, enabling genuine purchasing-power comparisons across the three time points.

- **Source:** [https://www.bls.gov/data/inflation_calculator.htm](https://www.bls.gov/data/inflation_calculator.htm)
- **File:** `data/external/cpi_deflators.csv`

## Methodology

### Data Pipeline

1. **Preprocessing** — Each dataset is cleaned independently: BLS wages are deflated to 2024 dollars; O\*NET importance scores are standardized; Felten scores are extracted from the appendix
2. **Merging** — An inner join on SOC occupation codes produces a master table of 655 occupations with 123 columns
3. **EDA** — Cross-dataset exploration of AIOE distributions, correlations with wages and employment, and trends stratified by exposure quartile

### UAIOE Index (Unsupervised AI Occupational Exposure Index)

An alternative AI exposure metric derived entirely from O\*NET features, without using Felten scores as input:

1. **PCA** on 655 × 93 standardized O\*NET features — PC1 explains 39% of variance and captures the core manual/physical vs. cognitive/communication dimension
2. **AIOE correlation** — PC1 correlates r = −0.943 with Felten AIOE, confirming alignment
3. **UAIOE score** — principal components are weighted by their correlation with Felten AIOE and summed
4. **Validation** — UAIOE achieves r = 0.942 with Felten AIOE across 655 occupations

### Clustering

K-means (k = 5) on PCA-reduced O\*NET features identifies five occupational risk profiles:

| Cluster | Size | Exposure | Representative occupations |
|---------|------|----------|---------------------------|
| 0 | 194 | Low (−1.02) | Production, construction, manual technical |
| 4 | 94 | Low (−0.92) | Routine service roles |
| 1 | 138 | Moderate (+0.25) | Healthcare support, personal services |
| 2 | 93 | High (+0.81) | Analytical and office roles |
| 3 | 136 | Highest (+1.29) | Knowledge work, management, legal |

## Setup

```bash
# Using uv (recommended)
uv sync

# Or pip
pip install -r requirements.txt
```

Requires Python ≥ 3.13.

## Running the Notebooks

Run notebooks in order within each directory:

```
notebooks/preprocessing/01 → 02 → 03 → 04
notebooks/eda/01 → 02 → 03
notebooks/04_merge_datasets.ipynb
notebooks/05_cross_dataset_EDA.ipynb
notebooks/06_uaioe_index.ipynb
```

Processed outputs are already committed to `data/processed/`, so EDA and analysis notebooks can be run without re-running preprocessing.

## Dependencies

| Package | Purpose |
|---------|---------|
| pandas | Data manipulation and merging |
| numpy | Numerical operations |
| scikit-learn | PCA, K-Means clustering, standardization |
| scipy | Statistical tests and correlation analysis |
| matplotlib | Visualization |
| openpyxl | Reading Excel source files |
