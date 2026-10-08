# Brisbane Water Quality: pH Time Series Forecasting

**Sequential Learning for Environmental Time Series Analysis**

---

## Project Overview

Water quality monitoring is critical for coastal and subtropical cities like Brisbane, Australia, where environmental, climatic, and anthropogenic factors heavily influence aquatic ecosystems.

This project focuses on modeling and forecasting the temporal dynamics of water quality parameters, with a primary emphasis on **pH** as the target variable. Since pH directly impacts aquatic ecosystem health, nutrient solubility, and water treatment efficiency, accurate predictions are essential for early anomaly detection and sustainable water management.

### Core Problem Statement
**How can we accurately predict future pH levels by leveraging historical time series data and multivariate exogenous environmental factors?**

---

## Dataset Summary

* **Source:** Brisbane Water Quality Monitoring System (`brisbane_water_quality.csv`).
* **Time Horizon:** ~11 months of historical observations (~30,000 continuous records).
* **Target Variable:** `pH` (Slightly basic, highly stable time series with a mean $\approx 8.06$).
* **Exogenous Features:** 
  * `Average Water Speed` & `Average Water Direction`
  * `Chlorophyll` (Indicator of algal growth & potential eutrophication)
  * `Temperature` (Subtropical climate variations, mean $\approx 24.4^\circ\text{C}$)
  * `Dissolved Oxygen`
  * `Salinity` & `Specific Conductance` (Estuarine freshwater/saltwater mixing dynamics)
  * `Turbidity` (Spikes related to rainfall/runoff events)

---

## Data Preprocessing & Strategy

To ensure optimal performance for sequential learning models (e.g., Recurrent Neural Networks / LSTMs), a rigorous data cleaning pipeline was implemented:

1. **Feature Pruning:** Removed non-informative, constant, and redundant quality flags (`[quality]` columns).
2. **Chronological Sorting:** Sorted entries strictly by `Timestamp`.
3. **Deduplication:** Resolved duplicate timestamps by retaining the most recent observation.
4. **Regular Resampling:** Fixed time steps to a **30-minute frequency** (`asfreq("30min")`), creating a uniform time grid.
5. **Missing Value Imputation:** Handled continuous gaps via **linear interpolation** to maintain sequence integrity.
6. **Feature Engineering:** Extracted temporal indicators (`Hour`, `Day of Week`, `Month`, `Year`).

---

## Key Exploratory Findings (Correlations with pH)

| Feature | Correlation with pH | Physical / Ecological Interpretation |
| :--- | :---: | :--- |
| **Temperature** | **-0.73** | Strong negative relationship; warmer water holds less dissolved $\text{CO}_2$, shifting acid-base equilibrium. |
| **Dissolved Oxygen** | **+0.66** | Strong positive correlation; higher pH often aligns with photosynthetic activity and healthier oxygenation. |
| **Turbidity** | **-0.39** | Moderate negative correlation; elevated turbidity indicates runoff or pollution events lowering pH. |
| **Average Water Speed** | **-0.37** | Hydrodynamic mixing influences chemical dispersion. |
| **Salinity** | **-0.31** | Reflects estuarine dynamics where freshwater/seawater ratios alter chemical balance. |
| **Chlorophyll** | **+0.26** | Photosynthetic $\text{CO}_2$ consumption by algae causes slight pH increases. |

---

## Tech Stack

* **Language:** Python 3.x
* **Data Processing & Analysis:** `pandas`, `numpy`
* **Deep Learning Framework:** `tensorflow` / `keras`
* **Visualization:** `matplotlib`, `seaborn`
