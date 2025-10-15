# STAT-443 Consulting Project

## Project Overview
This consulting project is part of **STAT-443 Professional Statistics**.  
The goal is to **fine-tune engineering systems** for an energy provider in Turkey, with a focus on **reducing Nitrogen Oxide (NOx) emissions** from gas turbines while also monitoring Carbon Monoxide (CO).

---

## Client
- **Type**: Energy provider in Turkey  
- **Equipment**: Gas turbines (with sensors installed for emissions and operational data)  
- **Contact**: Engineering Manager  
- **Preferred communication**: Email  

---

## Initial Client Meeting Notes

### Objective
- Main goal: Reduce **NOx emissions** from gas turbine operations.  
- Secondary pollutant: Monitor **CO emissions**.  
- Deliver a **report + documented code** (in R and Python).  
- Include **recommendations under different weather/ambient conditions**.  
- Timeline: Deliverables **by December**.

### Current System & Data
- Equipment: Gas turbines in continuous operation.  
- Sensors: Already measure NOx, CO, pressures, and temperatures.  
- Data: Hourly averages for ~2 years (~14,300 rows).  
- Normal power output range: **130–136 MWh**, with higher loads up to **160 MWh**.  
- Control/engineering variables:  
  - **TIT** (Turbine Inlet Temperature)  
  - **TAT** (Turbine Exhaust Temperature)  
  - **CDP** (Compressor Discharge Pressure)  
  - **GTEP** (Gas Turbine Exhaust Pressure)  
  - **AFDP** (Air Filter Differential Pressure)  
- Ambient variables:  
  - **AT** (Ambient Temperature)  
  - **AP** (Ambient Pressure)  
  - **AH** (Ambient Humidity)

### Processes to Investigate
- Combustion tuning (burner settings, firing temperature).  
- Effect of **ambient conditions** (temperature, pressure, humidity).  
- Possible optimization of NOx–CO trade-off.  
- Load band effects (different operating regimes).  

### Deliverables
- Exploratory Data Analysis (EDA).  
- Modeling (R/Python).  
- Recommendations for reducing NOx emissions without violating CO limits.  
- Client-ready presentation and report.  

---

## Data
**File:** `TurbineGroup2.csv`  

Variables:  
- `AT` : Ambient Temperature (°C)  
- `AP` : Ambient Pressure (mbar)  
- `AH` : Ambient Humidity (%)  
- `AFDP`: Air Filter Differential Pressure  
- `GTEP`: Gas Turbine Exhaust Pressure  
- `TIT` : Turbine Inlet Temperature (°C)  
- `TAT` : Turbine Exhaust Temperature (°C)  
- `TEY` : Turbine Energy Yield (MW/MWh)  
- `CDP` : Compressor Discharge Pressure (bar)  
- `CO`  : Carbon Monoxide emissions (ppm or mg/Nm³)  
- `NOX` : Nitrogen Oxide emissions (ppm or mg/Nm³, **main target**)  

---

## Methods Used So Far

### 1. Exploratory Data Analysis (EDA)
- **Histograms**: Examined variable distributions.  
  - AT: 0–35°C, uniform.  
  - AP: ~1010 mbar, bell-shaped.  
  - AH: Skewed high (>70%).  
  - TIT: Many runs near **1100°C limit**.  
  - TAT: Very tight at ~550°C.  
  - TEY: Distinct load bands (120–160 MW).  
  - CDP, GTEP, AFDP: Multi-modal (different operating regimes).  
  - CO: Mostly near 0, spikes occasionally.  
  - NOX: Bell-shaped, ~60–80 ppm.  

- **Correlations**:  
  - NOx strongly linked to TIT, CDP, TEY.  
  - CO negatively correlated with NOx (expected combustion trade-off).  
  - Ambient humidity (AH) reduces NOx slightly.  

- **Scatterplots**:  
  - NOx rises with TIT.  
  - NOx higher at certain load bands.  
  - CO and NOx show inverse pattern.  

- **Load Segmentation**:  
  - Created bins by TEY (≤136, 136–160, >160).  
  - NOx increases with higher load band.  

### 2. Baseline Models
- **Linear Regression**: For interpretability, to see effect of each variable.  
- **Random Forest Regressor**: To capture non-linear relationships.  
- **Feature Importance Findings**: TIT, CDP, TEY were most important predictors of NOx.  

### 3. Cleaning
- Outlier checks using z-scores.  
- Removal of extreme CO spikes and TIT pushed beyond physical range.  

---

## What Still Needs to Be Done

1. **Feature Engineering**  
   - Temperature deltas (TIT–TAT, TIT–TK2).  
   - Interaction terms (e.g., CDP × TIT).  
   - Ambient bins (low/high humidity).  

2. **Modeling for Recommendations**  
   - Scenario testing (what happens if TIT lowered by 5°C?).  
   - Balance CO vs NOx using multi-output regression.  
   - Time-of-day / seasonal analysis (ambient effect).  

3. **Deliverables**  
   - Full report with EDA, models, insights.  
   - Code in **R and Python**.  
   - Clear client recommendations:  
     - Which operating ranges produce high NOx.  
     - How ambient weather shifts emissions.  
     - What turbine adjustments reduce NOx while keeping CO acceptable.  

---

## Requirements Recap (from Client)
- **By December**: Documented code + report.  
- Answer: “What should be done under different ambient conditions?”  
- Present results to engineering manager.  
- Provide general overview before deep technical dive.  

---

## Repository Structure