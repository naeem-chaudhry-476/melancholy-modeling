# User Manual

## Introduction

This project builds and evaluates machine learning models to predict depression risk using survey data from the National Health and Nutrition Examination Survey (NHANES). The workflow combines data preparation, feature engineering, model training, model comparison, and final model selection.

The repository includes:

- Data extraction and cleaning notebooks
- Exploratory data analysis and feature engineering steps
- Multiple model training notebooks for CatBoost, XGBoost, SVM, Random Forest, and Logistic Regression
- A final comparison notebook and saved model artifacts
- A selected final model for inference and evaluation

This manual is intended for researchers, students, and developers who want to reproduce the pipeline, retrain models, and apply the final model to new data.

---

## Project Overview

The project follows the steps below:

1. Map raw NHANES files into a consistent dataset
2. Clean and merge relevant survey columns
3. Explore variable distributions and relationships
4. Engineer features for modeling
5. Train and validate multiple model families
6. Compare model performance on the required metrics
7. Save the best-performing model for deployment or further analysis

### Selected Model Summary

The final project model is a CatBoost classifier trained on the prepared NHANES feature set. The model card indicates the chosen configuration was selected based on test PR-AUC and recall, with the final model saved in the `Models` directory.

---

## Repository Structure

| Folder / File | Purpose |
| --- | --- |
| `1.RawDataMapping.ipynb` | Maps raw NHANES files into usable tables |
| `2.DataWrangling.ipynb` | Cleans and merges data |
| `3.EDA.ipynb` | Exploratory analysis and data review |
| `4.FeatureEngineering.ipynb` | Creates modeling-ready feature sets |
| `5.Modeling_CatBoost.ipynb` | CatBoost model training and tuning|
| `5.Modeling_rf_logreg.ipynb` | Logistic Regression and Random Forest models training and tuning|
| `5.Modeling_svm.ipynb` | SVM model training and tuning |
| `5.Modeling_XGBoost.ipynb` | XGBoost model training and tuning|
| `6.ModelComparison.ipynb` | Compares model results and selects final model |
| `Nhanes_RawData/` | Raw NHANES `.xpt` datasets |
| `DataSplit/` | Train/validation/test split CSV files |
| `Models/` | Saved model objects and evaluation summaries |
| `Figures/` | Generated plots and visual outputs |
| `docs/` | Supporting documentation or preview outputs |

---

## Prerequisites

Before running the project, ensure the following are installed and available:

### Required Software

- Python 3.10 or newer
- Jupyter Notebook or JupyterLab
- Git
- A terminal or command prompt

### Required Python Packages

The notebooks rely on the following libraries:

- pandas
- numpy
- matplotlib
- seaborn
- scikit-learn
- xgboost
- catboost
- joblib
- scipy

You can install the main dependencies with:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost catboost joblib scipy
```

### Recommended Setup

- Use a virtual environment for dependency isolation
- Run the notebooks in the project root folder
- Ensure the path structure remains unchanged so the notebooks can locate raw and split data files correctly

---

## Installation Guide

### 1. Clone the Repository

```bash
git clone <repository-url>
cd melancholy-modeling-main
```

### 2. Create a Virtual Environment

On Windows:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

On macOS/Linux:

```bash
python -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install --upgrade pip
pip install pandas numpy matplotlib seaborn scikit-learn xgboost catboost joblib scipy
```

### 4. Launch Jupyter

```bash
jupyter notebook
```

or

```bash
jupyter lab
```

### 5. Confirm Project Setup

Open the project root and check that the following folders exist:

- `Nhanes_RawData/`
- `DataSplit/`
- `Models/`
- `Figures/`

If those directories are missing or empty, verify that the repository was downloaded completely and that the underlying data files were not excluded by your environment.

---

## Step-by-Step Usage

The recommended workflow is to run the notebooks in order. Each notebook builds on the previous stage.

### Step 1: Prepare the Raw NHANES Data

Open and run `1.RawDataMapping.ipynb`.

This notebook maps the raw NHANES data files into a more structured format. It prepares the project inputs for cleaning and feature selection.

### Step 2: Clean and Merge Data

Open and run `2.DataWrangling.ipynb`.

This step typically:

- Filters incomplete or inconsistent records
- Combines relevant questionnaire and demographic tables
- Removes duplicated or irrelevant columns
- Creates the base tabular dataset used for modeling

### Step 3: Explore the Dataset

Open and run `3.EDA.ipynb`.

This notebook is useful for understanding:

- feature distributions
- missing values
- class imbalance
- relationships between predictors and the depression flag

### Step 4: Create Model-Ready Features

Open and run `4.FeatureEngineering.ipynb`.

This stage creates the train/validation/test splits that are later used by the model notebooks. The project appears to generate dataset variants such as:

- CatBoost-friendly categorical data
- One-hot encoded data for tree-based and linear models
- Standardized/scaled data when needed by other algorithms (SVM, Logistic Regression)

Typical output files in `DataSplit/` include:

- `Nhanes_Cat_Train.csv`
- `Nhanes_Cat_Val.csv`
- `Nhanes_Cat_Test.csv`
- `Nhanes_XGB_Forest_Train.csv`
- `Nhanes_XGB_Forest_Val.csv`
- `Nhanes_XGB_Forest_Test.csv`
- `Nhanes_SVM_Logistic_Train.csv`
- `Nhanes_SVM_Logistic_Val.csv`
- `Nhanes_SVM_Logistic_Test.csv`

### Step 5: Train the Models

Run the modeling notebooks in this order:

1. `5.Modeling_rf_logreg.ipynb`
2. `5.Modeling_svm.ipynb`
3. `5.Modeling_XGBoost.ipynb`
4. `5.Modeling_CatBoost.ipynb`

Each notebook:

- loads the relevant split files
- trains a model family on the training set
- tunes or validates parameters on the validation set
- saves the trained model artifact in the `Models/` folder
- records model metrics for comparison

### Step 6: Compare the Results

Open and run `6.ModelComparison.ipynb`.

This final notebook:

- loads all saved models
- evaluates them on the same test set
- compares required metrics such as accuracy, precision, recall, and F1
- visualizes results and model performance
- identifies the final selected model

### Step 7: Use the Final Model

The final selected model is saved in `Models/final_model.joblib` and is intended for reuse. To load and run inference in Python:

```python
import joblib
import pandas as pd

model_path = "Models/final_model.joblib"
model = joblib.load(model_path)

# Example: new observation dataframe with same feature names used during training
new_data = pd.DataFrame([
    {
        "Age": 42,
        "Gender": "Female",
        "Education": "College graduate",
        "Income": "$35,000-$54,999",
        # include all required modeled features here
    }
])

prediction = model.predict_proba(new_data)[0, 1]
print(f"Predicted depression risk: {prediction:.4f}")
```

> Important: your feature columns must match the exact fields used during training. Reusing a saved model without matching feature names and dtypes may produce errors or invalid predictions.

---

## Recommended Execution Sequence

```bash
# 1) Start Jupyter
jupyter notebook

# 2) Run notebooks in order
# 1.RawDataMapping.ipynb
# 2.DataWrangling.ipynb
# 3.EDA.ipynb
# 4.FeatureEngineering.ipynb
# 5.Modeling_rf_logreg.ipynb
# 5.Modeling_svm.ipynb
# 5.Modeling_XGBoost.ipynb
# 5.Modeling_CatBoost.ipynb
# 6.ModelComparison.ipynb
```

---

## Expected Outputs

After a successful run, you should see outputs in the following places:

| Output Location | Description |
| --- | --- |
| `DataSplit/` | Prepared train/validation/test CSV files |
| `Models/` | Trained model files and summary CSVs |
| `Figures/` | Visualizations and plots |
| `Models/final_model.joblib` | Final selected model for downstream use |
| `Models/final_model_comparison.csv` | Comparison metrics across models |
| `Models/model_scoreboard.csv` | Shared model scoreboard record |

---

## Troubleshooting

### Issue 1: `ModuleNotFoundError` or package import failure

Symptoms:

- `No module named pandas`
- `No module named xgboost`
- `No module named catboost`

Solution:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost catboost joblib scipy
```

If you are using a virtual environment, ensure it is activated before installing and running the notebooks.

---

### Issue 2: File not found errors

Symptoms:

- `FileNotFoundError`
- missing CSV files or raw data files
- notebook cannot locate `Nhanes_RawData` or `DataSplit`

Solution:

- Confirm the project root contains the expected folders
- Check that the notebooks are run from the repository root
- Verify that no files were moved or renamed
- Re-download the raw `.xpt` files if they were removed or not yet extracted

---

### Issue 3: Data split mismatch or wrong feature columns

Symptoms:

- model training fails with column mismatch
- new data does not match training columns
- predictions cannot be generated with the saved model

Solution:

- Use the same feature names that were used for training
- Recreate the feature matrix with the same encodings and transformations
- Recheck the exact dataset used by the modeling notebooks
- Ensure categorical values match the expected strings or categories

---

### Issue 4: CatBoost categorical encoding errors

Symptoms:

- categorical column type issues
- unexpected conversion errors during model training
- values treated as numeric instead of category labels

Solution:

- Confirm the data used for CatBoost is the categorical dataset created in `4.FeatureEngineering.ipynb`
- Ensure categorical variables are converted to strings when required
- Check that the selected columns match the model’s expected schema

---

### Issue 5: Low or unstable model performance

Symptoms:

- recall or precision is lower than expected
- model appears biased toward majority class
- class imbalance creates poor metric behavior

Solution:

- Review the class distribution in the EDA notebook
- Check whether the target flag is imbalanced
- Use validation tuning rather than relying on raw defaults
- Review the final model card for the threshold and metric logic used by the project

---

### Issue 6: Jupyter kernel cannot start

Symptoms:

- notebook fails to open
- kernel crashes
- package environment not recognized

Solution:

```bash
python -m ipykernel install --user --name myenv
jupyter kernelspec list
```

Then select the kernel matching your active environment in Jupyter.

---

## Best Practices

- Always run notebooks in order from raw data to model comparison
- Keep the repository structure intact
- Use the same Python environment for training and inference
- Save model artifacts only after validation has completed
- Document any new feature engineering steps before retraining
- Verify all required columns before using a saved model for predictions

---

## Support and Notes

This project is intended as a research and coursework modeling workflow. It is not a clinical diagnosis tool. The final model is best treated as a screening aid based on survey-derived features and should be interpreted cautiously.

The selected model summary in the repository indicates that CatBoost was chosen as the final model based on the project’s ranking metric and validation strategy. For project-specific metric details, review the saved model card and comparison outputs in the `Models/` directory.

---

## Quick Start Summary

```bash
git clone <repository-url>
cd melancholy-modeling-main
python -m venv .venv
.venv\Scripts\Activate.ps1  # Windows
pip install pandas numpy matplotlib seaborn scikit-learn xgboost catboost joblib scipy
jupyter notebook
```

Then run:

1. `1.RawDataMapping.ipynb`
2. `2.DataWrangling.ipynb`
3. `3.EDA.ipynb`
4. `4.FeatureEngineering.ipynb`
5. `5.Modeling_rf_logreg.ipynb`
6. `5.Modeling_svm.ipynb`
7. `5.Modeling_XGBoost.ipynb`
8. `5.Modeling_CatBoost.ipynb`
9. `6.ModelComparison.ipynb`

Finally, use the saved model from `Models/final_model.joblib` for prediction or benchmarking.
