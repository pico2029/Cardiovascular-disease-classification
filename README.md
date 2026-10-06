# Cardiovascular Disease Classification

This educational project explores binary classification using a cardiovascular dataset. The notebook compares a decision tree and a neural network to predict `cardio` (`0`: the condition is not recorded; `1`: the condition is recorded). It covers exploratory data analysis, data cleaning, hyperparameter selection, test-set evaluation, and the effect of different classification thresholds.

> This is an experiment on a public dataset. The models have not been validated for diagnosis or clinical decision-making.

## Repository contents

| File | Description |
| --- | --- |
| `cardiovascular-classification.ipynb` | Code, explanations, charts, and saved results. |
| `requirements.txt` | Python packages needed to run the notebook. |
| `.gitignore` | Exclusions for downloaded data, temporary files, and local documents. |


## Dataset

The notebook uses the [Cardiovascular-Disease-Dataset on Kaggle](https://www.kaggle.com/datasets/akshatshaw7/cardiovascular-disease-dataset), referenced in the code as `akshatshaw7/cardiovascular-disease-dataset`. In the saved run, it contains **70,000 records and 14 columns**: 11 predictors, two identifier columns excluded from modeling, and the `cardio` target.

The predictors describe age, sex, height, weight, blood pressure, cholesterol, glucose, smoking, alcohol consumption, and physical activity. The preprocessing pipeline handles implausible measurements, particularly blood-pressure values.

When run, the notebook first looks for `health_data.csv` in the current working directory. If the file is absent, it attempts to download the dataset with `kagglehub`. If the download fails in Google Colab, it offers a manual upload of a file named `health_data.csv`. The file must match the schema checked by the notebook; other dataset versions may need adjustments. Automatic download requires an Internet connection, and Kaggle access requirements may apply.

See the dataset page for its documentation and terms of use. The CSV should not be committed to this repository.

## How to run

**Python 3.11** is recommended because it is the version recorded in the notebook metadata. From the directory containing the repository files:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
jupyter lab cardiovascular-classification.ipynb
```

On Windows, activate the environment with `.venv\Scripts\activate` before installing the packages. Alternatively, open the notebook in Google Colab and install any missing packages listed in `requirements.txt`.

Run all cells from the beginning in order. Neural-network training and hyperparameter searches may take time. The notebook includes saved outputs that can be viewed without rerunning it. The exact package versions used for the saved run were not recorded, so a new run may produce slightly different results.

## Method

1. Check the dataset schema, missing values, duplicates, and measurement plausibility.
2. Split records into training, validation, and test sets, keeping records with identical predictor profiles in the same set. Exploratory analyses after the split use only the training set.
3. Build a cleaning and preprocessing pipeline, including treatment of implausible measurements.
4. Compare a baseline, a decision tree, and a neural network. Select hyperparameters using development data only; reserve the test set for final evaluation.
5. Use the validation set to examine thresholds that meet different minimum recall targets, then evaluate the threshold chosen for an 80% recall target on the test set.

**Accuracy** is the primary metric for model comparison. The notebook also reports precision, recall, specificity, F1, ROC-AUC, average precision, Brier score, and confusion matrices.

## Key results

| Selected model | Test accuracy | Test recall | Test ROC-AUC |
| --- | ---: | ---: | ---: |
| Decision tree | 73.44% | 69.35% | 0.7991 |
| Neural network | 73.97% | 69.91% | 0.8054 |

With a threshold selected on the validation set to target at least 80% recall, the neural network achieves **81.21% recall** and **71.99% accuracy** on the test set. Compared with the original 0.5 threshold, false negatives fall from 1,587 to 991, while false positives increase by 804. This illustrates the trade-off between detecting more positive cases and generating more false alarms.

These results apply to the dataset and split used in the notebook. Evaluation on independent data would be needed to assess how well the models generalize to other settings.
