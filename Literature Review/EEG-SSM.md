# EEG-SSM — Architecture-Focused Summary

**Paper:** [EEG-SSM (arXiv:2407.17801)](https://arxiv.org/abs/2407.17801)

---

## Motivation

Traditionally, SSMs were mainly used for capturing temporal reasoning, but they ignored spectral features (information across frequencies). This is a problem for EEG: the model can learn long-range temporal relationships yet miss vital frequency-related characteristics.

This paper introduces **EEG-SSM**, which brings together both needs: capturing **temporal** dependencies and handling **spectral** information. The model has two components: a **temporal** part and a **spectral** part.

---

## Task & Dataset

The paper studies **dementia detection** with three classes:

1. **Healthy Control (HC)** — control group
2. **Alzheimer’s (AD)** — memory loss and confusion due to neural death
3. **Frontotemporal degeneration (FTD)** — behavioral and motor deficits from frontal/temporal lobe decline

Data: **88 subjects**, sampled at **500 Hz**, **19 EEG channels**, **12–16 minutes** per subject, resting state. Small eye/jaw artifacts were removed during preprocessing for cleaner signals.

---

## Why EEG

Other options include cognitive tests (simple but influenced by education/emotion) and MRI (accurate but expensive/complex). EEG offers a non-invasive, cost-effective middle ground.

---

## Related Work & Positioning

Prior approaches include **SVM, MLP, CNN**, and **Transformers**. Transformers bring self-attention and parallel compute but can be heavy and can drop on long sequences. **SSMs** aim to keep the sequence strengths while addressing long-range modeling and efficiency.

---

## Model Overview: EEG-SSM

An SSM maps input **x** to a higher-dimensional latent **h** over time. While SSMs are often built for 1D sequences, this paper adapts them to **multivariate EEG**. The ability to capture **long-range dependencies** is especially helpful for **resting-state** data, which is close to our planned epilepsy setting.

### Temporal Component

3D EEG is first converted to 2D, then **linearly projected** to a feature vector to make temporal feature extraction straightforward.

### Spectral Component

The EEG is split into **Delta (0.5–4 Hz), Theta (4–8 Hz), Alpha (8–12 Hz), Beta (12–20 Hz), Gamma (20–40 Hz)**. A **1D conv** turns these into a single vector. An **ensemble layer** (Opt-W) then **weights the bands** per patient/case, highlighting the most informative bands.

### Model Inputs

EEG-SSM takes three inputs: a **temporal features** array, a **spectral features** array, and a **combined** array.

---

## Training Setup

The 3-class model uses **cross-entropy loss** and **Adam (lr = 0.001)** for **500 epochs**. The input size is **19** to match the channels.

---

## Baselines & Comparison

The paper also trains **RNN** and **Transformer** baselines.

* **RNN:** around **41–43%** in the shorter segments.
* **EEG-Transformer:** about **70%** at **2 s**, with accuracy dropping as segments get longer.
* **EEG-SSM (combined):** **~91%** at **2 s**, remaining robust as segments grow; performance declines at **256 s**, with the **temporal-only** variant dropping the most.

---

## Model Size & Sensitivity

EEG-SSM uses about **4.4k parameters**, much smaller than the baselines, while achieving higher accuracy.

* **Sampling rate:** ~**83%** at **250 Hz (2 s)**; ~**73–74%** at **125 Hz (4 s)** — lower rates reduce accuracy.
* **Channels:** **12 channels** ≈ **78%**; **6 channels** ≈ **69%** — more channels help.
* **Segment length:** Very long segments (**256 s**) reduce accuracy, especially for temporal-only.

---

## Relevance to Our Project

EEG-SSM is efficient and effective for EEG classification. It models **time** and **frequency** together, uses **few parameters**, and—when run **unidirectionally**—is suitable for **real-time/online** processing. Because our task is **binary** (seizure vs. non-seizure), we may match or improve on the 3-class figures. Our **256 Hz** sampling is lower than 500 Hz (likely a modest drop), and **variable channel counts** may cause per-patient differences—both trends match the paper’s findings.

---

## Code & Practicality

The paper **does not provide an official open-source repository**. There are **similar EEG SSM/Mamba-based projects** with open code (e.g., EEGMamba, S4/Mamba libraries), but adapting them to this paper’s **temporal + spectral + Opt-W** design and making it **online/causal** will **not be plug-and-play**. Expect custom data loaders, careful windowing, and architectural tweaks.
