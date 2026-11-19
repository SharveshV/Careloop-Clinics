# CareLoop Clinics — No-Show Guard

An end-to-end capstone project analyzing missed appointments across eight outpatient clinics in six Indian cities. The workflow cleans booking data, investigates no-show patterns with MySQL, builds an appointment risk model, and uses Gemini to structure call-centre notes into follow-up reasons.

## Four-part summary

- **Part 1 — Data preparation:** Cleaned 1,020 raw appointment records into 990 analysis-ready appointments.
- **Part 2 — SQL analysis:** Found 218 no-shows among 990 appointments, an overall no-show rate of 22.02% across eight clinics.
- **Part 3 — Risk modeling:** Logistic regression achieved the highest reference cross-validated ROC-AUC at approximately 0.77; the selected decision threshold was 0.35.
- **Part 4 — Follow-up notes:** Analyzed the first 10 notes; 9 contained text, and at least 7 of those 9 classifications agreed with the expected root causes.

## Recommendation

CareLoop should use the logistic regression model to rank appointments for evening-before outreach, after reminders have been sent or skipped. It was selected for the highest reference five-fold cross-validated ROC-AUC (about 0.77); a 0.35 threshold improved the out-of-fold F1 score to about 0.49 (precision 0.51, recall 0.47), and should be adjusted to match available call capacity. The SQL analysis found a 22.02% overall no-show rate (218 of 990 appointments), with no-shows at 60.5% for bookings made 31 or more days ahead (26 of 43) versus 16.2% for bookings made 1–7 days ahead (63 of 390). Among clinics with at least 95 appointments, Hadapsar Care Centre had the highest rate at 27.3%. Use the note analysis to shape follow-up questions and actions, while treating its 10-note sample as illustrative rather than a clinic-wide estimate.

## Project workflow

1. **Prepare the data:** Validate, clean, and enrich the appointment and clinic records; document rejected rows and generate features such as lead days.
2. **Analyze in MySQL:** Create the `clinics` and `appointments` tables, load the cleaned records, and run queries for clinic, lead-time, monthly, and city patterns.
3. **Model no-show risk:** Compare logistic regression, decision tree, and random forest models; tune and select a model and threshold using training data.
4. **Structure follow-up notes:** Use Gemini to classify notes into allowed root-cause categories, validate the responses, and join them with appointment data.

## Repository contents

```text
.
├── README.md
├── 01_ingest_clean.ipynb
├── rejected_rows.json
├── appointments_clean.csv
├── clinics_enriched.csv
├── load_data.sql
├── 02_queries.sql
├── 03_model.ipynb
├── test_risk_scores.csv
├── 04_genai.ipynb
├── notes_analysed.csv
└── data/
    ├── appointments.csv
    ├── clinics.csv
    └── followup_notes.csv
```

The files in `data/` are the original inputs and should remain unchanged. The notebooks and SQL scripts create or use the derived files shown above.

## Running the project

1. Run `01_ingest_clean.ipynb` in Google Colab to create the cleaned appointments, enriched clinics, and rejected-row report.
2. In MySQL Workbench with MySQL 8, create or select the `noshoguard` schema. Run `load_data.sql`, then run `02_queries.sql`. The clinics are loaded before appointments because `appointments.clinic_id` references `clinics.clinic_id`. Keep appointment IDs as strings, including their leading letters and zeros.
3. Run `03_model.ipynb` to compare models, choose the model and threshold, and create `test_risk_scores.csv`.
4. Run `04_genai.ipynb` to analyze the first 10 notes and create `notes_analysed.csv`. Enter the Gemini API key at runtime; never include it in the notebook source or repository.

## Limitations

- The 10 notes are an illustration and cannot establish the real reasons for missed appointments clinic-wide.
- The train/test split is random, not past-to-future, so the evaluation does not show how the model will perform on future appointments.
- The alert threshold depends on the front desk's call capacity and should be recalibrated as that capacity changes.

## Reproducibility

Use the random seeds and split settings specified in the project brief. Metrics may vary slightly across library versions; the values above are the brief's reference results.
