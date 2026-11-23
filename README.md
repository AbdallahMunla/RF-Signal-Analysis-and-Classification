# RF Signal Analysis & Modulation Classification

**Python DSP project | I/Q analysis · FFT · QPSK · FIR filtering · BPSK/QPSK classification**

An independent communications-engineering project exploring how complex baseband signals can be generated, analyzed, filtered, and classified. I built the experiments in Python, compared simulations with theory, and improved a modulation classifier after discovering its sensitivity to carrier-phase rotation.

## At a glance

| Experiment | Result |
| --- | --- |
| **QPSK BER simulation** | Simulated BER closely followed analytical AWGN predictions |
| **51-tap FIR low-pass filter** | ~60.3 dB relative response rejection between 100 Hz and 300 Hz |
| **Classifier at 0 dB SNR** | **51.2% → 74.8%** accuracy after changing the feature |
| **Classifier at 5–20 dB SNR** | 100% accuracy in the recorded, finite synthetic test trials |

> **Scope:** Results come from *simulated* baseband signals, not real RF or drone recordings.

## What I implemented

- **I/Q and spectral analysis:** generated complex signals, visualized in-phase and quadrature components, and analyzed spectra with the FFT.
- **Digital communications:** generated QPSK constellations, introduced additive white Gaussian noise (AWGN), implemented hard-decision demodulation, and compared BER with theory.
- **Digital filtering:** designed and evaluated a 51-tap FIR low-pass filter using SciPy.
- **Modulation classification:** implemented BPSK/QPSK classifiers and evaluated accuracy across signal-to-noise ratios with random carrier-phase rotation.

## The interesting problem: phase sensitivity

My first classifier used the fraction of signal energy on the Q axis. It worked in some cases but was sensitive to the orientation of the constellation: a rotated BPSK signal could be mistaken for QPSK.

I replaced that feature with a normalized second-order moment:

```python
power = np.mean(np.abs(signal) ** 2)
m2 = np.abs(np.mean(signal ** 2)) / power
```

Taking the magnitude makes this feature invariant to a *constant* phase rotation. In the recorded experiments, classification accuracy at **0 dB SNR increased from 51.2% to 74.8%**.

## Explore the project

| File | Contents |
| --- | --- |
| [**Jupyter notebook**](./FFT_python%20simulation.ipynb) | Executable Python experiments, plots, and recorded outputs |
| [**Project presentation (PDF)**](./RF_DSP_LinkedIn_Carousel_Refined.pdf) | Seven-slide overview of implementation, tests, results, and limitations |

**Tools:** Python · NumPy · SciPy · Matplotlib · Jupyter Notebook

## Limitations & next steps

These experiments use synthetic BPSK/QPSK baseband signals with AWGN and a constant random carrier phase. They do **not** demonstrate detection of real drones. Further work could evaluate real SDR I/Q captures, carrier-frequency offset, timing errors, multipath, and more modulation types.

---

**Abdallah El Munla**  
Communications & Electronics Engineer · Admitted to M.Sc. Electrical Communication Engineering, University of Kassel (Summer Semester 2027)  
[LinkedIn](https://www.linkedin.com/in/abdallah-el-munla)
