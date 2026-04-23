# Business Case Analysis

## B1. Problem Formulation

### (a)

This problem can be formulated as a supervised machine learning regression problem. The goal is to predict the number of items sold based on various input features.

* Target variable: items_sold
* Input features: store_id, store_size, location_type, promotion_type, competition_density, is_weekend, is_festival, transaction_date (derived features like month, day, etc.)
* Type of problem: Regression

Justification:
The target variable items_sold is a continuous numerical value, making this a regression problem. The objective is to estimate sales volume based on historical patterns and influencing factors.

---

### (b)

Using items_sold instead of total sales revenue is more reliable because revenue can be affected by external factors such as pricing strategies, discounts, or inflation. These factors may distort the true demand.

Items sold directly reflects customer demand and purchasing behavior, making it a more stable and consistent target variable.

Underlying principle:
The target variable should closely represent the actual business objective and should not be influenced by unrelated external factors. This ensures better model reliability and interpretability.

---

### (c)

Instead of using a single global model for all stores, a better approach is to use segmented or hierarchical modeling.

Possible strategies:

* Train separate models for different store categories (e.g., urban, semi-urban, rural)
* Group stores based on size or customer behavior and build models for each group
* Use store-specific features or embeddings to capture store-level differences

Justification:
Stores in different locations and categories behave differently in response to promotions. A single model may fail to capture these variations, whereas segmented models can provide more accurate and personalized predictions.

---

## B2. Data and EDA Strategy

### (a)

The data is available in four tables: transactions, store attributes, promotion details, and calendar data.

Join strategy:

* Join transactions with store attributes using store_id
* Join transactions with promotion data using promotion_type
* Join transactions with calendar data using transaction_date

Final dataset grain:

* One row represents one store on one day

Aggregations:

* Total items sold per store per day
* Average basket size
* Number of promotions applied
* Flags for weekend and festival days

This structure ensures that each row captures all relevant information needed for modeling.

---

### (b)

The following EDA steps should be performed:

1. Sales distribution analysis

   * Plot histogram of items_sold
   * Check for skewness or outliers
   * Helps decide if transformation is needed

2. Promotion effectiveness analysis

   * Compare average sales across different promotion types
   * Identifies which promotions are most effective

3. Time series analysis

   * Plot sales over time (daily or monthly)
   * Detect trends and seasonality

4. Correlation analysis

   * Use a correlation heatmap for numerical features
   * Identify important predictors and redundant features

Impact on modeling:

* Helps in feature selection
* Identifies need for transformations
* Reveals seasonal patterns to include as features
* Improves overall model performance

---

### (c)

If 80% of transactions occur without promotions, the dataset is imbalanced.

Impact:

* The model may become biased toward non-promotional data
* Poor performance when predicting promotional scenarios

Solutions:

* Use stratified or balanced sampling techniques
* Introduce a binary feature indicating promotion presence
* Evaluate model performance separately for promotional and non-promotional cases
* Consider weighting promotional cases higher during training

These steps ensure the model learns meaningful patterns for both scenarios.

---

## B3. Model Evaluation and Deployment

### (a)

A time-based train-test split should be used.

* Training set: first 80% of the timeline
* Test set: last 20% of the timeline

A random split is inappropriate because it breaks the temporal order and may lead to data leakage, where future information influences past predictions.

Evaluation metrics:

* RMSE (Root Mean Squared Error): penalizes larger errors more heavily
* MAE (Mean Absolute Error): provides average prediction error

Interpretation:

* Lower RMSE indicates better handling of large deviations
* MAE represents average error in predicting items sold, which is directly interpretable for business decisions

---

### (b)

Different recommendations for the same store across months occur due to changes in underlying factors.

Reasons:

* Seasonal variations (e.g., festive months vs normal months)
* Changes in customer demand patterns
* Differences in effectiveness of promotions across time

Investigation approach:

* Analyze feature importance from the model
* Compare input feature values across months
* Study seasonal trends in historical data

Communication:

* Explain how factors like festivals, weekends, and seasonal demand influence predictions
* Use feature importance and data trends to justify recommendations clearly to stakeholders

---

### (c)

End-to-end deployment process:

1. Train the model on historical data
2. Save the trained model
3. At the start of each month:

   * Collect new data
   * Prepare features using the same preprocessing pipeline
   * Generate predictions for each store

Monitoring:

* Track performance metrics such as RMSE and MAE over time
* Monitor prediction errors and data drift

Retraining:

* If model performance degrades significantly, retrain using updated data
* Schedule periodic retraining (e.g., monthly or quarterly)

This ensures the model remains accurate and adapts to changing business conditions.

---

