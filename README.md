# IFRS9-Expected Credit Loss Framework – Retail Loan Portfolio

## Project Overview

This project develops an end-to-end **IFRS 9 Expected Credit Loss (ECL)** framework for a retail unsecured loan portfolio using the Lending Club dataset.

The framework demonstrates how borrower-level probability of default (PD) models can be integrated with macroeconomic stress testing to produce forward-looking Expected Credit Loss estimates consistent with IFRS 9 principles.

The project is designed as a portfolio risk analytics demonstration rather than a production banking model.

---

# Project Structure

```
Notebook 1 - Data Quality Assessment
Notebook 2 - Exploratory Data Analysis
Notebook 3 - Feature Engineering
Notebook 4 - PD Model 
Notebook 5 - Macro Data
Notebook 6 - Macroeconomic Scenario Engine
Notebook 7 - Macroeconomic Sensitivity Analysis
Notebook 8 - Borrower-Level Probability of Default Stress Testing
Notebook 9 - IFRS 9 Expected Credit Loss (ECL)
```

---

# Overall Methodology

```
Loan Data
      │
      ▼
Data Cleaning
      │
      ▼
Feature Engineering
      │
      ▼
PD Model
(Logistic Regression)
      │
      ▼
Borrower Baseline PD
      │
      ▼
Macroeconomic Scenarios
      │
      ▼
Borrower-Level PD Stress Testing
      │
      ▼
Scenario PD
      │
      ▼
Stage Allocation
      │
      ▼
ECL = PD × LGD × EAD
      │
      ▼
Probability Weighted IFRS 9 ECL
```

---

# Key Assumptions

## 1. Portfolio Assumptions

- Portfolio consists of unsecured retail consumer loans.
- Lending Club loans originated during 2018.
- Portfolio is treated as a snapshot at the reporting date.
- Only one reporting date is considered.
- Loans are assumed to remain on the balance sheet during the ECL calculation.

---

## 2. Probability of Default (PD)

### PD Model

The probability of default model is developed using:

- Logistic Regression
- Class balancing applied due to highly imbalanced defaults

---

### Important Assumption

The model is **not directly driven by macroeconomic variables**.

Instead,

Macroeconomic conditions modify borrower PD through a separate stress-testing framework.

This follows the common banking approach:

```
Borrower Risk
        +
Macroeconomic Adjustment
        =
Forward Looking PD
```

---

## 3. Macroeconomic Assumptions

Historical macroeconomic data is used to represent economic cycles.

Variables include:

- Real GDP Growth
- Unemployment Rate
- Policy Interest Rate
- CPI Inflation
- House Price Growth
- Disposable Income Growth

Historical periods are used as stress anchors.

### Baseline

Current economic conditions.

### Downside

Representative of the Global Financial Crisis (2008–2009).

### Severe

Representative of the COVID-19 recession (2020).

These scenarios are historical analogues and are intended to demonstrate plausible economic downturns rather than provide economic forecasts.

---

## 4. Borrower-Level Stress Testing

Rather than applying one portfolio-wide PD multiplier,

each borrower receives an individualized stress adjustment based on their own risk profile.

Borrower vulnerability is calculated using characteristics already present in the PD model.

Example drivers include:

- Loan-to-credit-limit ratio
- Debt-to-income ratio
- Loan amount
- Available credit
- Income verification

Higher-risk borrowers experience larger PD increases under stress.

Lower-risk borrowers experience smaller PD increases.

This preserves borrower-level risk differentiation during scenario analysis.

---

## 5. Scenario Scaling

Each macroeconomic scenario is converted into a normalized stress index.

The stress index is transformed into a PD multiplier using a nonlinear function.

General form:

```
PD Multiplier = exp(k × Stress Index)
```

where:

- *k* is a calibration constant controlling stress severity.
- The nonlinear relationship reflects that default risk tends to accelerate during economic downturns rather than increase proportionally.

The borrower-specific stressed PD is calculated as:

```
Stressed PD =
Baseline PD ×
PD Multiplier ×
Borrower_Risk_Multiplier
```

The resulting stressed PD is capped at 100%.

---

## 6. Exposure at Default (EAD)

Simplified assumption:

```
EAD = Outstanding Loan Balance
```

No assumptions are made regarding:

- future drawdowns
- revolving utilization
- credit conversion factors
- amortization schedules
- prepayments

This simplification is appropriate for illustrating the IFRS 9 framework.

---

## 7. Loss Given Default (LGD)

LGD is assumed constant across all loans.

```
LGD = 45%
```

This assumption is commonly used for unsecured retail portfolios in academic demonstrations.

---

## 8. IFRS 9 Staging

Loans are allocated into IFRS 9 stages using simplified rules.

### Stage 1

Performing loans.

Recognition:

- 12-month ECL

---

### Stage 2

Significant Increase in Credit Risk (SICR).

Recognition:

- Lifetime ECL

---

### Stage 3

Credit-impaired loans.

Recognition:

- Lifetime ECL

---

## 9. Lifetime PD

Stage 1:

```
12-month PD
```

Stage 2 & Stage 3:

Lifetime PD is approximated by scaling the stressed annual PD over the remaining contractual life.

This project does not construct marginal PD term structures or transition matrices.

---

## 10. Expected Credit Loss Formula

Loan-level ECL is calculated as:

```
ECL = PD × LGD × EAD
```

where:

- PD = Scenario-specific borrower probability of default
- LGD = 45%
- EAD = Outstanding balance

---

## 11. Probability-Weighted ECL

Final IFRS 9 ECL is calculated as:

```
Weighted ECL =
0.60 × Baseline
+
0.30 × Downside
+
0.10 × Severe
```

Scenario weights are management assumptions and reflect one possible distribution of future economic outcomes for demonstration purposes.

---

# Model Limitations

This project intentionally simplifies several components of a production IFRS 9 framework.

Specifically:

- Constant LGD assumption
- Simplified EAD assumption
- No behavioural prepayment model
- No revolving credit utilisation model
- No cure or recovery modelling
- No term structure of PD
- Historical stress scenarios used instead of internally forecast macroeconomic scenarios
- Scenario calibration based on historical analogues rather than econometric satellite models

These simplifications allow the project to focus on demonstrating the integration of borrower-level credit risk modelling with forward-looking macroeconomic stress testing.

---

# Future Enhancements

Potential improvements include:

- Satellite models linking macroeconomic variables directly to PD
- Dynamic LGD models incorporating collateral values and recovery rates
- Dynamic EAD models with credit conversion factors
- Transition matrix modelling
- Lifetime marginal PD curves
- Monte Carlo macroeconomic scenario generation
- Machine learning PD models (e.g., XGBoost or LightGBM)
- Model calibration and validation using out-of-time samples
- Full IFRS 9 governance documentation, including model monitoring, back-testing, and validation

---

# Technologies Used

- Python
- pandas
- NumPy
- scikit-learn
- matplotlib
- seaborn
- scipy

---

# Disclaimer

This project is intended for educational and portfolio demonstration purposes only.

It is not a production-ready IFRS 9 implementation and should not be used for financial reporting or regulatory capital calculations without additional model development, validation, governance, and calibration.
