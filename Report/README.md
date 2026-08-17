# International-Bank-Fraud-Detection-TLAB

#### This project walks through the full end‑to‑end process of detecting fraudulent 

This project walks through the full end‑to‑end process of detecting fraudulent transactions using a cleaned and pre‑processed dataset. The workflow includes exploratory data analysis (EDA), data cleaning, feature encoding, model creation, hyperparameter tuning, and final evaluation using F1 score, precision, and recall.

### Exploratory Data Analysis (EDA)
The EDA revealed several important insights about the dataset and guided the cleaning and modeling decisions that followed

- Fraud is extremely rare, making the dataset highly imbalanced.
- Fraudulent transactions occur almost exclusively in TRANSFER and CASH_OUT transaction types.
- Balance‑related columns show strong patterns: fraudulent transactions often involve large withdrawals or transfers with sharp drops in the origin account’s balance.
- Some values were impossible, such as negative balances, indicating data quality issues.
- Several columns (like account IDs) had no predictive value and were removed later.

### Data Cleaning & Wrangling
Based on the EDA, the following cleaning steps were performed:

1. Dropped irrelevant columns:
2. nameOrig, nameDest (high‑cardinality identifiers)
3. isFlaggedFraud (rarely flags actual fraud and adds noise)

Corrected impossible values:

1. Negative balances replaced with NaN → then filled with 0
2. Removed extreme outliers in transaction amounts
3. Ensured proper formatting:
4. Numeric columns converted to numeric
5. type column encoded using one‑hot encoding
6. The cleaned dataset was saved as fraud_clean.csv for modeling.

### Modeling Approach
Baseline Model: 
- A lightweight Logistic Regression model was used to avoid overheating and ensure fast training.
- The dataset was sampled to 3% to reduce computational load while maintaining representative fraud cases.

Hyperparameter Tuning: 
- I used GridSearchCV with a small parameter grid:
C: [0.1, 1, 10]

Grid search was chosen because:

- The parameter space was small
- Logistic Regression is lightweight
- It is easy to interpret
- It runs safely on a laptop without overheating
- Random search was unnecessary for this project.

### Optimized Model Performance
After tuning, the optimized model achieved:

Precision (fraud): 1.00  
Recall (fraud):    0.89  
F1 Score (fraud):  0.94  

Confusion Matrix
[[7619    0]
 [   1    8]]


# Final Project Report
### Which insights did you gain from your EDA?
During the EDA, I discovered several important patterns in the fraud dataset. First, the dataset is extremely imbalanced, with fraudulent transactions representing a very small fraction of all observations. I also observed that certain transaction types—specifically TRANSFER and CASH_OUT—were strongly associated with fraud, while other types rarely appeared in fraudulent cases. Balance-related columns showed clear patterns: fraudulent transactions often involved large withdrawals or transfers where the origin account’s balance dropped sharply. I also identified impossible values such as negative balances, which indicated data quality issues that needed correction. Overall, the EDA highlighted which features were meaningful for fraud detection and which were not.

### How did you determine which columns to drop or keep?
I used insights from the EDA to decide which columns were useful for modeling. Columns such as nameOrig and nameDest were dropped because they are simply account identifiers with extremely high cardinality and no predictive value. The column isFlaggedFraud was also removed because it rarely flagged actual fraud and did not contribute meaningful information. Balance columns were kept because the EDA showed strong relationships between balance changes and fraudulent behavior. The type column was kept and later encoded because transaction type was one of the strongest indicators of fraud. In short, columns were dropped if they provided no statistical insight, no predictive value, or introduced noise, while columns showing meaningful patterns were retained.

### Which hyperparameter tuning strategy did you use? Grid-search or random-search? Why?
I used GridSearchCV for hyperparameter tuning. Grid search was chosen because it is straightforward, easy to interpret, and appropriate for a small, lightweight model like Logistic Regression. Since I intentionally reduced the dataset size to protect my computer from overheating, grid search was computationally safe and efficient. Random search is more useful for large models or very large parameter spaces, but in this project the parameter grid was intentionally small, making grid search the better choice.

### How did your model’s performance change after discovering optimal hyperparameters?
After tuning the model, performance improved slightly, especially in detecting fraudulent transactions. The optimized model maintained perfect precision for the fraud class and improved recall compared to the baseline. This means the tuned model was better at identifying fraud cases without increasing false positives. The F1 score for the fraud class increased, showing a better balance between precision and recall. Overall, the optimized model became more effective at identifying rare fraud cases while still maintaining high accuracy on non‑fraud transactions.

### What was your final F1 Score? And what is your interpretation of the precision metric vs the recall metric?
My final F1 score for the fraud class was 0.94. Precision for fraud was 1.00, meaning every transaction the model predicted as fraud was truly fraudulent—there were no false positives. Recall was 0.89, meaning the model successfully identified most fraud cases but missed one. Precision reflects how reliable the model’s fraud predictions are, while recall reflects how many actual fraud cases the model was able to catch. In fraud detection, recall is especially important because missing fraud can be costly, but high precision is also valuable because false alarms can disrupt legitimate customers. The final model achieved a strong balance between the two.