# Vital Signs from a PPG Signal

> **A training project in biosignal processing.** It recovers heart rate, heart-rate variability
> and breathing rate from a single optical pulse signal, and checks the answers against the
> hospital bedside monitor.

![Raw ICU PPG and the filtered signal with detected pulses](figures/ppg_pipeline.png)

## What it does

The light sensor in a smartwatch or a pulse oximeter is a photoplethysmograph (PPG): it shines
light into the skin and sees each heartbeat as a small pulse in the reflected light. From that
one signal this script reads three vital signs:

- **Heart rate**, from the time between pulses.
- **Heart-rate variability (HRV)**, from how much those intervals change beat to beat.
- **Breathing rate**, which shows up as a slow rise and fall in pulse height as the chest moves.

## How it works

1. Load an 8-minute real intensive-care recording (BIDMC record 03, PhysioNet, 125 Hz).
2. Band-pass filter the PPG to remove baseline drift and high-frequency noise.
3. Detect every pulse peak and compute the inter-beat intervals.
4. Heart rate and HRV (SDNN, RMSSD) come from those intervals.
5. Breathing rate comes from the spectrum of the slow pulse-height modulation.
6. Compare heart rate and breathing rate with the bedside monitor's own values, which are kept
   aside as ground truth.

![Heart rate and breathing rate compared with the bedside monitor](figures/ppg_validation.png)

![Beat-to-beat intervals and breathing spectrum](figures/ppg_hrv_resp.png)

## Repository structure

```
ppg-vitals/
├── ppg_vitals.py        # the whole pipeline
├── data/                # BIDMC record 03 (PhysioNet)
├── figures/             # figures written by the script
├── metrics.txt          # numbers written by the script
└── requirements.txt
```

## Run it

```bash
pip install -r requirements.txt
python ppg_vitals.py
```

## Data

BIDMC PPG and Respiration Dataset, record 03 (PhysioNet). Please cite the dataset as PhysioNet
requests: Pimentel, M.A.F. et al., "Towards a Robust Estimation of Respiratory Rate from Pulse
Oximeters," IEEE Transactions on Biomedical Engineering, 2016; and Goldberger, A. et al.,
"PhysioBank, PhysioToolkit, and PhysioNet," Circulation, 2000.

## Limits

One record from one patient, used for practice. The numbers in `metrics.txt` describe this
recording, not a general validation.

---

**Author:** Yaşar Aykut Altuntop · MSc student, Biomedical Sciences and Engineering, Koç University  
[LinkedIn](https://linkedin.com/in/aykutaltuntop) · [GitHub](https://github.com/Aykut0)
