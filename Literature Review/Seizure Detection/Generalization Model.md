# 📄 Paper Summary 2  
## **Comparison of Patient Non-Specific Seizure Detection Using Multi-Modal Signals**  
**Sigsgaard et al., Biomedical Signal Processing and Control, March 2024**  
🔗 https://www.sciencedirect.com/science/article/pii/S2772528623000377

---

## **Overview**
This paper focuses on building a **generalized (patient-non-specific)** seizure detection model using the Siena Scalp EEG Database.  
A key insight emphasized in the paper is that Siena includes **both EEG and ECG recordings**, making it possible to combine brain activity and heart-related changes for improved generalizability.

The authors evaluate EEG-only, ECG-only, and multi-modal detection using a rigorous **Leave-One-Patient-Out (LOPO)** validation setup.

---

## **EEG-Based Detection**
**Preprocessing & Windowing**
- High-pass filtering between **0.5–50 Hz**  
- Data split into **6-second windows**  
- **50% overlap** applied only for seizure windows to increase their representation  
- A window is labeled as *seizure* **only if it fully falls inside** the seizure interval (minimizes false positives)

**Feature Extraction & Classification**
- Temporal and frequency-based features extracted from each 6s window  
- Features fed into a **Random Forest** classifier

This setup focuses on capturing sharp EEG changes that characterize seizure activity while keeping non-seizure labels clean.

---

## **ECG-Based Detection**
**Preprocessing & Windowing**
- ECG processed using the standard **60-second windows**  
- **50% overlap** on seizure windows  
- Any overlap with a seizure segment is labeled as seizure  

**Handling Imbalance**
- The non-seizure class is **downsampled**  
- Achieves an approximate **seizure:non-seizure ratio of 9:1**

**Classification**
- Feature extraction → Random Forest classifier, similar to EEG

This approach leverages heart-rate and rhythm changes that often accompany seizures.

---

## **Multi-Modal Combination**
The paper evaluates two integration strategies:

1. **Feature-Level Fusion**  
   - EEG features + ECG features merged into one long feature vector  
   - Classified using a single model

2. **Decision-Level Fusion (Boolean OR)**  
   - Independent EEG and ECG models  
   - Final prediction = seizure if **either** model says seizure  

**Findings:**  
Decision-level fusion outperforms feature-level fusion because EEG and ECG changes **do not always occur at the same time**, so fusing predictions captures more true seizure moments.

---

## **Results (Leave-One-Patient-Out on Siena)**
Performance on each unseen patient (LOPO setup):

| PID | Sensitivity EEG (%) | Sensitivity ECG (%) | Sensitivity Multi (%) | Specificity EEG (%) | Specificity ECG (%) | Specificity Multi (%) | FP/h EEG | FP/h ECG | FP/h Multi | DL EEG (sec) | DL ECG (sec) | DL Multi (sec) |
|-----|----------------------|----------------------|------------------------|----------------------|----------------------|------------------------|----------|----------|------------|--------------|---------------|----------------|
| PN00 | 100 | 0 | 100 | 99.95 | 100 | 99.95 | 0.31 | 0 | 0.31 | 32.8 | NSD | 32.8 |
| PN01 | 100 | 100 | 100 | 100 | 99.57 | 99.57 | 0 | 2.6 | 2.6 | 10.5 | 34.5 | 10.5 |
| PN03 | 100 | 0 | 100 | 99.69 | 99.81 | 99.5 | 1.86 | 1.16 | 2.97 | 22 | NSD | 22 |
| PN05 | 66.67 | 0 | 66.67 | 98.75 | 99.21 | 98.01 | 7.46 | 4.64 | 11.77 | 14.5 | NSD | 14.5 |
| PN06 | 60 | 80 | 80 | 99.89 | 98.67 | 98.56 | 0.66 | 7.88 | 8.55 | 22 | 28.2 | 22.2 |
| PN07 | 100 | 0 | 100 | 97.32 | 99.61 | 97.01 | 16.03 | 1.83 | 17.86 | 9 | NSD | 9 |
| PN09 | 33 | 100 | 100 | 100 | 97.79 | 97.79 | 0 | 13.02 | 13.02 | 41 | 37 | 35 |
| PN10 | 30 | 10 | 30 | 99.88 | 99.52 | 99.41 | 0.7 | 2.83 | 3.48 | 12.3 | 3 | 8.3 |
| PN11 | 100 | 0 | 100 | 98.82 | 99.86 | 98.67 | 7.03 | 0.83 | 7.86 | 6 | NSD | 6 |
| PN12 | 100 | 25 | 100 | 99.5 | 98.55 | 98.05 | 2.95 | 8.52 | 11.48 | 12.5 | 46 | 12.5 |
| PN13 | 100 | 66.67 | 100 | 99.81 | 99.92 | 99.73 | 1.15 | 0.46 | 1.62 | 20 | 57 | 20 |
| PN14 | 50 | 0 | 50 | 98.69 | 99.75 | 98.47 | 6.86 | 1.32 | 7.97 | 18 | NSD | 18 |
| PN16 | 100 | 100 | 100 | 99.1 | 99.93 | 99.02 | 5.32 | 0.41 | 5.73 | 38 | 44 | 38 |
| PN17 | 100 | 100 | 100 | 99.97 | 100 | 99.97 | 0.19 | 0 | 0.19 | 21.5 | 42.5 | 21.5 |

**Mean Performance Across Patients:**
- **Sensitivity:**  
  - EEG: **81.43%**  
  - ECG: **41.55%**  
  - Multi-modal: **87.62%**
- **Specificity:** ~99% across all modalities  
- **FP/h:**  
  - EEG: **3.61**  
  - ECG: **3.25**  
  - Multi: **6.82**
- **Detection Latency (sec):**  
  - EEG: **20**  
  - ECG: **36.5**  
  - Multi: **19.3**

---

## **Key Takeaways**
- EEG alone provides strong sensitivity but may miss some physiological markers  
- ECG alone is inconsistent but complementary  
- **Decision-level fusion (Boolean OR)** gives the best generalization across patients  
- The LOPO evaluation confirms that this approach aims for **true cross-patient generalization**, unlike many EEG studies that train and test on the same subjects

---
