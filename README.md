## Enhancing Data Integrity in IoT-Based Hydroponics Using ML-Based Anomaly Detection

> Published in: *2026 International Conference on Information Technology and Computing (ICITCOM)*
> DOI: https://ieeexplore.ieee.org/abstract/document/11714348

---

## Authors & Affiliations

| # | Name | Affiliation |
|---|------|-------------|
| 1 | Steven Andre Gonassis | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |
| 2 | Muhammad Adib Kamali | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |
| 3 | Yohanes Setiawan | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |
| 4 | Reinaldi Santana Battung | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |
| 5 | Michael Angello Qadosy Riyadi | Department of Information Technology, Faculty of Informatics, Telkom University Surabaya Campus, Indonesia |

---

## Background

IoT-based hydroponic systems rely on continuous pH, EC, and temperature sensor readings to drive fuzzy logic actuation for nutrient delivery. Sensor anomalies — particularly sudden spikes and gradual drift — frequently go undetected, causing automated controllers to act on invalid data with potentially irreversible consequences to nutrient chemistry. Existing approaches rely solely on static, single-sensor threshold checks that are fundamentally incapable of capturing gradual drift or cross-node inconsistency, leaving a critical data integrity gap in closed-loop hydroponic control.

---

## Objectives

1. Develop a reproducible, automated two-stage labeling pipeline for unlabeled hydroponic sensor time-series data.
2. Train and compare five ML classifiers to detect sensor anomalies (spikes and drift) with high recall.
3. Select a lightweight model deployable on an ESP32 microcontroller as a pre-filter before the fuzzy logic control layer.
4. Demonstrate substantial improvement over a threshold-only baseline in anomaly recall.

---

## Methods

- **Hardware:** pH, EC, and temperature sensors on two ESP32 nodes transmitting via MQTT to a cloud database. Node 1 = primary (older sensor); Node 2 = validator (newer sensor).
- **Dataset:** 34,906 rows after removing 133 duplicate timestamps from 35,040 raw records; no missing values.
- **Two-Stage Labeling:**
  - *Single-sensor:* Rolling median spike detection (w = 5, threshold = 3σ) and EC threshold bounds (300–2000 ppm).
  - *Cross-sensor:* Node 2 independently pre-screened, then used to flag drift on Node 1 when inter-node differences exceed set thresholds (|ΔpH| > 0.5, |ΔEC| > 150 ppm, |ΔTemp| > 1.5°C).
- **Models:** Decision Tree, Random Forest, Logistic Regression, SVM, KNN — trained on chronological 80:20 split with StandardScaler; tuned via RandomizedSearchCV on F1.
- **Evaluation priority:** Recall (minimizing false negatives) and memory footprint (ESP32 constraint).

---

## Results & Findings

**Pre-tuning vs. Post-tuning (selected model: Decision Tree):**

| Model | Accuracy | Precision | Recall | F1-Score | Size (KB) |
|-------|----------|-----------|--------|----------|-----------|
| Decision Tree (pre) | 0.8625 | 0.8525 | 0.7375 | 0.7908 | 5.2256 |
| **Decision Tree (post)** | **0.8950** | **0.8470** | **0.8570** | **0.8519** | **2.2666** |
| Random Forest (post) | 0.8872 | 0.8478 | 0.8287 | 0.8382 | 891.2793 |
| Logistic Regression (post) | 0.8808 | 0.8503 | 0.8031 | 0.8260 | 0.7471 |
| SVM (post) | 0.8845 | 0.8518 | 0.8139 | 0.8324 | 1047.1650 |
| KNN (post) | 0.8982 | 0.8477 | 0.8671 | 0.8573 | 3747.5762 |

**Threshold-only baseline:** accuracy 0.6539, recall 0.0181, F1 0.0355 — confirming static rules cannot capture drift and spike anomalies.

The Decision Tree was selected for its best combination of recall (0.8570) and memory footprint (2.2666 KB), fitting within ESP32's 520 KB SRAM alongside MQTT stack, sensor drivers, and fuzzy logic tables.

---

## Conclusion

A two-stage ML anomaly detection pipeline combining rolling median spike detection and cross-sensor drift validation was proposed and validated on real hydroponic sensor data. After hyperparameter tuning, the Decision Tree achieved recall of 0.8570 and a 2.2666 KB footprint — the only model both high-recall and feasibly deployable on ESP32. The ML approach dramatically outperformed the threshold-only baseline (recall 0.0181), confirming that static rules are fundamentally inadequate for drift detection. The model was integrated as a validity gate before the fuzzy logic control layer, preventing drift-induced misregulation. Limitations include a single two-node deployment over ten days; future work should conduct closed-loop longitudinal studies measuring impact on dosing accuracy and plant yield.

---

## Citation

### BibTeX — Paper

```bibtex
@inproceedings{gonassis2026enhancing,
  title     = {Enhancing Data Integrity in IoT-Based Hydroponics Using 
               ML-Based Anomaly Detection},
  author    = {Gonassis, Steven Andre and Kamali, Muhammad Adib and 
               Setiawan, Yohanes and Battung, Reinaldi Santana and 
               Riyadi, Michael Angello Qadosy},
  booktitle = {2026 International Conference on Information Technology 
               and Computing (ICITCOM)},
  year      = {2026},
  publisher = {IEEE},
  doi       = {10.1109/ICITCOM.2026.11714348},
  url       = {https://ieeexplore.ieee.org/abstract/document/11714348}
}
```

### BibTeX — GitHub Repository

```bibtex
@misc{gonassis2026enhancing_github,
  author       = {Gonassis, Steven Andre and Kamali, Muhammad Adib and 
                  Setiawan, Yohanes and Battung, Reinaldi Santana and 
                  Riyadi, Michael Angello Qadosy},
  title        = {Enhancing Data Integrity in IoT-Based Hydroponics Using 
                  ML-Based Anomaly Detection},
  year         = {2026},
  publisher    = {GitHub},
  howpublished = {\url{https://github.com/[username]/iot-hydroponic-anomaly-detection-ml-ICITCOM2026}},
  note         = {Source code and dataset for ML-based anomaly detection 
                  in IoT hydroponic sensor systems}
}
```
