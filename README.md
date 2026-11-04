# Cognitive Aging — NHANES 2011–2012

An exploratory analysis of the associations between age, education, recorded sex and cognitive performance among adults aged 60 years and older, using data from the National Health and Nutrition Examination Survey (NHANES) 2011–2012.

The project combines a reproducible Python notebook with a Power BI report. It focuses on delayed verbal recall and also examines verbal learning, Animal Fluency and Digit Symbol Substitution Test performance.

## Research questions

- How are age and education associated with delayed verbal recall?
- How do these associations change when recorded sex is included in the model?
- What patterns emerge for verbal learning, semantic fluency and digit-symbol performance?
- Do participants with missing delayed recall scores differ across demographic groups?

## Data and sample

The analysis uses two public NHANES 2011–2012 files:

- [Cognitive Functioning (CFQ_G)](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/CFQ_G.htm)
- [Demographics (DEMO_G)](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/DEMO_G.htm)

The files are linked by the participant identifier `SEQN`. The cognitive file contains **1,687 participants aged 60 or older examined at a Mobile Examination Center (MEC)**. Of these, **1,452** have the information required for the primary delayed recall model.

Public-use age is capped at 80: the **80+** group includes all participants aged 80 years or older.

## Cognitive measures

| Measure | What is analyzed |
|---|---|
| CERAD delayed recall | Number of words recalled after a delay, from 0 to 10 |
| CERAD learning total | Sum of the three immediate recall trials, from 0 to 30 |
| Animal Fluency | Number of animals named during the task |
| Digit Symbol Substitution Test | Performance on a timed digit-symbol task |

## Methods

- Imported the original SAS transport files using Python and checked participant identifiers, score ranges and missing values.
- Explored cognitive test availability and differences in delayed recall missingness.
- Used MEC examination weights, survey strata and primary sampling units for survey-weighted estimates and models.
- Fitted an additive, survey-weighted linear model for delayed recall with age group, education level and recorded sex. The reference categories were **60–69 years**, **less than 9th grade** and **female**.
- Fitted separate exploratory models for the secondary cognitive measures.
- Checked selected estimates, standard errors and confidence intervals with manual calculations and examined model diagnostics and sample-selection sensitivity.

The manual calculations use the same data and survey assumptions as the main analysis; they are numerical checks rather than an independent replication.

## Selected findings

Among the **1,452 complete cases in the primary analysis**, the weighted mean delayed recall scores were:

| Age group | Participants with scores | Weighted mean | 95% confidence interval |
|---|---:|---:|---:|
| 60–69 | 780 | 6.45 | 6.07 to 6.84 |
| 70–79 | 429 | 5.32 | 5.14 to 5.50 |
| 80+ | 243 | 4.12 | 3.69 to 4.56 |

In the adjusted model, participants aged **80+** had an estimated score **2.20 points lower** than those aged 60–69 (95% CI: −2.87 to −1.53), holding education and recorded sex constant. Participants with a **college degree or higher** had an estimated score **1.42 points higher** than those with less than 9th-grade education (95% CI: 0.62 to 2.21), holding age and recorded sex constant.

Delayed recall scores were missing for **233 of the 1,687** older MEC participants. Missingness varied by age and education, which is relevant when interpreting the complete-case results.

These findings are associations from an exploratory cross-sectional analysis; they do not establish causes or measure individual cognitive change over time.

## Power BI report

The Power BI report presents:

- Weighted mean delayed recall by age group.
- Weighted delayed recall missingness by age group.
- Adjusted model coefficients with 95% confidence intervals.
- Tables with the underlying estimates and explanatory notes.

The age-specific means use the **1,452 primary-model complete cases**. The missingness chart uses the **1,687 older MEC participants**. Model coefficients are precomputed and do not update when an age group is selected in the report.

To open the report:

1. Download the repository and open `NHANES_Cognitive_Aging_Dashboard_EN.pbix` in Power BI Desktop.
2. The report uses the three CSV files in `NHANES_dashboard/`. To refresh the data on another computer, update the source paths in **Transform data → Data source settings** or in each Power Query source step.
3. The missingness CSV stores percentages on a 0–100 scale: for example, `5.58` represents **5.58%**.

## Run the notebook

1. Open `nhanes_cognitive_aging_organized.ipynb` in Google Colab.
2. Run the cells in order. The notebook downloads the official source files and records download information and checksums in `data/raw/`.
3. The notebook installs `pyreadstat` if needed and uses `svy` version `0.36.0` for survey analysis. Other libraries used include pandas, NumPy, Matplotlib, SciPy and Polars.
4. The dashboard export cells generate the aggregated CSV files used in the Power BI report.

Colab runtime storage is temporary. Internet access is required to download the source files and any missing packages.

## Repository structure

- `nhanes_cognitive_aging_organized.ipynb` — analysis, checks and dashboard data export.
- `NHANES_Cognitive_Aging_Dashboard_EN.pbix` — Power BI report.
- `NHANES_dashboard/weighted_recall_by_age.csv` — weighted recall estimates by age.
- `NHANES_dashboard/weighted_missingness_by_age.csv` — weighted missingness estimates by age.
- `NHANES_dashboard/primary_model_coefficients.csv` — primary model coefficients and confidence intervals.

## Limitations

- The analysis is **cross-sectional** and cannot establish causality or within-person decline.
- The models use complete cases. Cognitive-test nonresponse differs across groups, and survey weights do not necessarily remove this potential selection bias.
- The public data do not distinguish individual ages above 80.
- Cognitive scores are bounded, and the linear models are simplified descriptions of their relationships with the selected predictors.
- Secondary analyses are exploratory, and the reported intervals are not adjusted for multiple comparisons.
- The measures are not used to diagnose cognitive impairment.

## References

- [NHANES 2011–2012: Cognitive Functioning documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/CFQ_G.htm)
- [NHANES 2011–2012: Demographic Variables documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/DEMO_G.htm)
- [NCHS Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
- [Brody et al., Cognitive Performance in Adults Aged 60 and Over, NCHS](https://www.cdc.gov/nchs/data/nhsr/nhsr126-508.pdf)

**Author:** Claudio Lombardi
