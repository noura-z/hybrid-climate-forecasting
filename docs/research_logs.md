### Temperature Analysis

Observations:

- Clear annual seasonal pattern.
- Higher temperatures during summer months.
- Lower temperatures during winter months.
- No obvious long-term warming or cooling trend.
- Variance appears relatively stable across the study period.
- Several extreme temperature spikes likely correspond to heat-wave events.
- A few near-zero observations require validation.

### Humidity Analysis

Observations:

- Humidity values are generally high, consistent with a coastal climate.
- Most observations fall between 60% and 100%.
- Humidity exhibits substantial short-term variability.
- Possible seasonal behavior is visible, but less clearly than temperature.
- No obvious long-term trend can be concluded from the raw plot alone.
- Several low-humidity events should be investigated further.

### Sea Level Pressure Analysis

Observations:

- A seasonal pattern is visible.
- Pressure tends to be higher during winter and lower during summer.
- The series is smoother than humidity and exhibits clear temporal structure.
- Several short-lived pressure drops and peaks are present, likely corresponding to weather systems.
- No obvious long-term trend is visible.
- The variable appears suitable for time-series forecasting.

### Wind Speed Analysis

Observations:

- No strong annual seasonal pattern is visible.
- Wind speed exhibits substantial short-term variability.
- Most observations correspond to relatively low wind speeds.
- Occasional spikes indicate stronger wind events.
- Numerous zero-wind observations are present and should be investigated.
- Wind speed appears more stochastic than temperature and pressure.


### Precipitation Analysis

Observations:

- Precipitation is highly sparse.
- Approximately 13.2% of observations contain rainfall.
- Rainfall events are concentrated during wetter seasons.
- Long dry periods are visible, especially during summer.
- Several extreme rainfall events are present.
- The distribution appears highly right-skewed.
- Precipitation is expected to be the most challenging forecasting target.

# Exploratory Data Analysis

## Overview

An exploratory analysis was conducted to understand the temporal behavior, statistical properties, and forecasting potential of the selected climate variables:

* temperature_c
* humidity_pct
* sea_level_pressure_hpa
* wind_speed_ms
* precipitation_mm

The analysis included visualization, correlation analysis, stationarity testing, temporal dependence analysis, frequency-domain analysis, wavelet decomposition, and outlier inspection.

---

## Time-Series Analysis

Visual inspection revealed clear seasonal and temporal structures within the climate variables.

### Temperature

* Strong annual seasonality was observed.
* Summer periods exhibited higher temperatures while winter periods showed lower values.
* No major long-term trend was detected.

### Humidity

* High variability was observed.
* Humidity generally remained between 60% and 100%.
* Seasonal behavior appeared inversely related to temperature.

### Sea-Level Pressure

* Pressure exhibited smooth long-term evolution.
* Higher values generally occurred during winter periods.

### Wind Speed

* Wind speed showed moderate variability.
* No strong annual seasonal pattern was observed.

### Precipitation

* Rainfall was highly sparse and event-driven.
* Approximately 13.2% of observations contained precipitation.

---

## Correlation Analysis

The correlation matrix highlighted several physically meaningful relationships.

Key observations:

* Temperature and dew point showed strong positive correlation.
* Temperature and humidity showed strong negative correlation.
* Temperature and pressure exhibited moderate negative correlation.
* Humidity and wind speed exhibited moderate negative correlation.
* Station pressure and sea-level pressure were nearly perfectly correlated, indicating redundancy.

Precipitation displayed weak linear correlations with most variables, suggesting a more complex and nonlinear behavior.

---

## Stationarity Analysis

Stationarity was assessed using both Augmented Dickey-Fuller (ADF) and KPSS tests.

### Findings

* Sea-level pressure and precipitation satisfied both stationarity criteria.
* Temperature, humidity, and wind speed showed disagreement between ADF and KPSS, suggesting seasonal effects rather than random-walk behavior.
* No variable exhibited characteristics of a pure random walk process.

---

## Temporal Dependence Analysis

Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) were used to examine temporal memory.

### Temperature

* Strong persistence across multiple lags.
* Clear cyclical behavior associated with daily atmospheric processes.

### Humidity

* Significant temporal dependence and periodic structure.
* Similar behavior to temperature.

### Sea-Level Pressure

* Strong long-memory behavior.
* Slow decay of autocorrelation.

### Wind Speed

* Moderate persistence.
* Combination of structured and stochastic behavior.

### Precipitation

* Weak temporal dependence.
* Event-driven dynamics with limited persistence.

These findings support the use of autoregressive forecasting models.

---

## Fourier Transform Analysis

FFT was applied to identify dominant periodicities.

### Findings

Temperature:

* Strong 24-hour cycle.
* Significant harmonics at approximately 12 and 8 hours.

Humidity:

* Similar dominant frequencies to temperature.

Sea-Level Pressure:

* Dominated by low-frequency components.
* Slow atmospheric evolution.

Wind Speed:

* Weaker daily periodicity.

Precipitation:

* No dominant periodic structure.
* Largely event-driven behavior.

FFT confirmed the existence of strong daily cycles in temperature and humidity while highlighting the irregular nature of precipitation.

---

## Wavelet Analysis

A level-5 Discrete Wavelet Transform (DWT) was applied to analyze climate signals at multiple temporal scales.

Approximate scales:

| Component | Time Scale  |
| --------- | ----------- |
| D1        | 3–6 hours   |
| D2        | 6–12 hours  |
| D3        | 12–24 hours |
| D4        | 24–48 hours |
| D5        | 48–96 hours |
| A5        | >96 hours   |

### Findings

Temperature:

* Long-term seasonal behavior concentrated in A5.
* Daily-scale dynamics primarily captured in D3.

Humidity:

* Similar decomposition structure to temperature.

Sea-Level Pressure:

* Dominated by low-frequency behavior.
* Most information concentrated in A5.

Wind Speed:

* Energy distributed across both low and high-frequency components.

Precipitation:

* Sparse and localized events.
* Wavelet decomposition clearly highlighted the intermittent nature of rainfall.

Wavelet decomposition provided a multi-scale representation of climate dynamics and complemented the FFT analysis.

---

## Outlier Analysis

Boxplot analysis was conducted for all forecast targets.

Findings:

* Temperature extremes corresponded to realistic heat-wave events.
* Low humidity observations were physically plausible.
* Pressure extremes reflected strong atmospheric systems.
* Wind speed extremes were consistent with strong wind events.
* Extreme precipitation values likely represented genuine storm events.

No observations were removed since all detected outliers were considered meteorologically plausible.

---

## EDA Conclusion

The dataset exhibits:

* Strong temporal dependence.
* Clear seasonal and daily cycles.
* Physically meaningful inter-variable relationships.
* Multi-scale climate dynamics.
* Forecastable structure suitable for time-series modeling.

The analysis supports the use of a hybrid forecasting framework combining statistical and deep learning approaches.


# 3. Stationarity and Temporal Dependence

## 3.1 ADF and KPSS Tests
### Stationarity Analysis

ADF tests reject the null hypothesis of a unit root for all forecast targets.

KPSS tests indicate that temperature, humidity, and wind speed may exhibit non-stationary behavior due to seasonality or structural patterns.

Sea-level pressure and precipitation satisfy both stationarity criteria and appear suitable for direct VARMAX modeling.

The disagreement between ADF and KPSS for temperature, humidity, and wind speed is likely caused by seasonal effects observed during exploratory analysis.

## 3.2 Autocorrelation Analysis (ACF) & Partial Autocorrelation Analysis (PACF)
### Autocorrelation and Partial Autocorrelation Analysis

Autocorrelation Function (ACF) and Partial Autocorrelation Function (PACF) analyses were performed to evaluate temporal dependence and identify the memory structure of each climate variable.

#### Temperature

- The ACF exhibited strong persistence and a clear oscillatory pattern, indicating the presence of seasonal and daily cycles.
- Significant autocorrelation remained across many lags, suggesting that past temperature observations strongly influence future values.
- The PACF showed a strong contribution from the first few lags, after which the influence rapidly decreased.
- These results indicate that temperature possesses strong temporal structure and is highly predictable.

#### Humidity

- The ACF revealed periodic behavior similar to temperature, reflecting daily atmospheric cycles.
- Significant autocorrelation was observed across multiple lags, indicating temporal dependence.
- The PACF showed that most predictive information is concentrated in recent observations.
- Humidity is expected to be moderately predictable due to its structured temporal behavior.

#### Sea-Level Pressure

- The ACF decayed slowly and remained positive over a large number of lags, indicating long-term persistence.
- This behavior suggests that pressure evolves gradually over time and is influenced by slowly changing weather systems.
- The PACF displayed strong significance in the first few lags followed by a rapid decline.
- Sea-level pressure exhibited the strongest autoregressive structure among all studied variables.

#### Wind Speed

- The ACF showed weaker but still noticeable periodic patterns.
- Wind speed contained both persistent and stochastic components.
- The PACF indicated that most useful information is contained within the first few lags.
- Wind speed is expected to be more difficult to forecast than temperature or pressure due to its higher variability.

#### Precipitation

- The ACF decayed rapidly, indicating limited temporal persistence.
- Most autocorrelation values became weak after the first few lags.
- The PACF also showed significant influence only for very short lag intervals.
- These results confirm that precipitation behaves as an intermittent and event-driven process with weaker temporal structure than the other variables.

### Overall Findings

- All variables exhibited statistically significant temporal dependence.
- Temperature and humidity displayed strong cyclical behavior associated with daily atmospheric processes.
- Sea-level pressure showed the strongest long-term persistence.
- Wind speed demonstrated moderate predictability with a mixture of structured and random behavior.
- Precipitation exhibited the weakest temporal dependence and is therefore expected to be the most challenging forecasting target.

### Fourier Transform Analysis

The Fast Fourier Transform (FFT) was applied to identify dominant periodicities in the climate variables.

Key findings:

- Temperature exhibited strong peaks corresponding to approximately 24-hour, 12-hour, and 8-hour cycles, indicating pronounced daily periodic behavior.
- Humidity showed frequency peaks similar to temperature, suggesting a strong coupling with the daily temperature cycle.
- Sea-level pressure was dominated by low-frequency components, reflecting slowly evolving atmospheric systems.
- Wind speed displayed weaker daily periodicity and a greater contribution from irregular fluctuations.
- Precipitation exhibited no dominant periodic structure, indicating that rainfall events are largely intermittent and event-driven.

Overall, FFT confirmed that temperature and humidity contain strong cyclic patterns, whereas precipitation is considerably less periodic.


### Wavelet Decomposition Analysis

Wavelet decomposition was performed using a level-5 Discrete Wavelet Transform (DWT) to analyze climate variables across multiple temporal scales.

Approximate temporal scales:

| Component | Time Scale |
|------------|------------|
| D1 | 3–6 hours |
| D2 | 6–12 hours |
| D3 | 12–24 hours |
| D4 | 24–48 hours |
| D5 | 48–96 hours |
| A5 | >96 hours |

Key findings:

- Temperature exhibited clear long-term structure in A5 and strong daily-scale behavior in D3.
- Humidity showed decomposition patterns similar to temperature, confirming the presence of daily-scale variability.
- Sea-level pressure was dominated by low-frequency components (A5), indicating that pressure evolves primarily over long time scales.
- Wind speed contained energy across both long-term and short-term components, reflecting a combination of gradual atmospheric changes and local fluctuations.
- Precipitation was characterized by sparse and localized spikes across multiple decomposition levels, highlighting its intermittent and event-driven nature.

Wavelet decomposition provided a multi-scale representation of the climate signals and revealed temporal structures that are not directly visible through Fourier analysis alone.
