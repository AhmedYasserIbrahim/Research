
This combination helps the model learn new information while keeping what was already useful.  
The architecture uses **four** such residual blocks to build progressively deeper representations.

---

## **Training Details**
- **5 mini-batches**
- **150 epochs**
- **Dropout** used to reduce overfitting  
- **Band-pass filtering** applied to remove noise and keep relevant EEG frequencies  
- **Oversampling** used to balance spike vs non-spike classes  

---

## **Results (Siena Dataset)**

| Patient | Accuracy (%) | Sensitivity (%) | Specificity (%) | FPR | F1-Score (%) |
|--------|---------------|------------------|------------------|------|----------------|
| P00 | 99.35 | 99.90 | 100 | 0.0010 | 99.40 |
| P01 | 100 | 100 | 99.77 | 0.0000 | 99.80 |
| P05 | 99.80 | 99.87 | 99.79 | 0.0025 | 99.75 |
| P06 | 99.92 | 99.88 | 100 | 0.0012 | 99.90 |
| P07 | 100 | 100 | 100 | 0.0000 | 100 |
| P08 | 99.00 | 99.95 | 100 | 0.0020 | 99.25 |
| P11 | 99.95 | 99.90 | 100 | 0.0008 | 99.93 |
| P12 | 99.96 | 100 | 99.95 | 0.0015 | 99.95 |
| P14 | 99.50 | 99.85 | 100 | 0.0022 | 99.45 |
| P17 | 100 | 100 | 100 | 0.0000 | 100 |
| **Average** | **99.75** | **99.94** | **99.95** | **0.0011** | **99.74** |

---

## **Generalizability Concerns**
The paper states:

> “The model was evaluated using the CHB-MIT and Siena datasets, demonstrating its ability to apply between different patient populations and recording environments.”

However, they do **not specify** whether the evaluation was:
- subject-specific,  
- mixed-patient, or  
- leave-one-patient-out (LOPO).

Because this is not described, the reported accuracy **cannot be assumed to reflect true cross-patient generalization**.  
This opens a valuable opportunity to replicate their work and evaluate it under a proper generalized setup, where patients in the test set are unseen during training.

---
