# Part 3: Churn Prediction Model & Model Card

This repository builds, evaluates, interprets, and documents a 60-day churn prediction model for the D2C capstone.

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook churn_model.ipynb
```

The raw CSV files are included under `data/raw/`. The notebook uses `rfm_modeling_snapshot.csv`, which contains only features available on or before the customer snapshot date.

## Required Outputs

- `churn_model.ipynb` - full modeling workflow
- `model.pkl` - saved final model artifact
- `metrics.json` - baseline and final model metrics
- `error_analysis.md` - false positive/false negative analysis with customer IDs
- `model_card.md` - structured model card

## Loading the Saved Model

`model.pkl` is a plain Python pickle dictionary containing preprocessing metadata, coefficients, and the selected threshold. See the final cells in `churn_model.ipynb` for an example scoring function.
