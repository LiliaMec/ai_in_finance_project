# Machine Learning Project Guidelines  
*Course: Introduction to Machine Learning*

---

# 1. Group Formation

- Projects must be completed in **groups of 3–4 students**.
- Each student must **actively contribute** to the project.
- **Every group member must present** during the final presentation.
- Please fill in the **group registration sheet** once your group is formed.

**Group sheet:**  
👉 INSERT LINK TO GROUP SHEET HERE

---

# 2. Project Timeline

### Today
- Form your **group (3–4 students)**.
- Choose a **project topic**.
- Start exploring the **dataset**.

### Next sessions
You should already:
- Understand the **dataset structure**
- Have performed **initial exploratory analysis**
- Have identified **possible models**

### Final session
- **Project presentations**
- **Q&A discussion**

---

# 3. Presentation Requirements

### Presentation duration
- **8–10 minutes presentation**
- **+ 3–5 minutes questions**

### Slides
- Maximum **10 slides**

### Suggested Slide Structure

1. **Introduction**
   - What problem are you solving?
   - Why is it interesting?

2. **Dataset**
   - Source of the data
   - Number of observations
   - Variables description

3. **Data Exploration**
   - Distributions
   - Correlations
   - Missing values
   - Key observations

4. **Feature Engineering**
   - Created variables
   - Transformations
   - Justification

5. **Modeling Approach**
   - Models tested
   - Training procedure
   - Hyperparameters

6. **Evaluation**
   - Metrics used
   - Performance comparison

7. **Results**
   - Model performance
   - Visualizations

8. **Discussion**
   - What worked
   - What did not work

9. **Conclusion**
   - Key insights
   - Possible improvements

---

# 4. Project Workflow (Recommended)

Students should follow this workflow when working on their project.

---

## Step 1 — Problem Definition

Clearly define:

- What you want to **predict or explain**
- What are the **input variables**
- What is the **target variable**

Examples:

| Task | Example |
|-----|------|
| Classification | Predict default of a borrower |
| Regression | Predict stock return |
| Time Series | Predict interest rates |
| NLP | Predict market movement from news |

---

## Step 2 — Data Exploration

Before building models, you must **understand the dataset**.

Recommended steps:

- Inspect variables
- Plot distributions
- Check correlations
- Identify missing values
- Detect anomalies or outliers

Example tools:

```python
df.describe()
df.info()
sns.histplot()
sns.heatmap()
```

Questions you should answer:

- Are there missing values?
- Are variables normally distributed?
- Are some variables strongly correlated?

---

## Step 3 — Data Preprocessing

Typical preprocessing tasks:

### Missing values
- Remove rows
- Imputation (mean, median, model-based)

### Feature scaling

Common methods:

- Standardization
- Min-max scaling

### Encoding categorical variables

- One-hot encoding
- Label encoding

### Train/Test split

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
```

---

## Step 4 — Feature Engineering

Feature engineering often **improves performance more than complex models**.

Examples:

### Financial features
- Returns
- Volatility
- Moving averages

### Time series features
- Lag variables
- Rolling statistics

### Text features
- TF-IDF
- Word embeddings

Example:

```python
df["return"] = df["price"].pct_change()
df["ma_5"] = df["price"].rolling(5).mean()
```

---

## Step 5 — Modeling

You are encouraged to **test multiple models**.

### Regression models

- Linear Regression
- Ridge / Lasso

### Classification models

- Logistic Regression
- Random Forest
- Gradient Boosting
- XGBoost

### Neural networks

- MLP
- LSTM (for time series)
- Transformers (for NLP)

Example:

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier()
model.fit(X_train, y_train)
```

---

## Step 6 — Model Evaluation

Choose evaluation metrics depending on the task.

### Classification

| Metric | Meaning |
|------|------|
| Accuracy | Overall correctness |
| Precision | Correct positive predictions |
| Recall | Detection ability |
| F1-score | Precision/Recall balance |
| AUC | Ranking performance |

### Regression

| Metric | Meaning |
|------|------|
| RMSE | Error magnitude |
| MAE | Absolute error |
| R² | Explained variance |

Example:

```python
from sklearn.metrics import accuracy_score

accuracy_score(y_test, predictions)
```

---

## Step 7 — Model Interpretation

Understanding the model is **very important**.

You should attempt to analyze:

- Feature importance
- Model sensitivity
- Economic interpretation

Useful tools:

- SHAP
- LIME
- Feature importance plots

Example:

```python
model.feature_importances_
```

---

# 5. Suggested Project Topics

You may choose one of the following topics.

---

# Project 1 — Commodity Trade Data for Macroeconomic Forecasting

### Objective

Use international trade data to predict macroeconomic indicators.

### Dataset

UN Comtrade Database  
https://comtrade.un.org/

### Possible targets

- GDP growth
- CPI inflation
- Industrial production

### Example features

- Export growth
- Import growth
- Trade balance
- Commodity group aggregation

### Possible models

- Linear regression
- Random forest
- Gradient boosting
- LSTM

---

# Project 2 — Yield Curve Prediction

### Objective

Predict future yield curve factors.

### Dataset

FRED Treasury Yield Data  
https://fred.stlouisfed.org/

### Tasks

- Build yield curve factors:
  - Level
  - Slope
  - Curvature
- Use historical yields as features.

### Models

- Random forest
- Gradient boosting
- LSTM

---

# Project 3 — Credit Risk Modeling

### Objective

Predict probability of borrower default.

### Dataset

UCI Default of Credit Card Clients  

https://archive.ics.uci.edu/ml/datasets/default+of+credit+card+clients

or

https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset

### Tasks

- Handle class imbalance
- Train classification models

### Models

- Logistic regression
- Random forest
- XGBoost
- Neural networks

### Evaluation

- AUC
- F1-score
- Confusion matrix

---

# Project 4 — News-Based Stock Market Prediction

### Objective

Use financial news to predict stock market movement.

### Dataset

Daily News for Stock Market Prediction  

https://www.kaggle.com/datasets/aaron7sun/stocknews

### Tasks

- Preprocess text
- Extract embeddings
- Combine with financial data

### Models

- Logistic regression
- LSTM
- Transformers

---

# Project 5 — Machine Learning for Portfolio Allocation

### Objective

Use machine learning predictions to construct portfolios.

### Dataset

Stock Market Dataset  

https://www.kaggle.com/datasets/jacksoncrow/stock-market-dataset

### Tasks

- Predict returns or volatility
- Build portfolio optimization

### Evaluation

- Sharpe ratio
- Maximum drawdown
- Risk-adjusted return

---

# 6. Expected Deliverables

Each group must submit:

### Presentation slides
- Maximum **10 slides** :
    - Problem description
    - Dataset description
    - Methodology
    - Results
    - Discussion

### Code
- Jupyter notebook (.ipynb)
- or Python scripts (.py)


---

# 7. Recommended Tools

Students are encouraged to use:

- Python
- Pandas
- NumPy
- Scikit-learn
- PyTorch / TensorFlow
- Matplotlib
- Seaborn

---

# 8. Tips for a Successful Project

Start **simple**:

1. Build a **baseline model**
2. Improve with **feature engineering**
3. Compare **multiple models**
4. Interpret the **results**

Good projects:

- Explain the **data clearly**
- Compare **several models**
- Provide **clear visualizations**
- Draw **meaningful conclusions**

---

# 9. Academic Integrity

- Each group must submit **original work**.
- External resources are allowed but **must be cited**.
- Collaboration **between groups is not allowed**.

---

# 10. Getting Help

If you encounter difficulties:

- Ask questions during class sessions
- Discuss modeling choices
- Seek help early when problems arise

The project sessions are designed to **assist you in dataset exploration, modeling choices, and interpretation**.