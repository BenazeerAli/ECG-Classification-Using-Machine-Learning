Overview:
This project implements a complete ECG classification pipeline using the MIT-BIH Arrhythmia Database. It performs noise augmentation, R-peak detection, RR-interval feature extraction, and classification using Random Forest and Histogram Gradient Boosting models.
Explainability is added using SHAP to interpret how each RR-feature contributes to predictions for different heartbeat classes.

Workflow Summary:
1. Data Loading
Loads ECG signals and annotations from all 48 patients in the MIT-BIH Arrhythmia Database using wfdb.
Combines 2 minutes of ECG samples from each patient into a single array for uniform preprocessing.

2. Noise Augmentation
Four types of synthetic noise are added to improve model robustness:
Gaussian noise
Baseline drift
Powerline interference
Motion artifact spikes
A random noise augmentation function applies these during training to simulate realistic ECG distortions.

3. R-Peak Detection
R-peaks are detected using the scipy.signal.find_peaks method.
For each detected beat, the closest annotation sample from the .atr file is matched using:
closest_ann_idx = min(range(len(ann.sample)), key=lambda j: abs(ann.sample[j] - beat_idx))
This ensures every beat is labeled according to its true class (e.g., N, V, L, R).
For every ECG beat, RR-interval features are computed:

4.Feature	Description
mean_rr	Mean of surrounding RR intervals
std_rr	Standard deviation of RR intervals
min_rr	Minimum RR interval
max_rr	Maximum RR interval
These form the 4-dimensional input vector for classification.

5. Classification
Two classical ensemble models are trained:
Random Forest Classifier
Histogram Gradient Boosting Classifier
Evaluation is done on a test split using accuracy, confusion matrices, and classification reports.

Dependencies:
pip install wfdb numpy pandas matplotlib scikit-learn shap

