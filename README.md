# Freight Rate Prediction Challenge

See `freight-rate-ml-assessment.pdf` for the original assessment instructions.

## Repo layout

- `freight_rate_prediction.ipynb` - all solution code: EDA, data cleaning, model comparison, final model, and generation of both submission files.
- `train-test.csv` - labeled development data (48,000 loads, Jan-Oct 2025).
- `validation.csv` / `validation-predictions-template.csv` - the 12,000 loads to predict, and the template for the output.
- `december-chart-inputs.csv` - fixed-route December inputs to predict (date is the only thing that varies).
- `score.py` / `requirements.txt` - the scorer provided with the assessment; validates output format and renders the December chart.
- `validation_predictions.csv` - final output: `load_id,predicted_rate` for all 12,000 validation loads.
- `december_predictions.csv` - final output: the December inputs with `predicted_rate` filled in.
- `report.pdf` - validation/split approach, model comparison, and the December chart.

## Setup

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

`requirements.txt` covers both the scorer and the modeling notebook (pandas, numpy, matplotlib, scikit-learn). You'll also need Jupyter to open `freight_rate_prediction.ipynb`, or run it headless:

```bash
pip install jupyter nbconvert
jupyter nbconvert --to notebook --execute --inplace freight_rate_prediction.ipynb
```

Running the notebook top-to-bottom reproduces both output files from scratch: it loads `train-test.csv`, fixes the weight sign-error bug and imputes missing values, compares a baseline/linear/random-forest/gradient-boosting model on a time-based holdout, retrains the chosen linear regression on the full labeled set, and writes `validation_predictions.csv` and `december_predictions.csv`.

## Validate the output

```bash
python score.py --predictions validation_predictions.csv --december-predictions december_predictions.csv
```

This checks both files against the required format and regenerates `scorer_results/candidate_december.png` (the chart included in `report.pdf`).

## Notes

- Model choice and validation methodology are explained in `report.pdf`.
- `scorer_results/` is gitignored - rerun the command above to regenerate it.
