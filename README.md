# Bisnis.id User Profile EDA

![Python](https://img.shields.io/badge/Python-3.10%2B-blue)

Exploratory Data Analysis (EDA) on user registration and profile completeness data from the Bisnis.id portal (`bisnis_id_prod.User`). Prepared as a graduate Data Science midterm project (`PSIKS2W002`).

## Overview

The production `User` table contains 762,792 registered accounts across 48 columns (April 2012 to November 2026). Because the deprecated `is_retained_user` flag is no longer active in the system, this analysis focuses on demographic profile completeness (`has_demografi` and `profile_completeness_pct`) and acquisition channels (`auth_channel`) using a stratified, anonymized sample of 15,003 rows and 22 columns.

Key findings from the executed notebook (`EDA_Users_BisnisID_UTS.ipynb`):

- Google SSO accounts for 82.1% of registrations (12,317 of 15,003 in the sample), followed by Direct Email / Legacy at 17.0%.
- Over 93% of demographic fields (`gender_clean`, `age_years_raw`, `education`, `outcome_clean`) are empty and follow a Missing at Random (MAR) pattern: gender completeness is 1.61% for Google SSO users compared to 29.58% for Direct Email / Legacy users.
- Users with at least one demographic field (`has_demografi = 1`, 6.33% of cleaned rows) correlate positively with `profile_completeness_pct` (+0.887), `is_migration` (+0.682), and `account_age_days` (+0.367), and negatively with `sso_channel_count` (-0.407).
- Data cleaning removed the deprecated `login_recency_days` column (98.41% missing) and 101 rows with invalid `'0000-00-00'` registration dates (0.67%), leaving 14,902 clean rows while retaining valid long-term account ages from 2012.

## Prerequisites & Quick Start

1. Clone the repository and enter the project directory:

   ```bash
   git clone https://github.com/haluboyys/bisnisid-user-eda.git
   cd bisnisid-user-eda
   ```

2. Install the required Python packages:

   ```bash
   pip install -r requirements.txt
   ```

3. Open and run the notebook:

   ```bash
   jupyter notebook EDA_Users_BisnisID_UTS.ipynb
   ```

## Usage & Data Privacy

All outputs, tables, and charts are pre-executed inside `EDA_Users_BisnisID_UTS.ipynb` so they render directly on GitHub. The notebook follows the 9-chapter EDA structure covering problem framing, data understanding, inspection, missing data handling, descriptive statistics, transformations, bivariate/correlation analysis, outlier treatment, and findings.

To comply with personal data protection standards (UU PDP), no raw personally identifiable information (email addresses, full names, phone numbers, addresses, or password hashes) and no database credentials are stored in this repository. Text fields are anonymized into character lengths (`fullname_len`, `username_len`) or grouped categories.

<details><summary>Project Structure</summary>

```text
bisnisid-user-eda/
├── data/
│   └── users_bisnisid_sample.csv          # Anonymized stratified sample (15,003 rows, 22 columns)
├── figures/
│   ├── fig1_tren_dan_kanal.png            # Annual registration trend & auth_channel distribution
│   ├── fig2_missing_values.png            # Missing value percentages per column
│   ├── fig3_distribusi_skewness.png       # Numeric distributions with skewness & kurtosis
│   ├── fig4_transformasi_log.png          # Log transformation comparison on account_age_days
│   ├── fig5_univariate.png                # Univariate distributions (completeness, has_demografi, Active)
│   ├── fig6_bivariate.png                 # Account age vs profile completeness & has_demografi
│   ├── fig7_korelasi_heatmap.png          # Spearman correlation heatmap
│   └── fig8_outliers.png                  # Outlier detection boxplots (age & account age)
├── report/
│   └── EDA_AndraRamadanPratama_26MA41D006.pdf  # Final coursework report (Indonesian)
├── .gitignore
├── EDA_Users_BisnisID_UTS.ipynb           # Pre-executed Jupyter Notebook (Bab 1 - Bab 9)
├── README.md
└── requirements.txt
```

</details>

## Development & Verification

To verify that the dataset, notebook execution outputs, and figures are intact:

```bash
python -c "import nbformat, pandas as pd; df = pd.read_csv('data/users_bisnisid_sample.csv'); nb = nbformat.read('EDA_Users_BisnisID_UTS.ipynb', as_version=4); nbformat.validate(nb); print('CSV:', df.shape, '| Cells:', len(nb.cells))"
```

## Contributing

This repository is maintained as an academic coursework project. Suggestions and issues for improving the EDA pipeline are welcome via pull requests or issues.

## License

This repository was created for graduate coursework and does not include a `LICENSE` file yet.
