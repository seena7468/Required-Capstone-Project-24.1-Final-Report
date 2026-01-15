# Required-Capstone-Project-24.1-Final-Report

Jupyter NoteBook link 

### Project overview and goals: 
Predict the SSD rated life consumed based on the SSD drive log page/system data collected through NetApp Autosupport feature


### Findings:
Best models for predicting rated life of SSDs are Random Forest and Decision Tree. Since Decision Tree and Random Forest had perfect scores, hyperparameter tuning (e.g., max_depth, min_samples_split) was used to prevent overfitting.
Additionally performed k-fold cross-validation to check if the model’s performance is consistent across different subsets of the data.
Even with these more constrained hyperparameters, Random Forest and Decision Tree models still achieved perfect scores (MAE=0.0, RMSE=0.0, R^2=1.0) on both the cross-validation and the test set. This ahows that the the selected features are extremely predictive of the rated_life_used target variable, to the point where even a simpler model (like a Random Forest with max_depth=5) can achieve a perfect fit. Also the dataset is inherently simple and have a very clear, almost deterministic, relationship between the features and the target in the provided sample data.

### **Results and conclusion:** 
1. Polynomial Regression (Degree 3) provides a strong balance between accuracy and generalization.
2. Random Forest and Decision Tree models show perfect or near-perfect scores, which is suspicious and likely indicates overfitting.
3. Regularized models (Ridge, Lasso) perform well, but not as well as polynomial regression.
4. Linear and Support Vector models are outperformed by others.

<img width="450" height="350" alt="{0E24F6F9-2670-4DB2-88EF-6AFB06E80A35}" src="https://github.com/user-attachments/assets/6a68fbbb-d043-4259-a2c2-ec9b264c7400" />


### Next steps and recommendations:
The persistent perfect scores (MAE  0.0 , R^2  1.0 ) despite regularization efforts suggest that the dataset is inherently deterministic or have a very strong, possibly direct, relationship between features and the target variable, rated_life_used. Feature importance highlights that the most important feature identified by almost all models was average_erase_block_count.
Further investigation into the data's nature and the definition of the target variable will be done on differnt set of data. 

### Data:
Dataset is from NetApp ASUP. The dataset contains 49,424 observations and 30 columns spanning device-level metadata, configuration information, and SSD health indicators. 
The schema is cleanly structured: 14 numeric columns, 15 categorical identifiers,

### EDA:
The dataset is large, well-organized, and consistent. Many features are redundant due to multicollinearity, especially among wear indicators, workload counters, and spare-related metrics. Categorical variables offer limited explanatory value beyond identifying individual systems, and high-cardinality identifiers are not useful for analysis. Exploratory Data Analysis strongly indicates that meaningful modeling should concentrate on a streamlined set of numeric features related to wear, defects, number of disks and workload, while removing redundant variables and disregarding identifier-like categorical fields.

### Preprocessiing and Final Dataset:
Selectied a reduced set of numeric features that capture the most meaningful signals related to device wear, defects, and workload, as identified through correlation analysis and principal component analysis.

### Methodology:
Seven models were trained were trained, fine-tuned, and will be later compared to find the best model for this task. (linear regression was used as a basline model)
Following shows 

1. Polynomial Regression - Created pipeline for polynomial regression, tuned the polynomial degree using grid search and cross-validation. Best Params: {'poly__degree': 3, 'poly__include_bias': False}
2. Ridge Regression - Created pipeline with polynomial features, tunes the polynomial degree and regularization strength using grid search and cross-validation - {'poly__degree': 3, 'ridge__alpha': 0.01}
3. Lasso Regression - Created pipeline with polynomial features, tunes the polynomial degree and regularization strength using grid search and cross-validation - Best Params: {'lasso__alpha': 0.01, 'poly__degree': 3}
4. Random Forest Regressor - Used grid search and cross-validation to select the best model, evaluates it on the test set, and prints feature importances. - Best parameters: {'max_depth': 10, 'min_samples_leaf': 2, 'min_samples_split': 5, 'n_estimators': 50}
5. Decision Tree Regressor - Used grid search and cross-validation to select the best model - Best parameters: {'max_depth': 7, 'min_samples_split': 2}    
6. Support Vector Regressor- Created a pipeline that first standardizes the features (using StandardScaler) and then fits a Support Vector Regressor (SVR) with an RBF kernel. Computes permutation feature importance for the SVR model on the test set


<img width="2813" height="940" alt="{397359A7-E0C0-4664-B9B9-96FC54290B22}" src="https://github.com/user-attachments/assets/9e752356-aa89-4e17-8d0c-609205701c60" />


