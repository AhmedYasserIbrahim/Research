# 🧠 Time Series Models for EEG Signal Processing

## 1. RNNs (Recurrent Neural Networks)
- **Definition:** Models that process sequences step by step, where each hidden state depends on the previous one.  
- **Strengths:** Naturally designed for sequences, captures order.  
- **Weaknesses:**  
  - ❌ Suffer from **vanishing/exploding gradients** → lose information from the distant past.  
  - ❌ Sequential bottleneck → cannot parallelize, so training is slow.  
- **EEG relevance:** Too weak for long EEG streams; vanishing gradients make them unsuitable for real-time epilepsy detection.

---

## 2. LSTMs (Long Short-Term Memory)
- **Definition:** A type of RNN with extra **cell state** + **gates** (forget, input, output) to manage memory.  
- **Strengths:** Handles longer dependencies better than vanilla RNNs.  
- **Weaknesses:** Still sequential and resource-heavy.  
- **EEG relevance:** Better than RNNs, but scalability limits make them less ideal for long continuous EEG monitoring.

---

## 3. GRUs (Gated Recurrent Units)
- **Definition:** A simplified LSTM that merges memory and hidden states, with only two gates (update, reset).  
- **Strengths:** Faster and lighter than LSTM.  
- **Weaknesses:** Less expressive, still sequential.  
- **EEG relevance:** More efficient than LSTM, but still inherits scalability issues → not ideal for long real-time EEG.

---

## 4. CNNs (Convolutional Neural Networks)
- **Definition:** Use convolutional filters sliding over sequences to detect **local patterns**.  
- **Strengths:** Highly parallelizable and efficient.  
- **Weaknesses:** Only capture **short-term/local features** unless stacked very deeply.  
- **EEG relevance:** Useful for detecting rhythms/spikes (like seizures’ local bursts), but fail to capture long dependencies alone.

---

## 5. Transformers
- **Definition:** Sequence models using **self-attention**, where every token can directly attend to every other token.  
- **Strengths:**  
  - ✅ Excellent at modeling **long-range dependencies**.  
  - ✅ Fully parallelizable on GPUs/TPUs.  
- **Weaknesses:**  
  - ❌ Very expensive: attention scales quadratically with sequence length.  
  - ❌ Not inherently time-series models → need positional encoding.  
- **EEG relevance:** Powerful for discovering relationships in EEG, but too resource-heavy for **real-time, long recordings**.

---

## 6. LRUs (Linear Recurrent Units)
- **Definition:** Modernized RNNs that use **linear recurrence equations** instead of nonlinear updates.  
- **Strengths:**  
  - ✅ Efficient (\(O(T)\)).  
  - ✅ Avoid vanishing gradients → stable training.  
- **Weaknesses:** Still sequential (less parallel than SSMs).  
- **EEG relevance:** Good for online epilepsy detection since they are stable and efficient.

---

## 7. SSMs (State Space Models)
- **Definition:** Use **state-space equations** (matrix operations) to evolve hidden states:  
  \[
  h_{t+1} = A h_t + B x_t, \quad y_t = C h_t
  \]  
- **Strengths:**  
  - ✅ Capture **very long dependencies**.  
  - ✅ Parallelizable via convolution formulations.  
  - ✅ Efficient for real-time/online inference.  
- **Weaknesses:** Require careful parameterization for stability.  
- **EEG relevance:** Excellent match — can track both **short rhythms** and **long build-ups** (e.g., pre-seizure activity) while remaining efficient.

---

## 8. Modern Trends: Hybrid Models
- **CNN + RNN/SSM:** CNN captures local patterns; RNN/SSM captures long dependencies.  
- **Efficient Transformers:** (e.g., Linformer, Performer, Longformer) reduce attention cost for long EEG.  
- **SSM + Attention (e.g., Mamba):** Combine parallel efficiency with flexible global dependencies.

---

# 📊 Summary Table

| Model        | Strengths 🟢 | Weaknesses 🔴 | EEG Relevance 🧠 |
|--------------|--------------|---------------|-----------------|
| **RNN**      | Sequential modeling | Vanishing gradients, slow | Too weak for long EEG |
| **LSTM**     | Long memory via gates | Sequential, resource-heavy | Some use, not scalable |
| **GRU**      | Lighter than LSTM | Still sequential | Efficient, but limited |
| **CNN**      | Fast, parallel | Only local features | Good for spikes, not long deps |
| **Transformer** | Global relationships | Expensive (\(O(T^2)\)) | Accurate but impractical for online EEG |
| **LRU**      | Stable linear recurrence | Still sequential | Efficient, online detection |
| **SSM**      | Long deps + efficient | Math-heavy design | Best for EEG long + real-time |

---

# 🧠 Key Takeaways for EEG
1. **EEG = Long Sequences + Real-Time Processing.**  
   → RNN/LSTM/GRU are limited by scalability and gradient problems.  
2. **Transformers** give strong accuracy but are **too costly** for continuous EEG.  
3. **CNNs** help with **local bursts**, but miss long-term context if used alone.  
4. **LRUs** fix gradient issues and are efficient, good for step-by-step inference.  
5. **SSMs** are the **most promising modern choice**, offering efficient, scalable, and long-term modeling → well-suited for real-time epilepsy detection.  
6. **Hybrid models** combining CNNs, SSMs, and attention are emerging as the best path for EEG signal modeling.

---
