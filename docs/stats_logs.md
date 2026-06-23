# Modeling Phase Update – Statistical Baselines and Hybrid Architecture Design

## Objective

Transition from EDA to forecasting by identifying a suitable statistical baseline and validating the need for a residual deep-learning component.

Forecast targets:

* temperature_c
* humidity_pct
* sea_level_pressure_hpa
* wind_speed_ms
* precipitation_mm

Forecast horizon:

* 10 days
* 3-hour resolution
* 80 forecast steps

---

# VARMAX Investigation

Initial architecture:

```text
VARMAX
↓
Residuals
↓
LSTM
↓
Hybrid Forecast
```

Lag-order selection was performed using VAR information criteria.

Results:

| Criterion | Selected Lag |
| --------- | ------------ |
| AIC       | 16           |
| BIC       | 10           |
| HQIC      | 16           |

Selected lag:

```text
p = 10
```

VARMAX fitting was attempted but proved computationally expensive.

Observed behavior:

* Training frequently exceeded 20 minutes.
* Optimization was slow.
* Iterative experimentation became impractical.

Decision:

```text
VARMAX abandoned as primary baseline.
```

Reason:

Computational cost outweighed expected benefits.

---

# VAR Baseline

A VAR(10) model was trained and evaluated.

Positive observations:

* Significant autoregressive coefficients.
* Daily-cycle relationships identified.
* Residual ACF appeared largely flattened.

Residual diagnostics:

Ljung–Box tests remained highly significant.

Conclusion:

```text
VAR captured linear structure but left significant temporal dependence.
```

Forecast evaluation:

Performance was poor across forecast horizons.

Example:

| Variable    | R²    |
| ----------- | ----- |
| Temperature | -3.54 |
| Humidity    | -0.15 |
| Pressure    | -2.56 |
| Wind        | -0.37 |

Decision:

```text
VAR retained as baseline only.
```

---

# Statistical Model Comparison

Three statistical baselines were evaluated:

1. Persistence
2. ETS (Exponential Smoothing)
3. SARIMAX

Evaluation metric:

```text
R²
```

---

# Model Selection Results

Best-performing model per variable:

| Variable               | Selected Model |
| ---------------------- | -------------- |
| temperature_c          | ETS            |
| humidity_pct           | ETS            |
| sea_level_pressure_hpa | Persistence    |
| wind_speed_ms          | SARIMAX        |
| precipitation_mm       | Persistence    |

Observations:

Temperature and humidity benefited from seasonal ETS modeling.

Wind speed benefited from SARIMAX.

Pressure and precipitation were best predicted by persistence.

---

# Residual Diagnostics

Residuals from the selected models were evaluated using the Ljung–Box test.

Results:

| Variable      | Ljung–Box p-value |
| ------------- | ----------------- |
| Temperature   | ≈ 0               |
| Humidity      | ≈ 0               |
| Pressure      | 8.76e−33          |
| Wind          | 1.05e−24          |
| Precipitation | 4.52e−86          |

Interpretation:

All residual series reject the null hypothesis of white noise.

Therefore:

```text
All targets still contain predictable residual structure.
```

---

# Important Finding About Precipitation

Although persistence achieved the best forecast performance for precipitation, residual diagnostics revealed extremely strong remaining temporal dependence.

Conclusion:

```text
Best forecasting model
≠
Best residual-learning model
```

Precipitation remains a valid candidate for residual modeling despite persistence being the strongest statistical baseline.

---

# Final Hybrid Architecture

Selected architecture:

```text
Best Statistical Model Per Variable
                ↓
Residual Extraction
                ↓
Multivariate LSTM
                ↓
Residual Forecast
                ↓
Hybrid Forecast
```

Statistical layer:

* ETS for temperature
* ETS for humidity
* Persistence for pressure
* SARIMAX for wind
* Persistence for precipitation

Residual layer:

* Multivariate LSTM trained on residual series of all five targets.

---

# Current Status

Completed:

* VARMAX investigation
* VAR baseline
* Statistical model comparison
* Forecast evaluation
* Residual extraction
* Residual diagnostics
* Hybrid architecture selection

Next:

```text
04_residual_lstm.ipynb

1. Build residual dataset
2. Scale residuals
3. Create sliding windows
4. Train multivariate LSTM
5. Forecast residuals
6. Build hybrid forecast
7. Compare against statistical baselines
```
