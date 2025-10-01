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

# S4 (Structured State Spaces) — Quick Config Notes

**Paper:** Gu, Goel, Ré, *Efficiently Modeling Long Sequences with Structured State Spaces (S4)* — [arXiv](https://arxiv.org/abs/2111.00396). ([arXiv][1])
**Official code (Hazy Research):** [github.com/state-spaces/s4](https://github.com/state-spaces/s4). ([GitHub][2])

---

## Fixed (one-size) Hyperparameters

| Parameter                  | Simple meaning                                                                                            |
| -------------------------- | --------------------------------------------------------------------------------------------------------- |
| **`model.n_layers = 3`**   | Number of stacked S4 blocks (depth). ([GitHub][2])                                                        |
| **`model.d_model = 128`**  | Hidden width (features) of each block. ([GitHub][2])                                                      |
| **`model.norm = batch`**   | Normalization type inside blocks. ([GitHub][2])                                                           |
| **`model.prenorm = True`** | Apply normalization before the block. ([GitHub][2])                                                       |
| **`ssm.l_max = 8192`**     | Example receptive-field length used in a released WT103 config/checkpoint (task-dependent). ([GitHub][3]) |
| **Optimizer param groups** | Use **lower LR** and **0 weight decay** for SSM parameters `(A, B[, Δ])`. ([GitHub][2])                   |

**Legend (parameter meanings):**

* **`n_layers`**: number of S4 layers to stack (model depth). ([GitHub][2])
* **`d_model`**: width of the model (hidden dimensionality). ([GitHub][2])
* **`norm` / `prenorm`**: normalization choice and placement. ([GitHub][2])
* **`l_max`**: effective 1-D kernel length (how far in time the SSM “sees”). Example value **8192** appears in the WT103 setup. ([GitHub][3])
* **Optimizer param groups**: SSM kernel params get special treatment (smaller LR, zero WD) via grouped optimizer settings. ([GitHub][2])

---

## Training Setup (from the repo)

* **Entrypoint:** `python -m train` with **Hydra** configs under `configs/` and model presets under `models/`. ([GitHub][2])
* **Example CLI (from README):** `python -m train pipeline=mnist dataset.permute=True model=s4 model.n_layers=3 model.d_model=128 model.norm=batch model.prenorm=True`. ([GitHub][2])
* **Optimizer:** AdamW with **param-group overrides** (lower LR, zero weight decay for `(A,B[,Δ])`). ([GitHub][2])
* **Configs:** Predefined, **reproducible experiment configs** are provided in the repo to mirror paper results. ([GitHub][2])

---

## Sanity-Check Targets (to verify a re-implementation)

* **LRA performance:** The paper reports **SoTA across Long Range Arena** tasks, including solving **Path-X (length 16k)**; use the repo configs to reproduce comparable setups. ([arXiv][1])
* **Receptive field example:** The **WT103** language model checkpoint uses **`l_max = 8192`** (reference point for sequence length). ([GitHub][3])
* **Training scaffold:** Running the provided Hydra configs and respecting **SSM param-group rules** (lower LR, zero WD) should match the repo’s documented behavior. ([GitHub][2])

---

[1]: https://arxiv.org/abs/2111.00396?utm_source=chatgpt.com "Efficiently Modeling Long Sequences with Structured State Spaces"
[2]: https://github.com/state-spaces/s4?utm_source=chatgpt.com "state-spaces/s4: Structured state space sequence models"
[3]: https://github.com/changliang5811/Structured-State-Spaces?utm_source=chatgpt.com "Sequence Modeling with Structured State Spaces"


