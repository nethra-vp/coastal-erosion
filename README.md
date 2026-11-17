# A Quantum Based Hybrid Approach For Predictive Modelling Of Coastal Erosion Dynamics

This project explores binary coastal-erosion risk prediction using classical machine-learning, quantum machine-learning, and hybrid quantum-classical models. The primary workflow is in `coastal_erosion.ipynb`.

## What the project does

The notebook trains and evaluates models to distinguish **High** erosion risk from **Low/Moderate** risk. Its six environmental input features are:

- `wave_height`
- `wave_period`
- `wave_direction`
- `precipitation`
- `wind_speed`
- `tidal_height_m`

It compares model metrics, plots confusion matrices and ROC curves, and evaluates robustness under noise stress tests. Its final prediction/validation section applies a trained SVM to a later-period CSV and can save predictions to `coastal_erosion_predictions.csv`.

## Models

| Model | Brief description |
|---|---|
| Random Forest (RF) | An ensemble of decision trees for nonlinear tabular classification. |
| Support Vector Machine (SVM-RBF) | An RBF-kernel classifier; this is also used in the final prediction pipeline. |
| XGBoost | A gradient-boosted tree classifier with class-imbalance weighting. |
| 4-Qubit Data Re-uploading Classifier | Re-encodes PCA-reduced features through a trainable four-qubit circuit. |
| Standard Variational Quantum Classifier (VQC) | Encodes PCA-reduced inputs once, then applies trainable entangling layers. |
| Angle-Embedded VQC (AE-VQC / Old Quantum Model) | An earlier angle-embedded variational circuit retained for comparison. |
| 6-Qubit Data Re-uploading Classifier | A six-qubit version of the data re-uploading circuit. |
| Hybrid Quantum-Classical Neural Network (Hybrid QML) | Combines PyTorch feature layers and classifier layers with a PennyLane quantum layer. |

Standalone quantum classifiers use PCA and angle scaling for circuit inputs. The hybrid model scales the environmental features before its PyTorch/PennyLane layers.

## Project files

| File or directory | Purpose |
|---|---|
| `coastal_erosion.ipynb` | Main model-training, evaluation, stress-test, and prediction/validation notebook. |
| `datasets/` | `dataset_2020-2025.csv` contains Labeled data used by the model-training and held-out evaluation cells. |
| `datasets/` | `dataset_2026-2030.csv` contains Later-period data used by the final prediction/validation section. |
| `coastal_erosion_predictions.csv` | Prediction output produced by the final notebook pipeline. |
| `Model_Results_Visualizations/` | Visualizations generated from model training. |
| `Technical_Diagrams_and_Illustrations/` | Project diagrams and report graphics. |

The CSVs contain the six model input features above as well as other metadata. The `erosion_risk` column is the target where labels are available.

## Setup

Use a Python environment with Jupyter and the packages imported by the notebook. For example, from PowerShell in the project directory:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install jupyter pandas numpy scikit-learn xgboost pennylane pennylane-lightning torch matplotlib seaborn
```

## Run the notebooks

1. Keep the CSV files in the project root, alongside `coastal_erosion.ipynb`; the notebook reads them by relative path.
2. Start Jupyter with `jupyter lab` or `jupyter notebook`.
3. Open `coastal_erosion.ipynb` and run its cells from top to bottom. Later evaluation, stress-test, and prediction cells depend on the trained model objects and preprocessing variables created by earlier cells.
4. Allow extra time for the PennyLane quantum and hybrid-model training and stress-test cells.

The final prediction/validation cell currently uses `dataset_2026-2030.csv` both as prediction input and as the source of validation labels. 

## Generated outputs

Running the main notebook can create evaluation figures in the working directory, including:

- Per-model confusion matrices such as `rf_confusion_matrix.png`, `svm_confusion_matrix.png`, and the `hybrid_confusion_matrix` PNGs.
- Model-comparison figures such as `all_models_accuracy_precision_f1.png`, `all_models_roc_curves.png`, and `training_time_vs_roc_auc.png`.
- Stress-test figures such as `all_models_stress_test.png`, `all_models_snr_vs_accuracy.png`, and `all_models_snr_vs_high_risk_recall.png`.
- `validation_confusion_matrix.png` and `coastal_erosion_predictions.csv` from the final prediction/validation pipeline.

The exact files produced depend on which notebook cells are run. Generated figures and CSV output may be overwritten when their corresponding cells are run again.
