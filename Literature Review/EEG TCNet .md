# EEG-TCNet — Architecture-Focused Summary

**Paper:** [EEG-TCNet: An Accurate Temporal Convolutional Network for Embedded Motor-Imagery Brain–Machine Interfaces](https://arxiv.org/abs/2006.00622)
**Code:** [https://github.com/Thoriri/EEG-TCNet](https://github.com/Thoriri/EEG-TCNet)

---
## Prior Models & Motivation

The paper describes TCPT, which is the previous state-of-art CNN model for EEG MI classification. However, this model is extremely complex raising questions regarding its latency and privacy concerns. EEGNet models are more computationally feasible but it comes at the expense of accuracy (72% vs 89%).

That is the reason why EEG-TCNet was introduced - a model that combines the accuracy with the feasibility of computation. Its number of parameters increases linearly unlike traditional CNN and also it does not suffer from vanishing gradients problem like RNN, allowing it to efficiently handle long range dependencies.

## Reported Performance & Comparative Position

This model was able to achieve ~77% accuracy for classifying and ~83% accuracy after optimizing the hyperparameters based on the patient. When comparing TCNet to other models in terms of both accuracy and number of parameters (computational feasibility), it excels and exceeds all other models on the MOABB. In our use case, this model would be ideal as we require online processing, which makes complex and heavy architectures, like CNN, impossible to implement despite their high accuracy.

## Architecture Highlights

The architecture of the model implements causal convolutions where every filter's output is the same size as the input and the output at one filter depends only on the current and previous data, preventing the model from 'cheating' by looking at the future data. The implementation of causal convolutions is a vital step in our project, as we need to implement the EEG detection in real time.

The model also uses dilated convolutions, which allows the receptive field to increase exponentially. This is useful especially in deep networks as it allows the model to effectively handle long range dependencies, something that is necessary in our project as we map how seizures can build over time for detecting the occurrence of the seizure at the earliest stage.

The model has a number of features that are implemented as they showed better accuracy than their alternatives, which include:

* Batch normalization - Standardizes the values in our dataset
* Exponential linear unit (ELU) activation - Introduces non-linearity
* Normal dropout - Reduces variance and avoids overfitting

The model has 1D convolutions but it is able to take in 2D data.

The model's convolutions learn temporal data allowing the model to handle time based dependencies and features. The convolutions are then combined with the feature maps to create features representing past and current data which can be used for effective classification, especially in temporal problems like EEG detection.

## Two Approaches in the Paper

**1) Fixed EEG TCNet** — This approach optimizes hyperparameters for all patients collectively to achieve optimum accuracy. The problem with this approach is that EEG data is highly variable between patients, which is a challenge in hyperparameters as hyperparameters that work on one patient may not work on the other, giving us ~30% variation in the accuracy, which would be unacceptable for model deployment, especially in such a sensitive use case as epilepsy detection. The hyperparameters they achieved were: **F1 = 8, F2 = 16, KE = 32, KT = 4, L = 2, FT =  12, pe = 0.2, pt = 0.3.**
*C = number of EEG channels, T = number of time samples, F1 = number of temporal filters, F2 = number of spatial filters, KE = kernel size in first convolution, KT = kernel size in TCN module, and FT = number of filters in TCN module.*

**2) Variable EEG TCNet** — This approach optimizes per patient hyperparameters by using cross validated grid search, ensuring that for every patient, the model parameters fit perfectly and avoiding inter-patient variability issues that resulted in a high variation in the fixed EEG TCNet approach, which in some cases dropped the accuracy to as low as 54% for some patients.

## Training Setup

The  models are trained for 750 epochs with an Adam optimizer  at a learning rate of 0.001 and a batch size of 64. These training hyperparameters are determined via cross-validation on the training set. The loss function used is cross entropy loss function.

## Relevance to Our Use Case

This paper highly fits our use case given that it provides a very competitive accuracy to the current implemented models and exceeds them in the number of parameters needed. This allows us to detect seizures in real-time as it is not computationally heavy, and also achieve a high accuracy, comparable to the accuracy achieved by extremely heavy models. This model also takes into consideration that the problem we have at hand is a temporal problem, and the seizures can build up over time, allowing the detection of a seizure build up at the earliest stage.

---

## How the model handles **2D** and **3D** inputs with “1D” temporal convs

* **2D (channels × time):** Treat **time** as the sequence and **stack channels as features** at each time step.
  Example (3 channels × 5 time steps):

  * t1 → **[a11, a21, a31]**
  * t2 → **[a12, a22, a32]**
  * t3 → **[a13, a23, a33]**
  * t4 → **[a14, a24, a34]**
  * t5 → **[a15, a25, a35]**
    A 1D temporal convolution **slides only along time**; it does not slide across the feature dimension.

* **3D (channels × frequency × time):** At each time step, **flatten everything except time** (e.g., channels and frequency) into a feature vector; the 1D temporal conv again slides **only over time**.
  Example (2 channels × 3 frequencies × 4 time steps):

  * t1 → **[a111, a112, a113, a211, a212, a213]**
  * t2 → **[a121, a122, a123, a221, a222, a223]**
  * t3 → **[a131, a132, a133, a231, a232, a233]**
  * t4 → **[a141, a142, a143, a241, a242, a243]**

---

## Preprocessing (brief, simple)

1. **Cut a fixed-length window** around the task (e.g., ~4–5 s).
2. **Make shapes consistent** (same channel order, same trial length).
3. **Normalize per channel** (subtract mean, scale variance using **training-set** stats).
4. **Split cleanly** (avoid any leakage between training and test sets).

---

## Hyperparameter definitions (simple)

* **C:** number of EEG **channels**.
* **T:** number of **time samples** per trial.
* **F1:** number of initial **temporal filters** (different temporal views on the raw signal).
* **F2:** number of **spatial/pointwise filters** (mixing across channels after temporal filtering).
* **KE:** **kernel size** (in samples) for the first temporal conv (how long the window is).
* **KT:** **kernel size** inside the TCN (how far each TCN conv looks back).
* **L:** number of **TCN residual blocks** stacked (depth over time; longer effective memory).
* **FT:** number of **feature maps** in the TCN (its width).
* **pe / pt:** **dropout rates** in the front end / in the TCN (regularization).

---

## Why this is suitable for **seizure detection**

* **Causal** processing respects real-time constraints (no future leakage).
* **Dilations + TCN depth** let you set the **look-back window** to match the **pre-ictal horizon**.
* **Compactness** enables **on-device** inference with low latency and stronger privacy.
* **Per-patient tuning** aligns with EEG variability across individuals in clinical contexts.
