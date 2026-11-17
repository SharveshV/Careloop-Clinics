# CareLoop Clinics — No-Show Guard

An end-to-end capstone project exploring missed appointments across eight outpatient clinics in six Indian cities. The project cleans booking data, analyzes no-show patterns with MySQL, builds models to flag appointments at risk, and uses Gemini to structure call-centre notes into follow-up reasons.

## Business question

Which appointments are most likely to be missed, why do patients miss them, and what should CareLoop do first?

The risk score is intended for use on the evening before an appointment, after that day's reminders have been sent or skipped.

## Project workflow

1. **Clean and prepare the data** — validate and clean appointment records, document rejected rows, create features such as lead days, and enrich clinic records with weather data.
2. **Analyze with MySQL** — create the `clinics` and `appointments` tables, load the cleaned data, and query no-show patterns by clinic, booking lead time, month, and city.
3. **Build and evaluate risk models** — compare logistic regression, decision tree, and random forest models, then select a model and follow-up threshold using training data.
4. **Structure follow-up notes** — use Gemini to classify call-centre notes into allowed reasons, validate the responses, and combine the results with appointment data.

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
