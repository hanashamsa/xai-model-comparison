

# Explainable AI - Model Explanations
In this, you are challenged to explain a model. For this, you will research exisiting approaches and apply them to your model and interpret the results.


As a dataset used:

https://www.kaggle.com/datasets/jockeroika/life-style-data

Based on all the features of the data we try to predict calorie consumption.


## Report

## 1. Project Overview

The goal of this project is to **explain machine learning models** using multiple explainability approaches.
We focus on understanding **how and why models predict calorie consumption during workout sessions**, and we interpret their behavior using both **global and local explainability methods**.



## 2. Dataset Description

We use the **Lifestyle Data** dataset from Kaggle:
[https://www.kaggle.com/datasets/jockeroika/life-style-data](https://www.kaggle.com/datasets/jockeroika/life-style-data)

The dataset is tabular and contains features related to lifestyle, fitness, and nutrition.

### Feature Groups

* **Demographics:** Age, Gender, Height (m), Weight (kg), BMI
* **Workout behavior:** Session_Duration (hours), Workout_Type (HIIT, Yoga, Cardio, Strength), Workout_Frequency (days/week), Experience_Level
* **Physiological metrics:** Avg_BPM, Max_BPM, Resting_BPM
* **Nutrition:** Calories intake, carbohydrates, proteins, fats, sugar, sodium
* **Exercise metadata:** Sets, Reps, Calories burned per 30 minutes

### Target Variable

* **Calories_Burned** – total calories burned during a workout session.

This dataset is well suited for explainability analysis because the features are interpretable and have clear real-world meaning.



## 3. Modeling Task

The task is formulated as a **supervised regression problem**:

> Predicting **Calories_Burned** based on demographic, workout, physiological, and nutritional features.



## 4. Models Used

### 4.1 Primary Model – XGBoost Regressor

The primary model used in this project is an **XGBoost Regressor**, chosen because:

* It performs exceptionally well on tabular data
* It captures non-linear relationships
* It is widely studied in explainability literature

A preprocessing pipeline is applied:

* Numeric features are passed directly
* Categorical features are one-hot encoded
* Preprocessing and model are combined in a single pipeline

#### Performance (Test Set)

* **RMSE ≈ 7.6**
* **R² ≈ 0.999**



### 4.2 Comparison Model – Random Forest Regressor

To validate the robustness of the explanations, we additionally trained a **Random Forest Regressor** using the same dataset and preprocessing pipeline.

#### Performance (Test Set)

* **RMSE ≈ 2.7**
* **R² ≈ 0.9999**

Since both models achieve extremely high accuracy, the focus of this project is on **explainability behavior rather than predictive performance alone**.



## 5. Explainability Approaches

We apply **four explainability approaches**, covering both **global and local explanations**, following the explainability space described by Hohman et al.



### 5.1 SHAP (SHapley Additive exPlanations)

**Summary**
SHAP explains predictions by computing Shapley values from cooperative game theory. It provides both global and local explanations.

**Interpretation**

* SHAP summary plots identify **Session_Duration (hours)** as the most influential feature
* **Workout_Type** and **Experience_Level** strongly impact predictions
* XGBoost produces sharper and more concentrated feature attributions
* Random Forest shows smoother and more evenly distributed contributions

SHAP confirms that both models rely on meaningful workout-related features.



### 5.2 Partial Dependence Plots (PDP)

**Summary**
Partial Dependence Plots visualize the marginal effect of selected features on model predictions, averaged over all other features.

**Interpretation**
PDPs were generated for:

* **Session_Duration (hours)**
* **Workout_Type**
* **Experience_Level**

Results show that:

* Longer workout sessions strongly increase predicted calories burned
* Workout type introduces step-wise differences reflecting categorical exercise effects
* Experience level has a positive but more moderate influence

Both models exhibit similar trends, with **Random Forest producing smoother curves** and **XGBoost showing sharper transitions**.

PDPs provide a global view of how key workout-related features affect calorie expenditure.



### 5.3 LIME (Local Interpretable Model-Agnostic Explanations)

**Summary**
LIME explains individual predictions by fitting a simple local model around a single instance.

**Interpretation**

* LIME highlights local contributors such as session duration, repetitions, and exercise intensity
* For XGBoost, fewer features dominate with stronger influence
* For Random Forest, feature contributions are more balanced

LIME complements global methods by explaining **why a specific prediction was made**.



### 5.4 Surrogate Decision Tree (Global Approximation)

**Summary**
A shallow **Decision Tree Regressor (max depth = 3)** is trained as a surrogate model to approximate XGBoost predictions.

**Interpretation**

* The surrogate tree reveals **Session_Duration** as the primary decision factor
* **Workout_Type** appears as a secondary split
* The model logic becomes human-readable

Although the surrogate does not match XGBoost’s accuracy, it provides a simplified global explanation of its decision logic.



## 6. Model Comparison and Interpretation

Across all explainability methods, both models consistently identify:

* **Session_Duration (hours)** as the dominant predictor
* **Workout_Type** as a major categorical driver
* **Experience_Level** as a secondary but stable factor

### Comparison Summary

| Aspect                      | XGBoost                                  | Random Forest                             |
| --------------------------- | ---------------------------------------- | ----------------------------------------- |
| How feature effects appear  | Strong influence from a few key features | More balanced influence across features   |
| Reaction to extreme values  | More sensitive to extreme feature values | Less sensitive, more stable               |
| Global explanation behavior | Consistent across methods                | Consistent across methods                 |
| Local explanation behavior  | Explanations can vary more per instance  | Explanations are more stable per instance |

Interpretation:
Although XGBoost and Random Forest use different ensemble strategies, both models consistently identify the same key features. This agreement increases confidence that the explanations reflect real patterns in the data rather than artifacts of a specific model.




## 7. Conclusion

This project shows how ensemble-based machine learning models can be made transparent and understandable through the use of complementary explainability techniques. By applying **SHAP, Partial Dependence Plots, LIME, and a surrogate decision tree**, we were able to analyze the models from both **global and local perspectives** and gain meaningful insights into their decision-making processes.

Across all explanation methods and both models, **Session Duration** consistently emerged as the most influential factor in predicting calories burned, followed by **Workout Type** and **Experience Level**. The strong agreement between XGBoost and Random Forest confirms that these findings are robust and not dependent on a single model or explanation method.

Overall, this work demonstrates that combining multiple explainability approaches provides a more complete and trustworthy understanding of model behavior, supporting the development of **reliable, interpretable, and responsible AI systems**.



## References

* Lundberg, S. M., & Lee, S.-I. (2017). *A Unified Approach to Interpreting Model Predictions*
* Ribeiro, M. T., Singh, S., & Guestrin, C. (2016). *Why Should I Trust You?*
* Hohman, F., et al. (2018). *Visual Analytics in Deep Learning*


