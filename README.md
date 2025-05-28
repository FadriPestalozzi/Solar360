# Solar360: An Integrated AI Platform for Solar Forecasting, Panel Health & Public Sentiment

A Holistic Solar-AI Platform to Leverage Forecasting, Panel Health & Public
Sentiment.

## Table of Contents

- [Executive Summary](#executive-summary)
- [Public Datasets](#public-datasets)
- [Machine-Learning Stack](#machine-learning-stack)
- [Workflow & Integration](#workflow--integration)
- [Expected Deliverables (2 Weeks)](#expected-deliverables-2-weeks)
- [Alignment with OpenClimateFix](#alignment-with-openclimatefix)
- [Why Solar360 Matters](#why-solar360-matters)

## Executive Summary

**Solar360** unifies three machine‑learning pipelines into one practical
solution for accelerating solar‑power adoption:

1. **Satellite‑based PV generation forecasting** – predicts the next few hours
   of solar output from Meteosat/GOES imagery.
2. **Automated solar‑panel defect detection** – spots cracks & hotspots in EL /
   IR images so operators can fix under‑performing modules.
3. **Climate‑discourse sentiment analysis** – gauges public support or
   scepticism around solar & climate topics on social media.

By joining technical, operational **and** societal signals, Solar360 helps grid
operators schedule renewables more accurately, maintenance teams maximise
output, and communicators address public concerns. The project is scoped for a
two‑week bootcamp sprint (3 people) and builds directly on
[Open Climate Fix’s](https://github.com/openclimatefix) open‑source tool‑chain.

---

## Public Datasets

| Component          | Dataset                                                                          | Format                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------ | -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PV forecasting** | OCF **nowcasting_dataset** – 1 311 UK PV systems & matched Meteosat images       | `zarr` / NetCDF / CSV  →  [https://github.com/openclimatefix/nowcasting_dataset](https://github.com/openclimatefix/nowcasting_dataset) ([github.com](https://github.com/openclimatefix/nowcasting_dataset?utm_source=chatgpt.com))                                                                                                                                                                          |
|                    | OCF **predict_pv_yield** experiments & **SatFlow** model code (optical‑flow CNN) | Notebooks / PyTorch – [https://github.com/openclimatefix/predict_pv_yield](https://github.com/openclimatefix/predict_pv_yield), [https://github.com/openclimatefix/satflow](https://github.com/openclimatefix/satflow) ([github.com](https://github.com/openclimatefix/predict_pv_yield?utm_source=chatgpt.com), [github.com](https://github.com/openclimatefix/nowcasting_dataset?utm_source=chatgpt.com)) |
| **Panel defects**  | **ELPV Dataset** – 2 624 300×300 px EL images of functional/defective cells      | JPEG/PNG  →  [https://github.com/zae-bayern/elpv-dataset](https://github.com/zae-bayern/elpv-dataset) ([github.com](https://github.com/zae-bayern/elpv-dataset?utm_source=chatgpt.com))                                                                                                                                                                                                                     |
|                    | Alt. Kaggle set: _Defective Solar Cells (EL)_                                    | ZIP images – [https://www.kaggle.com/datasets/philanoe/defective-solar-cells-electroluminescence-images](https://www.kaggle.com/datasets/philanoe/defective-solar-cells-electroluminescence-images) ([kaggle.com](https://www.kaggle.com/datasets/philanoe/defective-solar-cells-electroluminescence-images?utm_source=chatgpt.com))                                                                        |
| **Sentiment**      | **Climate‑Change Twitter Dataset** (Effrosynidis et al.) – 43 k labelled tweets  | CSV  →  [https://www.kaggle.com/datasets/deffro/the-climate-change-twitter-dataset](https://www.kaggle.com/datasets/deffro/the-climate-change-twitter-dataset) ([kaggle.com](https://www.kaggle.com/datasets/deffro/the-climate-change-twitter-dataset?utm_source=chatgpt.com))                                                                                                                             |

All sources are free to download or access via public API, meeting bootcamp
criteria.

---

## Machine‑Learning Stack

| Pipeline    | Core Model                              | Bootcamp Skill Validated     |
| ----------- | --------------------------------------- | ---------------------------- |
| PV forecast | CNN ➜ ConvLSTM (sequence‑to‑vector)     | **Time‑series DL** (RNN/CNN) |
| Panel QC    | Fine‑tuned ResNet‑18 (classification)   | **Image CNN**                |
| Sentiment   | DistilBERT fine‑tuned on climate tweets | **Text RNN/Transformer**     |

We will benchmark simple baselines (persistence, TF‑IDF + logreg, etc.) then
demonstrate DL uplift (lower RMSE, higher F1).

---

## Workflow & Integration

```mermaid
graph TD
  A[Satellite images (t‑5→t)] -->|ConvLSTM| B[PV output forecast t+1→t+4h]
  C[EL / IR panel images] -->|ResNet| D[Defect probability per panel]
  E[Tweets #solar #climate] -->|BERT| F[Sentiment & stance score]
  D --> B
  F --> B
```

- **Defect scores** adjust the effective capacity in the generation forecast
  (e.g. derating a faulty site).
- **Sentiment trends** inform uncertainty or communication plans (e.g.
  anticipating policy shifts, community acceptance).
- Results surface in a lightweight dashboard or notebook for demo.

---

## Expected Deliverables (2 Weeks)

- **3 trained models** with documented code (PyTorch/TensorFlow, scikit‑learn
  baselines).
- **Quantitative metrics:**

  - PV forecast RMSE vs persistence
  - Defect detection accuracy ≥ 90 %
  - Sentiment classification F1 ≥ 85 %

- **Integrated demo** (Streamlit / slides) showing:

  - 4‑hour PV forecast curve vs actuals
  - Annotated panel images highlighting defects
  - Sentiment timeline for “solar panels” keyword

- **README & docs** (this file) released under MIT; can be upstreamed to OCF
  repos.

---

## Alignment with Open Climate Fix

- Builds on OCF’s
  [prediction pipelines](https://github.com/openclimatefix/open-source-quartz-solar-forecast)
  ([github.com](https://github.com/openclimatefix/open-source-quartz-solar-forecast?utm_source=chatgpt.com))
  and data tooling, so results can be **merged back** as pull‑requests.
- Adds **maintenance awareness** (panel health) – a dimension missing in
  existing OCF forecasts, improving accuracy in real deployments.
- Provides **social‑insight layer** vital for scaling solar adoption, echoing
  OCF’s _Always Open_ philosophy and community outreach on
  [https://openclimatefix.org](https://openclimatefix.org)
  ([openclimatefix.org](https://openclimatefix.org/)).

---

## Why Solar360 Matters

- Accurate nowcasts let grid operators cut fossil backup and CO₂ (see National
  Grid ESO quote on OCF site)
  ([openclimatefix.org](https://openclimatefix.org/)).
- Detecting faults early boosts farm output & asset lifetime, avoiding waste and
  unnecessary panel replacements.
- Monitoring public sentiment helps address misinformation, accelerating policy
  and community support for renewables.

With one cohesive story, Solar360 showcases **CNNs, RNNs and NLP** in service of
the energy transition – an ideal capstone for a Le Wagon Data Science cohort and
a springboard to contribute to the real‑world open‑source climate ecosystem.
