# EEG-TCNet (TCN for EEG) — Quick Config Notes

**Paper:** Ingolfsson et al., *EEG-TCNet: An Accurate Temporal Convolutional Network for Embedded Motor-Imagery Brain–Machine Interfaces* — [arXiv](https://arxiv.org/abs/2006.00622)
**Official code (ETH Zürich):** [github.com/iis-eth-zurich/eeg-tcnet](https://github.com/iis-eth-zurich/eeg-tcnet)

---

## Fixed (one-size) Hyperparameters

| Parameter    | Simple meaning                                                            |
| ------------ | ------------------------------------------------------------------------- |
| **F1 = 8**   | Number of temporal filters in the EEGNet branch’s first convolution.      |
| **F2 = 16**  | Number of pointwise/depthwise-separable filters in the EEGNet branch.     |
| **KE = 32**  | Kernel size (samples) used in the EEGNet branch.                          |
| **KT = 4**   | Temporal kernel size for the TCN module.                                  |
| **L = 2**    | Number of residual TCN blocks (stack depth).                              |
| **FT = 12**  | Number of filters in the initial convolutional layers (paper’s notation). |
| **pe = 0.2** | Dropout rate in the EEGNet branch.                                        |
| **pt = 0.3** | Dropout rate in the TCN module.                                           |

**Legend (parameter meanings):**

* **KT:** Kernel size in TCN module
* **pt:** Dropout rate in TCN module
* **L:** Number of residual blocks
* **FT:** Filters in convolutional layers
* **F1:** Temporal filters in EEGNet
* **KE:** Kernel size in EEGNet
* **pe:** Dropout rate in EEGNet

---

## Training Setup (from the paper)

* **Optimizer:** Adam
* **Learning rate:** 0.001
* **Batch size:** 64
* **Epochs:** 750

---

## Sanity-Check Targets (to verify a re-implementation)

* **Trainable parameters:** ~4,272
* **Compute per inference:** ~6.8 MMACs
* **BCI IV-2a accuracy (fixed model):** ~77.35% (κ ≈ 0.70)
* **With subject-specific tuning:** ~83.84% (≈ +6.49%)

---


