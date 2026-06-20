# Data Contract

## Project

**Title:** Hybrid Statistical and Deep Learning Framework for Multi-Target Climate Forecasting

**Objective:**
Develop a reproducible climate forecasting system capable of predicting multiple meteorological variables over a 10-day horizon using a hybrid architecture combining statistical forecasting models and deep learning models.

---

# Dataset Information

## Source

* Source: MASCIR Meteorological Station (Rabat Airport)
* Location: Rabat, Morocco
* Dataset Type: Historical Weather Observations
* Collection Method: Automated Weather Station

## Storage Location

```text
data/raw/All data.xlsx
```

## Data Format

* Format: Excel (.xlsx)
* Observation Frequency: 3-hour intervals
* Approximate Records: 9,392 observations

---

# Forecasting Scope

## Forecast Targets

The system will forecast the following meteorological variables:

| Variable               | Description          | Unit |
| ---------------------- | -------------------- | ---- |
| temperature_c          | Air Temperature      | °C   |
| humidity_pct           | Relative Humidity    | %    |
| sea_level_pressure_hpa | Atmospheric Pressure | hPa  |
| wind_speed_ms          | Wind Speed           | m/s  |
| precipitation_mm       | Precipitation Amount | mm   |

## Forecast Horizon

* Horizon: 10 days
* Frequency: 3-hour intervals
* Future Steps:

```text
10 days × 8 observations/day = 80 forecast steps
```

---

# Column Dictionary

| Original Column               | Standardized Name      | Description                             | Unit             |
| ----------------------------- | ---------------------- | --------------------------------------- | ---------------- |
| Local time in Rabat (airport) | timestamp              | Observation timestamp                   | datetime         |
| T                             | temperature_c          | Air temperature                         | °C               |
| Po                            | station_pressure_hpa   | Station atmospheric pressure            | hPa              |
| P                             | sea_level_pressure_hpa | Sea-level atmospheric pressure          | hPa              |
| Pa                            | pressure_change_hpa    | Pressure tendency over previous 3 hours | hPa              |
| U                             | humidity_pct           | Relative humidity                       | %                |
| DD                            | wind_direction         | Wind direction                          | degrees/cardinal |
| Ff                            | wind_speed_ms          | Average wind speed                      | m/s              |
| ff10                          | wind_gust_10min_ms     | Maximum wind gust over 10 minutes       | m/s              |
| ff3                           | wind_gust_3sec_ms      | Maximum wind gust over 3 seconds        | m/s              |
| N                             | cloud_cover            | Total cloud cover                       | oktas            |
| WW                            | present_weather_code   | Current weather condition code          | WMO code         |
| W1                            | past_weather_code_1    | Past weather code                       | WMO code         |
| W2                            | past_weather_code_2    | Past weather code                       | WMO code         |
| Tn                            | min_temperature_c      | Minimum observed temperature            | °C               |
| Tx                            | max_temperature_c      | Maximum observed temperature            | °C               |
| Cl                            | low_cloud_type         | Low cloud classification                | WMO code         |
| Nh                            | cloud_amount           | Cloud amount                            | oktas            |
| H                             | cloud_base_height      | Height of cloud base                    | m                |
| Cm                            | middle_cloud_type      | Middle cloud classification             | WMO code         |
| Ch                            | high_cloud_type        | High cloud classification               | WMO code         |
| VV                            | visibility             | Horizontal visibility                   | m/km             |
| Td                            | dew_point_c            | Dew point temperature                   | °C               |
| RRR                           | precipitation_mm       | Precipitation amount                    | mm               |
| tR                            | precipitation_period_h | Duration of precipitation measurement   | hours            |
| E                             | ground_condition       | Ground surface condition                | WMO code         |
| Tg                            | ground_temperature_c   | Ground temperature                      | °C               |
| E'                            | ground_state           | Ground state classification             | WMO code         |
| sss                           | snow_depth             | Snow depth or snow cover indicator      | cm               |

---

# Feature Classification

## Forecast Targets

```text
temperature_c
humidity_pct
sea_level_pressure_hpa
wind_speed_ms
precipitation_mm
```

## Numerical Predictors

```text
station_pressure_hpa
pressure_change_hpa
wind_gust_10min_ms
wind_gust_3sec_ms
min_temperature_c
max_temperature_c
cloud_amount
cloud_base_height
visibility
dew_point_c
ground_temperature_c
```

## Categorical Predictors

```text
wind_direction
present_weather_code
past_weather_code_1
past_weather_code_2
low_cloud_type
middle_cloud_type
high_cloud_type
ground_condition
ground_state
```

---

# Data Quality Rules

## Timestamp

* Must be unique.
* Must be sorted chronologically.
* Missing timestamps must be detected and reported.

## Numerical Features

* Stored as numeric values.
* Invalid textual values must be cleaned.
* Missing values must be documented and imputed when appropriate.

## Precipitation

Examples such as:

```text
No precipitation
```

must be converted to:

```text
0.0
```

before modeling.

---

# Modeling Constraints

## Validation Strategy

The project will use:

* Expanding Window Validation
* Walk-Forward Validation

The following methods are prohibited:

* Random train/test split
* Standard k-fold cross validation

to prevent temporal data leakage.

## Baselines

All advanced models must be compared against:

* Persistence Forecast
* Seasonal Naive Forecast
* ETS
* VARMAX

before being considered successful.

---

# Planned Architecture

```text
Raw Data
    ↓
Validation Layer
    ↓
Feature Store
    ↓
Wavelet Decomposition
    ↓
VARMAX
    ↓
Residual Extraction
    ↓
Multi-Output LSTM
    ↓
Ensemble Layer
    ↓
Forecast
    ↓
SHAP Explainability
```

---

# Version

Version: 1.0

Last Updated: Initial Project Setup
