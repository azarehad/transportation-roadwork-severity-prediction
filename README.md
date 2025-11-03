# Transportation Roadwork Severity Prediction

This project explores the "US Road Construction and Closures" dataset from Kaggle to analyze and predict the severity of roadwork projects. The analysis begins with a broad exploratory data analysis of the entire US dataset and then narrows in to build predictive models for roadwork events in **New York City**.

The primary goal is to build a binary classification model that can predict high-impact roadwork events (Severity 4) versus all other events.

**Kaggle:** [View the original notebook on Kaggle](https://www.kaggle.com/code/ashkanzare/transportation-roadwork-severity-prediction)

## Data Source

* **Dataset:** [US Road Construction and Closures](https://www.kaggle.com/datasets/sobhanmoosavi/us-road-construction-and-closures)
* **File:** `US_Constructions_Dec21.csv`
* **Description:** This dataset contains spatiotemporal data on road constructions and closures in the US, collected from Bing and MapQuest APIs. Severity is defined on a scale of 1 (least impact) to 4 (most impact) on traffic.

## Methodology

The project is has three main parts: exploratory data analysis, data preprocessing for modeling, and machine learning.

### 1. Exploratory Data Analysis

* **Geospatial Analysis:**
    * Visualized the total number of roadwork projects by state using a Plotly choropleth map.
    * Identified the top 10 states and cities with the most roadwork projects.
        * **Top City (Total Projects):** Phoenix
        * **Top City (Severity 4 Projects):** Houston
* **Temporal Analysis:**
    * Investigated the distribution of projects by year, hour, and day of the week.
    * Calculated that approximately 3.5% of all projects last longer than 15 days.
* **Severity Analysis:**
    * Analyzed the relationship between `Severity` and features like `Duration` and `Distance(mi)`.
    * Found that higher severity generally correlates with longer duration and greater distance.
* **Hypothesis Testing (A/B Tests):**
    * **Result 1:** The duration of projects starting at midnight is **not significantly different** from projects starting at other hours.
    * **Result 2:** Roadwork projects occurring on **weekdays have a significantly longer duration** than those on weekends.

### 2. Data Preprocessing & Modeling (New York City)

For the modeling phase, the project focuses exclusively on data from **New York City (`nc`)**.

* **Target Variable:** The `Severity` column was binarized to create a binary classification problem:
    * **Class 1:** Severity 4 (high-impact events)
    * **Class 0:** Severity 1, 2, or 3 (all other events)
* **Feature Engineering:**
    * `Duration` was calculated in minutes.
    * `Hour` and `Weekday` were one-hot encoded.
    * Features with high percentages of missing data (like `Wind_Chill(F)`) and irrelevant/redundant features (like `Street`, `City`, `County`) were dropped.
* **Sampling:** The dataset was highly imbalanced. To fix this, a balanced dataset was created by **over-sampling** the minority class (Severity 4) and **under-sampling** the majority class (Other) to get 10,000 samples for each.
* **Train/Test Split:** A **temporal split** was used to prevent data leakage and simulate a real-world prediction scenario:
    * **Training Set:** All data *before* 2021.
    * **Test Set:** All data *from* 2021.

### 3. Machine Learning Models

Four different classification models were trained and evaluated on their ability to predict Severity 4 events.

| Model | Test Accuracy | Test AUC |
| :--- | :--- | :--- |
| Logistic Regression | 50.9% | 0.593 |
| Bagging Classifier | 74.5% | 0.838 |
| **Random Forest** | **82.6%** | **0.908** |
| Gradient Boosting | 74.6% | 0.863 |

## Results & Key Findings

* The **Random Forest Classifier** was the best-performing model, achieving an **Accuracy of 82.6%** and an **AUC of 0.908** on the 2021 test data.
* **Feature Importance:** The most significant predictors for a high-severity event are:
    1.  `Duration`
    2.  `Distance(mi)`
    3.  `End_Lng` (End Longitude)
    4.  `Start_Lat` (Start Latitude)
    5.  `End_Lat` (End Latitude)
    6.  `Start_Lng` (Start Longitude)
* **Qualitative Analysis:** A review of the event descriptions suggests that Severity 4 events are often long-term, structural closures (e.g., "COVID-19 - Outdoor dining," "major gas installation"), while lower-severity events are related to short-term, mobile work (e.g., "bridge maintenance," "crane operation").

## 🚀 How to Use

1.  Clone this repository.
2.  Ensure you have the required Python libraries installed: `pandas`, `numpy`, `geopandas`, `plotly`, `matplotlib`, `seaborn`, and `scikit-learn`.
3.  Download the `US_Constructions_Dec21.csv` file from the [Kaggle dataset](https://www.kaggle.com/datasets/sobhanmoosavi/us-road-construction-and-closures) and place it in the correct directory.
4.  Run the `transportation-roadwork-severity-prediction.ipynb` notebook.
