# CMPE 255 Project: Impact of AI on Work

## Datasets
### BLS OEWS
Source: https://www.bls.gov/oes/tables.htm
Files:
data/raw/bls/oesm19nat.xlsx   ← May 2019
data/raw/bls/oesm22nat.xlsx   ← May 2022
data/raw/bls/oesm24nat.xlsx   ← May 2024

### O*NET 30.1
Source: https://www.onetcenter.org/db_releases.html
Files:
data/raw/onet/Abilities.xlsx
data/raw/onet/Work Activities.xlsx
data/raw/onet/Occupation Data.xlsx

### Felten AIOE
Source: https://github.com/AIOE-Data/AIOE
Files:
data/raw/felten/AIOE_main.xlsx      ← Appendix A (main scores)
data/raw/felten/AIOE_genai.xlsx     ← 2023 GenAI extension

### CPI Deflators
File data/external/cpi_deflators.csv contains inflation adjustment factors 
from the BLS Consumer Price Index (CPI-U). These are used during BLS 
preprocessing to convert all wages to real 2024 dollars, so wage 
comparisons across 2019, 2022, and 2024 reflect genuine purchasing power 
changes rather than inflation. Values can be checked at: https://www.bls.gov/data/inflation_calculator.htm.

### Progress Status
- select data sourses
- preprocess datasets (notebooks/ 01, 02, 03)
- merge preprocessed datasets to create a master dataset (notebook/ 04)
- EDA