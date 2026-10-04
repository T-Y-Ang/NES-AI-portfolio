### Neurosurgery & Artificial Intelligence Portfolio
I am a clinician currently working in neurosurgery with interests in neuroscience, medical technology and the application of machine learning to clinical problems.
This portfolio documents a series of projects exploring how computational methods can be applied to prediction, medical imaging and physiological data in neurosurgery.
The emphasis is not only on model performance, but on clinical question formulation, methodological validity, interpretability and the limitations involved in translating machine-learning models into clinical practice.

---

## Projects

| Project | Clinical question | Methods | Status |
|---|---|---|---|
| **Traumatic Brain Injury Outcome Prediction** | Can early clinical and imaging information predict 90-day functional outcome after TBI? | Logistic regression, Random Forest, XGBoost, SHAP | **Completed** |
| **Glioma MRI Segmentation** | Can deep learning identify and segment glioma components on multimodal MRI? | Medical image processing, 3D deep learning, segmentation | **In development** |
| **EEG Seizure Detection** | Can seizure activity be detected from EEG recordings using signal processing and machine learning? | Signal processing, machine learning, deep learning | **Planned** |
| **Intracranial Pressure Crisis Forecasting** | Can physiological time-series data provide early warning of intracranial hypertension? | Time-series analysis, machine learning, deep learning | **Future project** |

---

## Project 1 — Traumatic Brain Injury Outcome Prediction

**Status: Completed**

A prognostic machine-learning study using the multinational GNS-I traumatic
brain injury cohort.

The project examines whether information available during early assessment can
predict unfavourable functional outcome at 90 days, and whether structured
imaging findings provide additional prognostic information beyond clinical
assessment alone.

### Key findings

- Early clinical information provided useful discrimination for 90-day
  functional outcome.
- Random Forest and XGBoost demonstrated higher discrimination than the
  logistic-regression baseline.
- The clinical + imaging Random Forest achieved a held-out ROC-AUC of **0.854**,
  average precision of **0.706**, and Brier score of **0.114**.
- Adding imaging information produced relatively modest improvements in
  discrimination.
- Glasgow Coma Scale, pupil reactivity and age were the most influential
  predictors in the interpreted Random Forest model.

**[View the TBI project showcase →](projects/tbi-outcome-prediction.md)**

---

## Portfolio approach

These projects are developed around clinically relevant questions rather than
algorithm selection alone.

Across the portfolio, I aim to consider:

- appropriate clinical question formulation;
- data quality and missing information;
- prevention of information leakage;
- comparison with interpretable baseline models;
- discrimination and calibration;
- model interpretability;
- clinical and methodological limitations; and
- the distinction between internal model performance and clinical
  generalisability.

The projects are intended as educational and research work and should not be
interpreted as clinically validated decision-support tools.

---

## Current development

The next project focuses on **glioma segmentation from MRI**, extending the
portfolio from structured clinical prediction into medical imaging and deep
learning.

This will explore preprocessing of multimodal MRI, three-dimensional tumour
segmentation, quantitative evaluation and visualisation of predicted tumour
regions.

Future projects will extend the portfolio into EEG signal analysis and
physiological time-series modelling.
