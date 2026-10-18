# CrisisSense AI: Multimodal Disaster Impact Forecasting

Hackathon submission for **AI Innovation Hackathon 2026, Track 1 (AI/ML Methodology), Challenge 01**.
Author: Maham Fatima (individual submission)

## Problem

During floods and severe storms, decision-makers receive fragmented information from weather feeds, sensors and public reports. CrisisSense AI estimates where disruption is likely to become severe before ground conditions fully reveal the impact, and shows the evidence and uncertainty behind each forecast.

## Approach

The system combines two or more data modalities into a spatial grid of cells for one study area:

1. **Feature pipeline:** Rainfall time series, elevation-derived slope and drainage features, and sensor readings are aligned to a common grid and time step.
2. **Anomaly filtering:** An Isolation Forest flags faulty or noisy sensor readings before they reach the model.
3. **Risk model:** A LightGBM regressor predicts a severity score per cell.
4. **Uncertainty:** Quantile regression (10th and 90th percentiles) gives a prediction interval for each cell.
5. **Explanation:** Feature importance shows which signals drive each prediction.

## Data

| Source | Description | Type |
|---|---|---|
| Open-Meteo API | Hourly rainfall for [study area] | Real |
| [Elevation dataset] | Terrain elevation for [study area] | Real |
| Sensor streams | Simulated gauge readings with injected missing values and noise | Synthetic |

Synthetic sensor data is used because real, labelled sensor streams for the study area were not available within the hackathon window. The generation script and parameters are included in `data/`.

## Evaluation

- **Forecast error:** MAE and RMSE on a held-out time period, split chronologically
- **Zone overlap:** Agreement between predicted high-risk cells and labelled high-risk cells
- **Calibration:** Share of true values that fall inside the 80% prediction interval
- **Robustness:** Performance when one data source is removed or noise is added

| Metric | Result |
|---|---|
| MAE | [value] |
| RMSE | [value] |
| High-risk zone overlap | [value] |
| Interval coverage (target ~80%) | [value] |
| Robustness (source removed) | [value] |

## How to Run

1. Open `notebook.ipynb` in Google Colab, or run it locally with Python 3.10+.
2. Install dependencies: `pip install -r requirements.txt`
3. Run all cells in order. Data is fetched or generated automatically.

## Limitations

- The study area covers one region, so results may not transfer elsewhere.
- Sensor data is synthetic. Performance on real sensor networks is untested.
- Risk labels are derived from rainfall thresholds, not from recorded flood impacts.
- Uncertainty intervals are not guaranteed to be well calibrated beyond the test period.
- This is a hackathon research prototype. It is not a production warning system and should not be used for real emergency decisions.

## Ethics and Data Use

Only publicly available and openly licensed data is used. No personal or sensitive information is collected. Dataset limitations and bias are documented above.

## Future Work

Satellite imagery integration, real sensor network data, and a live Streamlit dashboard with streaming updates.
