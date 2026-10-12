# Decoding Left- and Right-Hand Motor Imagery from EEG Signals

**Jamiu Hassan** | B.Sc. Human Anatomy, Bayero University Kano

A beginner computational neuroscience project using Python to classify imagined left- versus right-hand movements from scalp EEG. Prepared as an independent learning project for a Neurasmus application; no programme affiliation is implied.

[View executed Kaggle notebook](https://www.kaggle.com/code/jamiuhassan/decoding-left-and-righthand-motor-imagery-from-eeg) | [GitHub repository](https://github.com/jamiuhassan0041-rgb/eeg-motor-imagery-classification)

## Research question

Can mu- and beta-band power at three central electrodes distinguish left/right motor imagery across recordings from one participant?

## Files and execution

- `Motor_Imagery_EEG_Jamiu_Hassan.ipynb`: cleaned analysis with explanations, checks, evaluation and limitations.
- `README.md`: project overview.

Upload the notebook to Kaggle, enable Internet access, and run every cell from top to bottom in a fresh session. The notebook installs MNE, uses NumPy, Pandas, Matplotlib, SciPy and scikit-learn, and downloads three EDF files into `/kaggle/working`. Package versions are printed; retain them and save executed outputs for reproducibility. Raw EEG is downloaded from PhysioNet rather than bundled in this project.

**Verification status:** Kaggle Version 2 completed successfully, and its results were checked on 12 October 2026. The notebook supplied here preserves saved outputs from matching analysis cells in the earlier successful Kaggle export; Version 2 independently reproduced the results. Only documentation and troubleshooting-cell cleanup differ from the executed analysis. No local rerun is claimed.

Verified environment: Python 3.13.15, MNE 1.13.2, NumPy 2.1.3, Pandas 2.3.3, SciPy 1.16.3 and scikit-learn 1.6.1.

## Dataset and methods

PhysioNet EEG Motor Movement/Imagery Dataset v1.0.0, participant S001, runs R04, R08 and R12. Each has 64 channels sampled at 160 Hz for 125 seconds. For these runs, T0 is rest, T1 left-fist imagery and T2 right-fist imagery.

1. Filter each continuous recording independently at 1-40 Hz using a zero-phase FIR filter.
2. Inspect spectra, channel amplitudes and selected traces; retain the existing reference and all trials for an exploratory baseline. No ICA or amplitude-based rejection was applied.
3. Extract 0-3 second imagery epochs (481 samples, including both endpoints).
4. Estimate Welch PSD with two-second windows and one-second overlap.
5. Integrate mu (8-13 Hz) and beta (13-30 Hz) power at C3, Cz and C4; convert to dB relative to 1 microvolt squared, yielding six features.
6. Use StandardScaler followed by logistic regression (C=1.0). Fit scaling only on training data.
7. Explore three contiguous-block folds within R04, then fit all R04 trials and evaluate untouched model settings on R08 and R12. Compare with a training-majority dummy classifier.

## Results

| Evaluation | Correct | Accuracy | Balanced accuracy |
|---|---:|---:|---:|
| Within-R04 cross-validation | 10/15 | 66.7% | 65.2% |
| Test on R08 | 9/15 | 60.0% | 58.0% |
| Test on R12 | 10/15 | 66.7% | 67.9% |
| Combined R08 + R12 | 19/30 | 63.3% | 63.3% |
| Combined dummy baseline | 15/30 | 50.0% | 50.0% |

Combined confusion matrix:

| | Predicted left | Predicted right |
|---|---:|---:|
| Actual left | 13 | 2 |
| Actual right | 9 | 6 |

Left recall was 86.7%; right recall was 40.0%. The model made four more correct predictions than the dummy but tended to predict left. These are modest exploratory results, not evidence of a reliable BCI.

## Limitations

Only one participant and 45 total task trials were analysed (15 training, 30 separate-run test). Runs are not independent participants or a demonstration of cross-session performance. Frontal transients consistent with possible eye artifacts were observed, and frontal amplitudes tended to differ by class. Central-channel selection does not guarantee freedom from this confound. Artifact handling and test-run signal-quality inspection remain incomplete. No significance test was performed. We cannot attribute decoding solely to motor-related neural activity, claim above-chance statistical significance, or generalise to new participants. Absolute power does not establish event-related desynchronization.

Future improvements should predefine artifact handling and evaluate additional participants with appropriate grouping. Since current test results are known, tuning on these runs would require fresh test data for an independent assessment.

## Skills demonstrated

EEG data handling, anatomical interpretation of electrode selection, signal processing, spectral analysis, Pandas feature tables, scikit-learn pipelines, held-out evaluation, and transparent reporting of limitations.

## Sources

- [PhysioNet dataset and citation/licence information](https://physionet.org/content/eegmmidb/1.0.0/), DOI [10.13026/C28G6P](https://doi.org/10.13026/C28G6P).
- [MNE filtering](https://mne.tools/stable/auto_tutorials/preprocessing/30_filtering_resampling.html).
- [MNE spectra](https://mne.tools/stable/auto_tutorials/time-freq/10_spectrum_class.html).
- [scikit-learn common pitfalls](https://scikit-learn.org/stable/common_pitfalls.html).
- [Original Kaggle working notebook](https://www.kaggle.com/code/jamiuhassan/decoding-left-and-righthand-motor-imagery-from-eeg).
