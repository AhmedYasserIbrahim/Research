# EEG Dataset Benchmark (Compatibility with Siena)

## 1. Dataset Comparison Table

| Dataset | Modality | Channels | Sampling Rate | Patients | Age Range | Seizure Labels | Volume |
|--------|----------|-----------|----------------|----------|------------|----------------|---------|
| **Siena** | Scalp EEG (10–20) | ~21 | 512 Hz (downsampled to 256 Hz) | 14 | 20–71 yrs | Yes | ~30–40 hrs |
| **SeizeIT2** | Wearable EEG | Behind-ear sensors | ~250 Hz | 125+ | All ages | Yes | Very large |
| **NMT** | Scalp EEG | ~19 | 200 Hz | 2417 recordings | All ages | No | Large |
| **TUH-SZ** | Scalp EEG | 19–36 | 250–512 Hz | 600+ | Broad | Yes | 1500+ hrs |
| **CHB-MIT** | Scalp EEG (10–20) | 22–23 | 256 Hz | 23 | 1.5–22 yrs | Yes | ~98 hrs |

---

## 2. Dataset Compatibility Discussion

### ❌ SeizeIT2  
SeizeIT2 is not suitable because it uses **wearable behind-the-ear sensors**, which are very different from Siena’s standard scalp EEG setup.  
The signals have different characteristics and cannot be merged meaningfully with Siena.

---

### ❌ NMT  
NMT is not usable for our task because it **does not contain seizure onset or offset labels**.  
Most recordings are simply marked as normal or abnormal, so we cannot extract seizure windows in a reliable way.  
Therefore, it cannot be combined with Siena.

---

### ✅ TUH Seizure Corpus  
TUH is a good match with Siena.  
It uses standard **scalp EEG**, includes **clear seizure labels**, and contains a very large number of seizure events.  
We can downsample TUH to 256 Hz and select the same common 10–20 channels.  
This would allow us to **add more seizure windows** to help balance Siena, which has fewer seizures.

---

### ⭐ CHB-MIT  
CHB-MIT is a pediatric dataset, but it uses the same **scalp EEG setup**, the same **10–20 system**, and **256 Hz**, which matches our processed Siena exactly.  
This makes it useful for studying **how seizure detection models generalize across age groups**.

There is a recent paper called **APTGNet (2025)** that tries to perform cross-age epilepsy detection, but their work is different from ours because:
- they do *not* use Siena  
- their “adult” data is mostly **intracranial EEG**, which is **invasive**, unlike Siena  
- they also do not include older adults like those in Siena  
- their datasets come from very different sources and setups  

Our contribution focuses specifically on **non-invasive scalp EEG** across **children, adults, and older adults**, using consistent datasets (CHB-MIT + Siena).  
This fills a clear gap in the literature.

---

