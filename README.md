# Telepass Insurance Conversion Prediction

## Overview

This project investigates how Telepass can use insurance quote, customer, pricing, and transaction data to improve quote conversion and support its insurance-platform strategy. The analysis combines 36,173 insurance quotes created between January and June 2020 with customer transaction history from June 2019 to June 2020.

Exploratory analysis and predictive modelling were used to identify factors associated with policy issuance and to compare a logistic regression baseline with three neural-network architectures. The strongest neural network achieved a test ROC-AUC of 0.786 and PR-AUC of 0.612, outperforming the logistic regression baseline. The results suggest that the model is useful for ranking and prioritising prospective customers, but its precision is not high enough for fully automated decisions.

## Business Questions

- Which customer, quote, broker, and behavioural factors are associated with insurance conversion?
- Can a predictive model identify customers who are more likely to purchase a quoted policy?
- How could Telepass improve its brokerage performance and customer targeting?
- Does the available evidence support expansion into direct insurance provision?

## Dataset

The analysis uses two linked tables:

- **Insurance Quotes:** 36,173 quotes issued from January to June 2020, including customer, vehicle, broker, pricing, and policy-status information.
- **Transactions:** customer activity recorded from June 2019 to June 2020.

To reduce leakage, transaction features were limited to activity occurring before each quote month and were then aggregated at quote level.

The observed quote-to-policy conversion rate was **27.96%**.

## Methodology

The workflow includes:

1. Data cleaning and validation
2. Exploratory data analysis
3. Feature engineering and leakage controls
4. A stratified 70/15/15 train-validation-test split
5. Preprocessing with imputation, one-hot encoding, and standardisation
6. Logistic regression as an interpretable baseline
7. Three neural-network experiments with class weighting and regularisation
8. Evaluation using accuracy, ROC-AUC, PR-AUC, precision, recall, and F1-score

## Model Performance

| Model | Test Accuracy | ROC-AUC | PR-AUC |
|---|---:|---:|---:|
| Logistic regression | 0.602 | 0.693 | 0.438 |
| Tuned logistic regression | 0.601 | 0.692 | 0.436 |
| Neural network v1 | 0.650 | 0.741 | 0.550 |
| Neural network v2 | 0.645 | 0.770 | 0.592 |
| **Neural network v3** | **0.672** | **0.786** | **0.612** |

For converted quotes, the strongest model achieved **0.74 recall**, **0.45 precision**, and an **F1-score of 0.56**.

## Key Findings

- Issued quotes had a lower average sale price than non-issued quotes: approximately **EUR 399** compared with **EUR 467**.
- Conversion varied substantially by broker, ranging from **12.83% to 32.10%**.
- Platform engagement, quote timing, price, and broker-related variables were more informative than many demographic and vehicle attributes.
- Neural networks captured useful nonlinear patterns and ranked potential converters more effectively than logistic regression.

These findings describe associations in the available data and should not be interpreted as causal effects.

## Business Recommendations

1. **Prioritise engaged customers:** Use the model to rank leads, particularly active Telepass users and customers approaching policy renewal.
2. **Improve price and broker performance:** Monitor price competitiveness and investigate why conversion differs across brokers.
3. **Use predictions as decision support:** Combine model scores with commercial judgement rather than automating customer decisions.
4. **Strengthen the brokerage model first:** The analysis supports optimisation of Telepass's brokerage activity, but it does not establish readiness to underwrite insurance directly.

## Repository Structure

```text
telepass-insurance-conversion/
├── README.md
├── requirements.txt
├── telepass_insurance_conversion.ipynb
└── data/
    └── telepass_insurance_data.xlsx  # not included
```

## Setup

Clone the repository and install the required packages:

```bash
git clone https://github.com/larasafak/telepass-insurance-conversion.git
cd telepass-insurance-conversion
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter lab
```

Place an authorised copy of the workbook in `data/telepass_insurance_data.xlsx`. In the notebook, replace the existing local file path with:

```python
file_path = "data/telepass_insurance_data.xlsx"
```

The workbook is expected to contain sheets named `Insurance Quotes` and `Transactions`.

## Data Availability

The source workbook is not included because it was provided as academic case material and redistribution rights are unclear. The notebook retains its analytical outputs so that the approach and findings can still be reviewed.

## Limitations

- The target measures quote conversion, not claim risk, profitability, or underwriting performance.
- The data covers a limited period that includes the early COVID-19 disruption.
- The analysis is observational, so identified relationships are not necessarily causal.
- Customer-level behavioural and demographic features require appropriate privacy, fairness, and governance controls before operational use.
- Moderate positive-class precision means the model should support prioritisation rather than make autonomous decisions.

## Tools

Python, pandas, NumPy, scikit-learn, TensorFlow/Keras, Matplotlib, Seaborn, SciPy, and JupyterLab.

## Author

Lara Gunseli Safak

