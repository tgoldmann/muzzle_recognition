# The Muzzle as a Biometric Feature: Cattle Identification Using Unique Muzzle Patterns


## Availability Note

In accordance with the journal's submission guidelines and the ongoing peer-review process, the full source code is currently set to **private**. All assets will be made public upon the final publication of the manuscript.

---

## Research Overview

This study addresses the limitations of traditional cattle identification (e.g., ear tagging) by treating the cattle muzzle as a unique biometric trait, similar to a human fingerprint.

---

## Dataset

Raw public datasets for cattle identification often contain significant noise. We unified six public sources (**CMID**, **Muzzle-268**, **CFFD**, **MCND**, **Muzzle-211385**, and **Roboflow**) and performed a rigorous manual filtering process.

### Links
The experiments are based on images from the following publicly available datasets:

- Cattle Muzzle Images Dataset (CMID)  
  https://www.kaggle.com/datasets/kollabathulakaushik/cattle-images-db-for-muzzle-based-identification

- Muzzle-268  
  https://zenodo.org/records/6324361

- Cows Frontal Face Dataset (CFFD)  
  https://zenodo.org/records/10535934

- Muzzle Cow New Dataset (MCND)  
  https://zenodo.org/records/7988559

- Muzzle-211385  
  https://www.kaggle.com/datasets/norashimah/muzzle-211385

### Quality Control Protocol:

* **Exclusion Criteria:** Images were removed if >75% of the muzzle was occluded by hay, dirt, or halters, or if motion blur rendered the bead structure indiscernible.
* **Consistency:** Duplicate identities and images found across different source folders (particularly in CFFD) were merged or removed.
* **Minimum Sample Size:** Each identity was curated to ensure enough samples existed for meaningful representation learning.

### Final Recognition Split:

| Dataset | Filtered IDs | Filtered Images | Recognition IDs | Recognition Images |
| --- | --- | --- | --- | --- |
| **CMID** | 24 | 742 | 20 | 617 |
| **Muzzle-268** | 251 | 4583 | 245 | 4440 |
| **CFFD** | 140 | 720 | 135 | 703 |
| **MCND** | 27 | 142 | 24 | 126 |
| **Muzzle-211385** | 40 | 863 | 24 | 482 |
| **Total** | **482** | **7050** | **448** | **6368** |

*Note: 34 identities were completely withheld from all training and validation phases to be used exclusively for the open-set verification benchmark (folder verification).*


## Training and Close-Set evaluation
The following text files contain the filtered filenames used for training, validation, and testing of the recognition models:

- `training_close_set/train_files.txt`  
  List of image filenames used for training.

- `training_close_set/val_files.txt`  
  List of image filenames used for validation.

- `training_close_set/test_files.txt`  
  List of image filenames used for testing.

---

## Open-Set Verification Protocol

To prevent the models from achieving "false" accuracy by learning background artifacts or camera sensor noise (domain bias), we introduced a **Strict Within-Dataset Verification Protocol**.

Instead of random image pairing, our protocol generates **2,000 pairs** (1,000 genuine, 1,000 impostor) under the following constraints:

1. **Genuine Pairs:** Two different images of the same animal.
2. **Impostor Pairs:** Two different animals originating **from the same source dataset**.

By enforcing that impostor pairs must share the same environmental context (lighting, background, camera type), the model is forced to learn the fine-grained biometric differences in the muzzle ridges rather than relying on image metadata or background shortcuts.
