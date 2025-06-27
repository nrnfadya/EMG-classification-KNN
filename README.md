## EMG Classification KNN

This project was classificate the EMG signal during activities of daily life (ADL) to FABOOS group. The classification was performed using the newly proposed EMAHA-DB1 dataset, which consists of multichannel surface EMG (sEMG) signals collected from 25 healthy individuals performing 22 daily activities. These activities were systematically grouped into five FAABOS categories: no object action, pull/push drawer, object grasping, flexion and extension of fingers, and writing. sEMG signals were acquired using five Noraxon Ultium wireless sensors placed on representative upper limb muscles. 

## Dataset
Surface EMG (sEMG) signals collected during activities of daily life (ADL) provide better insights toward understanding neuromuscular disorders, persons with limb disabilities, aging adults and neuromotor deficits. Hand movement and control mechanism analysis may improve the design of prosthetic devices, realistic biomechanical hands, and rehabilitation therapy. Towards this goal, we present a sEMG signal database corresponding to the Indian population named “ElectroMyography Analysis of Hand Activities - DataBase -1 (EMAHA-DB1).” The dataset can be used for classification studies and statistical analysis of sEMG signals.

source

N. K. Karnam, A. C. Turlapaty, S. R. Dubey and B. Gokaraju, "EMAHA-DB1: A New Upper Limb sEMG Dataset for Classification of Activities of Daily Living," in IEEE Transactions on Instrumentation and Measurement, doi: 10.1109/TIM.2023.3279873.

## Preprocessing
To ensure high-quality input data for classification, each sEMG signal undergoes a structured preprocessing pipeline consisting of three main steps: notch filtering, low-pass filtering, and wavelet denoising.
- Notch Filtering

The first step is to remove power line interference at 50 Hz using an IIR notch filter (notch_filter). This filter is specifically designed with a quality factor Q=30 to sharply attenuate the 50 Hz frequency while preserving other frequency components of the EMG signal. This step helps eliminate noise from electrical sources such as lighting or power outlets.

- Low-Pass Filtering

After removing the 50 Hz component, a 4th-order Butterworth low-pass filter with a cutoff frequency of 150 Hz is applied via the lowpass_filter function. This stage suppresses high-frequency noise beyond the typical frequency range of sEMG, which usually lies below 150 Hz, while retaining the biologically relevant signal components.

- Wavelet Denoising

To further clean the signal, wavelet-based denoising is performed using the wavelet_denoise function with the Symlet-4 (sym4) wavelet up to level 3 decomposition. The noise threshold is estimated adaptively from the detail coefficients using a robust estimator, and soft thresholding is applied to remove high-frequency noise components. This step is particularly useful for preserving signal morphology while suppressing stochastic noise.

- Full Signal Processing Pipeline

These three steps are wrapped in the process_signal function, which sequentially applies notch filtering, low-pass filtering, and wavelet denoising to each channel of the EMG signal. The resulting signal is then rectified using the absolute value to prepare it for feature extraction.
