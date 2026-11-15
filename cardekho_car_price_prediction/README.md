```markdown
# CarDekho Used Car Price Prediction

This project focuses on predicting the selling price of used cars based on various features such as vehicle age, kilometers driven, mileage, engine capacity, power, fuel type, transmission type, and seller type using the CarDekho dataset.

## 📋 Table of Contents
- [Dataset Information](#dataset-information)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Feature Engineering & Preprocessing](#feature-engineering--preprocessing)
- [Model Development & Evaluation](#model-development--evaluation)
- [Hyperparameter Tuning](#hyperparameter-tuning)
- [Inference](#inference)
- [Saved Artifacts](#saved-artifacts)

---

## 📊 Dataset Information
- **Size:** 15,411 rows and 13 columns.
- **Target Variable:** `selling_price`
- **Features:**
  - **Numerical (7):** `vehicle_age`, `km_driven`, `mileage`, `engine`, `max_power`, `seats`, `selling_price`
  - **Categorical (6):** `car_name`, `brand`, `model`, `seller_type`, `fuel_type`, `transmission_type`

---

## 🔍 Exploratory Data Analysis (EDA)
- **Target Distribution:** The `selling_price` is right-skewed, with most used cars priced under 3 million (INR).
- **Top Brands & Models:** Maruti and Hyundai are the most frequently sold brands. The Hyundai i20 and Maruti Swift Dzire are the top sold car models.
- **Correlation:** Strong positive correlations exist between `selling_price` and both `max_power` (~0.75) and `engine` (~0.59). There is a negative correlation with `vehicle_age` (~ -0.24).
- **Categorical Relations:** 
  - **Transmission:** Automatic transmission cars command a higher price compared to manuals.
  - **Fuel Type:** Electric and Diesel cars tend to have higher median selling prices than Petrol, CNG, and LPG cars.
  - **Seller Type:** Dealer listings have a slightly higher median selling price compared to individual sellers.

---

## ⚙️ Feature Engineering & Preprocessing
1. **Feature Drop:** High-cardinality columns (`car_name`, `brand`, `model`) were removed from the modeling pipeline.
2. **Categorical Encoding:** One-Hot Encoding (`pd.get_dummies`) was applied to convert categorical variables (`seller_type`, `fuel_type`, `transmission_type`) into numerical format.
3. **Train-Test Split:** Splitted into 80% training and 20% testing sets.

---

## 🤖 Model Development & Evaluation
Several regression algorithms were trained and evaluated on the test set:

| Model | RMSE | $R^2$ Score |
| :--- | :--- | :--- |
| **RandomForestRegressor** | ~210,981 | ~93.59% |
| **ExtraTreesRegressor** | ~215,498 | ~93.31% |
| **DecisionTreeRegressor** | ~348,231 | ~82.53% |
| **LinearRegression** | ~467,543 | ~68.51% |
| **Ridge** | ~467,539 | ~68.51% |
| **Lasso** | ~467,542 | ~68.51% |
| **SVR** | ~860,776 | -6.74% |

---

## ⚡ Hyperparameter Tuning
Hyperparameter tuning was performed on the **RandomForestRegressor** using `RandomizedSearchCV` over 3 folds (100 iterations):
- **Best Parameters:** `{'n_estimators': 300, 'min_samples_split': 2, 'min_samples_leaf': 1, 'max_features': 'log2', 'max_depth': 30, 'bootstrap': True}`
- **Tuned Model Performance:**
  - **Root Mean Squared Error (RMSE):** ~201,153
  - **$R^2$ Score:** **~94.17%**

---

## 💾 Saved Artifacts
All trained model files are saved as pickles:
- `LinearRegression_model.pkl`
- `Ridge_model.pkl`
- `Lasso_model.pkl`
- `SVR_model.pkl`
- `DecisionTreeRegressor_model.pkl`
- `ExtraTreesRegressor_model.pkl`
- `RandomForestRegressor_model.pkl`
- `best_model.pkl` (The tuned RandomForest model)

---

## 🚀 Inference Example
To load the best model and make a prediction on a sample data array:
```python
import pickle

# Load the best model
with open('best_model.pkl', 'rb') as file:
    model = pickle.load(file)

# Sample array format: [vehicle_age, km_driven, mileage, engine, max_power, seats, seller_type_Dealer, seller_type_Individual, seller_type_Trustmark Dealer, fuel_type_CNG, fuel_type_Diesel, fuel_type_Electric, fuel_type_LPG, fuel_type_Petrol, transmission_type_Automatic, transmission_type_Manual]
sample_features = [10, 108800, 12.80, 2494, 102.0, 7, 1.0, 0.0, 0.0, 0.0, 1.0, 0.0, 0.0, 0.0, 0.0, 1.0]

predicted_price = model.predict([sample_features])
print(f"Predicted Selling Price: {predicted_price[0]:.2f}")
```
```