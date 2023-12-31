# DAV 6150 Module 4 Assignment: Feature Selection & Dimensionality Reduction

## Author: Pratibha Bhalerao

---

## Introduction

Welcome to the DAV 6150 Module 4 Assignment, where we will explore feature selection and dimensionality reduction techniques for predicting the number of shares of online news articles. This README provides an overview of the project, the steps involved, and the key findings.

### Goal

The primary objective of this analysis is to identify the most influential explanatory variables to be included within a linear regression model that predicts the number of shares an online news article will receive. To achieve this, we will:

1. Conduct Exploratory Data Analysis (EDA) to understand the dataset.
2. Apply feature selection and dimensionality reduction techniques.
3. Build a linear regression model for prediction.

Let's get started!

---

## Data

We begin by loading the dataset, which is available at the following URL:

```python
df = pd.read_csv("https://raw.githubusercontent.com/Pratibhabhalerao080/DAV-6150/main/M4_Data.csv")
```

### Exploratory Data Analysis (EDA)

To understand our data better, we perform the following:

1. Check dataset information:
   ```python
   df.info()
   ```

2. Generate summary statistics for numerical columns:
   ```python
   df.describe()
   ```

3. Examine missing values:
   ```python
   df.isnull().sum()
   ```

4. Explore the distribution of shares:
   ```python
   plt.figure(figsize=(8, 4))
   sns.histplot(df[' shares'], bins=30, kde=True)
   plt.title('Distribution of Shares')
   plt.xlabel('Shares')
   plt.ylabel('Frequency')
   plt.show()
   ```

5. Visualize the relationship between categorical variables and shares using box plots:
   ```python
   plt.figure(figsize=(10, 6))
   sns.boxplot(x=' n_tokens_title', y=' shares', data=df)
   plt.title('Box Plot of Title Length vs. Shares')
   plt.xlabel('Number of Tokens in Title')
   plt.ylabel('Shares')
   plt.xticks(rotation=90)
   plt.show()
   ```

Our EDA highlights the distribution of shares, potential outliers, and relationships between variables, providing insights into our data.

### Feature Selection / Dimensionality Reduction

For feature selection, we use correlation analysis to identify relevant features:

```python
correlation_with_target = df.corr()[' shares'].abs().sort values(ascending=False)
```

We then select the top 10 features based on their correlation with shares.

For dimensionality reduction, we apply Principal Component Analysis (PCA) to the selected features:

```python
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

X_selected = df[selected_features]

# Standardize the selected features
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X_selected)

# Apply PCA with a suitable number of components
n_components = 8
pca = PCA(n_components=n_components)
principal_components = pca.fit_transform(X_scaled)

# Create a DataFrame with the principal components
df_pca = pd.DataFrame(data=principal_components, columns=[f'PC{i}' for i in range(1, n_components + 1)])
```

We also examine the explained variance ratio provided by PCA to understand how much information is retained by each component.

### Regression Model Evaluation

We evaluate a linear regression model using the selected features:

```python
# Split the data into features (X) and the target variable (y)
X = df[selected_features]
y = df[' shares']

# Split the data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Initialize the linear regression model
model = LinearRegression()

# Train the model on the training data
model.fit(X_train, y_train)

# Make predictions on the test data
y_pred = model.predict(X_test)

# Evaluate the model's performance
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)
```

The results of our linear regression model evaluation are as follows:

- Mean Squared Error (MSE): 117,772,231.55
- R-squared (R2) Score: 0.024

---

## Conclusion

In summary, our analysis involved extensive data exploration, feature selection, dimensionality reduction using PCA, and linear regression modeling. Despite these efforts, the model's performance exhibited a high Mean Squared Error (MSE) and a low R-squared (R2) Score, indicating that it struggled to predict the number of shares effectively.

This outcome may be attributed to the inherent complexity of predicting social media shares, the potential need for additional features, or the presence of non-linear relationships. Further refinement and exploration are necessary to improve the predictive power of the model.

Thank you for exploring this analysis, and feel free to review the code and results for more details.﻿# Feature_Selection_Dimentinality_Reduction
