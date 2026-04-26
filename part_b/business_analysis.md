## Business Case Analysis

### Scenario: Promotion Effectiveness at a Fashion Retail Chain

---

## B1. Problem Formulation

### (a) Machine Learning Formulation

The goal here is to predict how many items will be sold at a store when a particular promotion is applied during a given time period.

* **Target Variable:** `items_sold`

* **Input Features:**

  * Store-related: store size, location type, competition density
  * Promotion-related: type of promotion
  * Time-related: month, weekend, festival indicators
  * Customer behavior: footfall, past sales patterns

* **Type of Problem:** Supervised Learning (Regression)

This is a regression problem because we are predicting a numerical value (number of items sold), not assigning categories.

---

### (b) Why Items Sold Instead of Revenue

Using total revenue might seem like a good idea, but it can be misleading. Revenue is affected by pricing, discounts, and product mix. For example, a big discount might increase the number of items sold but reduce total revenue.

On the other hand, **items sold directly reflects how effective a promotion is in attracting customers and increasing sales volume**.

This highlights an important principle in machine learning:

The target variable should closely match the actual business objective and should not be influenced by unrelated factors.

---

### (c) Alternative Modelling Strategy

Instead of building one single model for all stores, a better approach would be to account for differences across locations.

For example:

* Build separate models for urban, semi-urban, and rural stores
* Or include location-based features and interactions in the model

This is important because customer behavior and promotion effectiveness can vary significantly depending on the store’s location.

---

## B2. Data and EDA Strategy

### (a) Data Joining and Dataset Design

The data comes from four different tables, so we need to combine them carefully:

* Join **transactions** with **store attributes** using `store_id`
* Join with **promotion details** using promotion identifiers
* Join with **calendar data** using `transaction_date`

The final dataset should have one row representing a store’s performance for a given month under a specific promotion.

Before modelling, we would aggregate:

* Total items sold per store per month
* Total visits or footfall
* Average basket size
* Promotion applied

This ensures the data matches the level at which decisions are made.

---

### (b) Exploratory Data Analysis

Before building the model, I would perform the following analyses:

1. **Promotion vs Items Sold (Bar Chart)**
   To compare how different promotions perform

2. **Time Series Plot**
   To observe trends and seasonal patterns in sales

3. **Correlation Heatmap**
   To understand relationships between variables

4. **Boxplots by Location Type**
   To compare performance across different store categories

These analyses help in understanding the data better and guide feature selection and model design.

---

### (c) Handling Promotion Imbalance

Since most transactions happen without promotions, the dataset is imbalanced.

This can cause the model to:

* Focus more on “no promotion” cases
* Fail to learn the actual impact of promotions

To handle this:

* Use sampling or weighting techniques
* Ensure enough representation of promotion cases
* Possibly model promotion impact separately

---

## B3. Model Evaluation and Deployment

### (a) Train-Test Split and Metrics

Since the data is time-based, we should split it chronologically:

* Train on earlier data
* Test on the most recent data

A random split would mix past and future data, which is unrealistic and leads to overly optimistic results.

**Evaluation Metrics:**

* RMSE: penalizes large errors more heavily
* MAE: gives average prediction error in simple terms

In this case:

* RMSE helps identify large mistakes in forecasting
* MAE helps understand overall prediction accuracy

---

### (b) Explaining Model Decisions

Feature importance can help explain why different promotions are recommended for the same store in different months.

For example:

* In December, festivals and higher demand may make loyalty-based promotions more effective
* In March, customers might be more price-sensitive, making discounts more effective

By analyzing important features, we can clearly explain these differences to the marketing team.

---

### (c) Deployment Strategy

The deployment process would look like this:

1. **Save the model** using tools like joblib
2. **Prepare new monthly data** using the same preprocessing steps
3. **Generate predictions** for each store
4. **Select the best promotion** based on predicted items sold

For monitoring:

* Compare predicted vs actual performance
* Track metrics like RMSE over time
* Detect performance drops

If performance declines, the model should be retrained using updated data.

This ensures the system remains accurate and useful for decision-making.
