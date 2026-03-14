# Early Warning Concept — Seismic Hazard Classification

## What This Is
This concept artifact describes how a trained Logistic Regression classifier can power a shift-level hazard warning view. This is not a production-ready system but a thought exercise for a course project.

## What a Safety Operator Would See
For each upcoming shift, an operator would see:
- **Predicted Class**: Hazardous / Non-Hazardous
- **Risk Score**: Probability (0–1) from the model
- **Warning Status**: 
  - 🟢 **Clear** (score < 0.55)
  - 🟡 **Watch** (0.55–0.75)
  - 🔴 **Alert** (score > 0.75)

The threshold of 0.55 was selected on the validation set to maximize the F2 score, prioritizing recall over precision.

## Model Inputs
The classifier uses seismic sensor readings from the previous shift, including:
- Seismic and seismoacoustic activity categories (a/b/c/d)
- Ghazard level
- Shift type (N/W)
- Energy and maximum energy
- Bump counts (nbumps, nbumps2, nbumps3, etc.)

## Trade-off and Scoring
At the chosen threshold, the model identifies 57.7% of truly hazardous shifts. However, it also generates false alarms on roughly 17% of safe shifts (flagged as hazardous).

In safety contexts, over-warning is generally better than under-warning. The F2 scoring metric specifically penalizes missed hazardous shifts more heavily than false alarms.

## Limitations
1. **No timestamps**: We cannot verify if the model generalizes across different time periods.
2. **Class imbalance**: The 14.2:1 imbalance makes rare events inherently difficult to detect.
3. **Model type**: This is a tabular machine learning model, not a physics-based seismic predictor.
4. **Generalization**: The model was trained on historical data from a single mine. Performance in other environments is unknown.
5. **Miss rate**: A 57.7% recall means that roughly 4 out of 10 hazardous shifts are missed.

## Requirements for Real Use
- **Temporal validation**: Testing on recent shifts held out strictly by time.
- **Domain review**: Expert analysis of feature definitions and data quality.
- **Operator protocol**: Clear instructions for actions when an Alert or Warning is triggered.
- **Retraining**: Regular updates to the model as mine conditions evolve.

