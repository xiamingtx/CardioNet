# Dataset Description
This repository contains the dataset for the paper: **"[Cardiac 3D Mechanical and Electrical Signal Reconstruction via Defocused Speckle Imaging](https://ieeexplore.ieee.org/abstract/document/11551354)"**. 

It provides dual-camera Defocused Speckle Imaging (DSI) videos and cardiac signals. The dataset is collected from two distinct cohorts to ensure robustness and clinical relevance.

* **Total Participants:** 50
  * **Lab Cohort:** 20 healthy subjects.
  * **Clinical Cohort:** 30 patients from the ultrasound department.
* **Data Modalities:** Dual-camera DSI videos, Seismocardiogram (SCG), Gyrocardiogram (GCG), and Electrocardiogram (ECG).

We release our dataset at [Here](https://huggingface.co/datasets/Ming03/DSI-SCG-ECG).

# Data Structure
The dataset includes raw videos and processed signal files in Python `.pkl` format:
* `Lab_data.pkl`: Processed signals for the 20 healthy subjects.
* `Clinic_data.pkl`: Processed signals for the 30 ultrasound department patients.

## Sampling Rates
* **DSI Optical Flow & Mechanical Signals (SCG/GCG):** 250 Hz
* **ECG Reference Signals:** 500 Hz

# Signal Dictionary (Processed Data)
When loading the `.pkl` files, the data for each subject is stored as dictionaries containing numpy `ndarray` objects. Most 2D arrays (e.g., `(N, 500)`) represent `N` cardiac cycles resampled to a fixed length of 500.

## 1. Reference Signals (Ground Truth)
* `ECG`: Reference Electrocardiogram signal.
* `SCG`: Reference Seismocardiogram signal.
* `GCGx`: Reference Gyrocardiogram signal (x-axis).
* `GCGy`: Reference Gyrocardiogram signal (y-axis).

## 2. Reconstructed Signals (via Physical Model)
Signals reconstructed using the physical model from two tracked points (`g` and `r`):
* `SCG_g` / `SCG_r`: Reconstructed SCG signals from point `g` and point `r`.
* `GCGx_g` / `GCGx_r`: Reconstructed GCGx signals from point `g` and point `r`.
* `GCGy_g` / `GCGy_r`: Reconstructed GCGy signals from point `g` and point `r`.

## 3. Camera Motion Signals (DSI Optical Flow)
Raw motion signals captured by the two cameras at two specific points (`g` and `r`) across the x and y axes:
* **Camera 1:** `cam1_gx`, `cam1_gy`, `cam1_rx`, `cam1_ry`
* **Camera 2:** `cam2_gx`, `cam2_gy`, `cam2_rx`, `cam2_ry`

## 4. Raw Signal Lengths
1D arrays recording the original signal length of each cardiac cycle before length normalization:
* `ECG_Raw`: Original length of ECG signals per cardiac cycle.
* `SCG_Raw`: Original length of SCG signals per cardiac cycle.
* `GCGx_Raw`: Original length of GCGx signals per cardiac cycle.
* `GCGy_Raw`: Original length of GCGy signals per cardiac cycle.

# Usage Example
Here is a quick example of how to load and explore the `.pkl` files in Python:

```python
import pickle

# Load the Lab data
with open('Lab_data.pkl', 'rb') as f:
    lab_data = pickle.load(f)

subject_id = 'Lab_1'
video_id = 0

subject_data = lab_data[subject_id][video_id]

# Extract reference ECG and reconstructed SCG
ecg_ref = subject_data['ECG']             # shape: (N_cycles, 500)
scg_reconstructed = subject_data['SCG_g'] # shape: (N_cycles, 500)
raw_lengths = subject_data['ECG_Raw']     # shape: (N_cycles,)

print(f"Number of cardiac cycles: {ecg_ref.shape[0]}")
```

# Citing this work
Please cite the following paper if you find our code helpful.
```
@article{xia2026cardiac,
  title={Cardiac 3D Mechanical and Electrical Signal Reconstruction via Defocused Speckle Imaging},
  author={Xia, Ming and Liu, Lin and Zhao, Ningbo and Luo, Zi and Shan, Caifeng and Wang, Wenjin},
  journal={IEEE Transactions on Biomedical Engineering},
  year={2026},
  publisher={IEEE}
}
```
