# EEG N170 Preprocessing and Classification

This project provides a pipeline for preprocessing EEG data from an N170 paradigm and training a deep learning classifier on the resulting epochs. It consists of two main scripts:

* **`preproc.py`**: Loads raw BIDS‐formatted EEG data, applies filtering, ICA artifact removal, epoching, automatic rejection, and saves preprocessed NumPy arrays and ERP plots.
* **`classify.py`**: Loads the preprocessed data, extracts channels/time‐windows of interest (N170), performs data augmentation, builds and trains a DeepConvNet model in TensorFlow/Keras, and evaluates performance.

---

## Repository Structure

```
project-root/
├── preproc.py            # EEG preprocessing and ERP plotting
├── classify.py           # Model training and evaluation script
├── README.md             # Project overview and usage instructions
├── Load_and_Predict.py   # Load a model and make predictions on new data
└── Models/               # Saved models for future predictions
```

## Requirements

* **Python 3.8+**
* **MNE**
* **autoreject**
* **NumPy**
* **scikit-learn**
* **imbalanced‑learn**
* **Matplotlib**
* **TensorFlow 2.x**
* **Seaborn**

You can install the required packages via pip:

```bash
pip install mne autoreject numpy scikit-learn imbalanced-learn matplotlib tensorflow seaborn
```

---

## Usage

### 1. Preprocessing

**Script**: `preproc.py`

1. **Configure paths**:

   * Update `bids_root` to point to your BIDS‐formatted EEG dataset.
   * Ensure EEGLAB `.set` files are stored under `sub-*/eeg/` as `sub-*_task-N170_eeg.set`.

2. **Run the script**:

   ```bash
   python preproc.py
   ```

3. **Outputs**:

   * **ERP plots** saved under `Data/Plots/{subject}/Evoked_Face`, `Evoked_Car`, and `Comparison/`.
   * **Preprocessed data**:

     * Individual subject `.npy` files: `X_eeg_{subject}.npy`, `y_labels_{subject}.npy`
     * Concatenated and balanced arrays: `Data/X_eeg.npy`, `Data/y_labels.npy`

---

### 2. Classification

**Script**: `classify.py`

1. **Prepare data**:

   * Ensure preprocessed `.npy` files are in `Data/Data/` and subject folders contain `X_eeg_{subject}.npy` & `y_labels_{subject}.npy`.
   * Script will load these and extract epochs around the N170 window (100–200 ms).

2. **Run the script**:

   ```bash
   python classify.py
   ```

3. **Outputs**:

   * **Workshop data**: Saved under `Data/Workshop_data/X_data.npy` & `y_data.npy`.
   * **Model training logs** printed to console.
   * **Final test accuracy** displayed.
   * **Plots**:

     * Training/validation accuracy & loss curves.
     * Confusion matrix heatmap.
     * Classification report summarizing precision, recall, F1-score.

---

## Customization

* **Filtering parameters**: Modify `raw.filter()` and `raw.notch_filter()` frequencies in `preproc.py`.
* **Epoch timing**: Adjust `tmin`, `tmax`, and baseline in both scripts to target different ERP windows.
* **Model architecture**: Edit the `DeepConvNet` function in `classify.py` to explore alternative network designs or hyperparameters.

---

## License & Author

**Author**: Owen Anderson<br>
**License**: MIT License (feel free to adapt for academic and research purposes)
