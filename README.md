# CMPE 255 Project: Impact of AI on Work

# Raw data 
## BLS OEWS
Source: https://www.bls.gov/oes/tables.htm
Files:
data/raw/bls/oesm19nat.xlsx   ← May 2019
data/raw/bls/oesm22nat.xlsx   ← May 2022
data/raw/bls/oesm24nat.xlsx   ← May 2024

## O*NET 30.1
Source: https://www.onetcenter.org/db_releases.html
Files:
data/raw/onet/Abilities.xlsx
data/raw/onet/Work Activities.xlsx
data/raw/onet/Occupation Data.xlsx

## Felten AIOE
Source: https://github.com/AIOE-Data/AIOE
Files:
data/raw/felten/AIOE_main.xlsx      ← Appendix A (main scores)
data/raw/felten/AIOE_genai.xlsx     ← 2023 GenAI extension

# Progress Status
- select data sourses
- preprocess datasets (notebooks/ 01, 02, 03)
- merge preprocessed datasets to create a master dataset (notebook/ 04)
- EDA