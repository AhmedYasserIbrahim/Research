# EEG Dataset Benchmark (Compatibility with Siena)

## 1. Dataset Comparison Table

| Dataset | Modality | Channels | Sampling Rate | Patients | Age Range | Seizure Labels | Volume (Hours) |
|--------|----------|-----------|----------------|----------|------------|----------------|----------------|
| **Siena** | Scalp EEG (10–20) | ~21 | 512 Hz (downsampled to 256 Hz) | 14 | 20–71 yrs | Yes | ~30–40 hours |
| **SeizeIT2** | Wearable EEG | Behind-ear sensors | ~250 Hz | 125+ | All ages | Yes | ~11,000 hours |
| **NMT** | Scalp EEG | ~19 | 200 Hz | 2417 recordings | All ages | No | ~1,800–2,000 hours (estimated) |
| **TUH-SZ** | Scalp EEG | 19–36 | 250–512 Hz | 600+ | Broad | Yes | ~1,500–2,000 hours |
| **CHB-MIT** | Scalp EEG (10–20) | 22–23 | 256 Hz | 23 | 1.5–22 yrs | Yes | ~98 hours |

---

## 2. Dataset Compatibility Discussion

### ❌ SeizeIT2  
SeizeIT2 is not suitable because it uses **wearable behind-the-ear sensors**, which are very different from Siena’s standard scalp EEG setup.  
The signals do not match Siena’s structure or channel layout, so combining them would not be meaningful.

---

### ❌ NMT  
NMT is not usable because it **does not include seizure onset or offset labels**.  
It mainly marks recordings as normal or abnormal, which does not allow us to extract seizure windows.  
For this reason, NMT cannot be merged with Siena for seizure detection.

---

### ✅ TUH Seizure Corpus  
TUH is a strong match with Siena.  
It is standard **scalp EEG**, contains **clear seizure labels**, and provides a very large amount of seizure activity.  
By downsampling TUH to 256 Hz and using the common 10–20 channels, we can add more seizure windows to help **balance Siena’s class imbalance**, since Siena has fewer seizures overall.

---

### ⭐ CHB-MIT  
CHB-MIT is pediatric EEG but still uses the same **scalp EEG**, the same **10–20 system**, and also **256 Hz**, which matches our processed Siena exactly.  
This makes it ideal for studying **cross-age generalization**, which is not addressed in most seizure detection research.

A recent 2025 paper, **APTGNet**, also explores age generalization, but it differs from our work because:
- it does **not** use Siena  
- its “adult” dataset is mostly **intracranial EEG**, which is **invasive**, unlike Siena  
- it does not include older adults (Siena goes up to 71 years)  
- it mixes very different EEG modalities from unrelated sources  

Our approach focuses on consistent, **non-invasive scalp EEG** from **children, adults, and older adults**.  
This creates a cleaner and more realistic cross-age evaluation and fills a noticeable gap in the literature.

---
