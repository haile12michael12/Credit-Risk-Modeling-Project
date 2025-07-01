
#  Credit Risk Modeling Project

This project is a complete end-to-end credit risk modeling pipeline that includes data ingestion, EDA, feature engineering, proxy target labeling, machine learning model training, evaluation, deployment via API, and CI/CD automation.



## 📁 Project Structure
- credit-risk-modeling-project/
- ├── .github/workflows/ci.yml # CI/CD GitHub Actions Workflow
- ├── data/ # Git-ignored data directory
- │ ├── raw/ # Raw data
- │ └── processed/ # Processed, model-ready data
- ├── notebooks/
- │ └── 1.0-eda.ipynb # Exploratory analysis notebook
- ├── src/
- │ ├── init.py
- │ ├── data_processing.py # Feature engineering logic
- │ ├── train.py # Model training script
- │ ├── predict.py # Inference script
- │ └── api/
- │ ├── main.py # FastAPI app with prediction endpoint
- │ └── pydantic_models.py # Pydantic schemas for request/response
- ├── tests/
- │ └── test_data_processing.py # Unit tests
- ├── requirements.txt
- ├── .gitignore
- └── README.md


##  Credit Scoring Business Understanding

### 1. Basel II and the Need for Interpretability
The Basel II Accord requires financial institutions to assess and document their risk exposures more rigorously. This demands interpretable models that are explainable, auditable, and compliant. Models like logistic regression with WoE help institutions meet regulatory expectations due to their clarity and transparency.

### 2. Why Use a Proxy Variable?
In the absence of a direct “default” label, we construct a proxy variable to simulate credit risk (e.g., disengaged or low-value customers). However, proxy labels may introduce **bias** and **label noise**, leading to model misinterpretation and regulatory or business risk.

### 3. Trade-Offs: Simplicity vs. Performance
| Metric                | Interpretable Model (e.g., Logistic Regression) | Complex Model (e.g., GBM)         |
|----------------------|-----------------------------------------------|----------------------------------|
| Explainability       | High                                          | Low to Medium (needs SHAP/LIME) |
| Regulatory Fit       | Strong                                        | Needs justification              |
| Performance          | Moderate                                      | High                             |
| Deployment Risk      | Low                                           | High                             |



## 📊 Task 2 – Exploratory Data Analysis (EDA)

**Goals:**
- Understand data structure, completeness, and distributions.
- Detect outliers and relationships between features.
- Guide feature engineering.




##  Task 3 – Feature Engineering

All logic implemented in `src/data_processing.py` using `sklearn.pipeline.Pipeline`.

### Transformations:
- **Aggregate Features:** Total/Avg/Count/StdDev of transaction amounts.
- **Datetime Extraction:** Hour, Day, Month, Year from timestamps.
- **Categorical Encoding:** One-Hot & Label Encoding.
- **Missing Value Handling:** Mean/Median/KNN imputation.
- **Scaling:** Normalization [0,1] or Standardization (Z-score).
- **WOE/IV**: Optional use of `xverse` and `woe` for monotonic binning and predictive strength evaluation.


##  Task 4 – Proxy Target Variable Engineering

### Approach:
- **RFM Scoring:** Calculate Recency, Frequency, and Monetary values.
- **Clustering:** Segment users via K-Means (k=3) on scaled RFM.
- **Target Definition:** Label the lowest value cluster as `is_high_risk = 1`.

Final dataset includes this binary target for supervised learning.



## Task 5 – Model Training and Tracking

All logic in `src/train.py` with experiment tracking using `mlflow`.

### Models Used:
- Logistic Regression
- Random Forest
- Gradient Boosting Machines

### Techniques:
- Data split (train/test)
- GridSearchCV for hyperparameter tuning
- Metrics: Accuracy, Precision, Recall, F1, ROC-AUC

### Model Registry:
Best model registered in **MLflow Model Registry**.

### Testing:
Basic unit tests defined in `tests/test_data_processing.py`.



## 🚀 Task 6 – Deployment & CI/CD

### 🔧 API Deployment
- Built using **FastAPI** (`src/api/main.py`)
- `/predict` endpoint returns default probability
- Pydantic models ensure schema validation


### 🔁 CI/CD Pipeline
Using **GitHub Actions** via `.github/workflows/ci.yml`:
- **Linting** via `flake8`
- **Testing** via `pytest`
- Fails build on errors

## Prerequisites

- Python 3.x
- Jupyter Notebook or JupyterLab
- Required Python libraries (see below)

## Setup

To set up the environment for this notebook, follow these steps:

1. **Clone the Repository:**
   
   git clone https://github.com/ElbetelTaye/Credit-Scoring-Model-.git
 

2. **Install Required Libraries:**

   Make sure to install the required libraries by running:
   
   pip install -r requirements.txt
   

3. **Start Jupyter Notebook:**

   Launch the Jupyter Notebook:
   
   jupyter notebook
   

## Contributing

If you would like to contribute to this project:

1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Commit your changes (`git commit -am 'Add new feature'`).
4. Push to the branch (`git push origin feature-branch`).
5. Create a new Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
