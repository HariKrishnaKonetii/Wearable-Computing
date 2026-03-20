# Homework 3 – Statistical Learning & Prediction

**Student:** Hari Krishna  
**Course:** EE599  

---

## Overview

This assignment implements three statistical learning methods from scratch:

- K-Means Clustering  
- K-Nearest Neighbors (KNN)  
- Change Point Analysis (CPA)  

All methods are implemented using basic numerical operations without external machine learning libraries.

The algorithms are applied to wearable activity data (`dailySteps.csv` and `dailyCalories.csv`) to analyze activity patterns, perform prediction, and detect behavioral changes over time.

---

## Repository Contents

- `INF632_HW3_.ipynb` – Main notebook with implementation and results  
- `dailySteps (1).csv` – Daily step count data  
- `dailyCalories.csv` – Daily calorie data  
- `cluster_summary.csv` – K-Means cluster statistics  
- `change_point_summary.csv` – Change point analysis results  
- `plot_kmeans_steps_calories.png` – K-Means visualization  
- `plot_knn_steps_class.png` – KNN visualization  
- `plot_change_points_steps.png` – Change point visualization  

---

## 1. K-Means Clustering

### Approach
- Centroids are initialized using spread-out points  
- Euclidean distance is used for cluster assignment  
- A single-pass update is performed as required  

### Implementation Details
- Handles cases where `k` exceeds dataset size  
- Retains centroid if a cluster becomes empty  
- Maintains consistent feature dimensions  

### Results

| Cluster | Count | Mean Steps | Mean Calories |
|--------|------:|-----------:|--------------:|
| 0 | 284 | 92.32 | 2697.99 |
| 1 | 359 | 5884.43 | 2693.82 |
| 2 | 24 | 14119.46 | 3232.42 |

### Interpretation
The clustering separates the data into:
- low activity  
- moderate activity  
- high activity  

Higher activity clusters show increased calorie expenditure, indicating meaningful grouping.

---

## 2. K-Nearest Neighbors (KNN)

### Approach
- Euclidean distance is used to measure similarity  
- Calories are converted into two classes using the median value:
  - HighCalorie  
  - LowCalorie  

### Implementation Details
- Ensures `k` is odd to avoid ties  
- Adjusts `k` if it exceeds dataset size  

### Results
- Test input: **8000 steps**  
- Predicted class: **HighCalorie**  
- Holdout accuracy: **0.80**

### Interpretation
The nearest neighbors of the test point belong to the higher calorie class, showing that step count is a strong predictor of calorie expenditure.

---

## 3. Change Point Analysis (CPA)

### Approach
- Uses variance reduction (sum of squared error) to evaluate splits  
- Identifies dominant change points recursively  
- Stops when additional splits are not significant  

### Preprocessing
- Zero-step days are removed  

### Results

| Date | Mean Before | Mean After | Difference |
|------|------------:|-----------:|-----------:|
| 2013-07-15 | 5981.57 | 2932.93 | -3048.64 |
| 2013-07-29 | 2932.93 | 8522.84 | +5589.91 |
| 2013-08-18 | 8522.84 | 4270.26 | -4252.58 |
| 2014-06-25 | 4270.26 | 191.26 | -4079.00 |
| 2014-08-29 | 191.26 | 5571.51 | +5380.25 |

### Interpretation
The detected change points correspond to significant shifts in activity levels.

- Large changes (>4000 steps) indicate behavioral transitions  
- Multiple changes occur during summer months  
- This suggests seasonal variation and lifestyle changes  

---

## 4. Visualization Summary

### K-Means Plot
Shows separation of activity groups based on steps and calories.  
Centroids represent average behavior of each group.

---

### KNN Plot
Displays labeled data and the test point, explaining the classification decision.

---

### Change Point Plot
Shows the time series of steps with marked change points indicating major shifts in activity.

---

## Key Observations

- Activity data forms distinct behavioral clusters  
- Step count is strongly related to calorie expenditure  
- Change points reveal meaningful temporal patterns  
- Seasonal trends are visible in the dataset  

---

## Conclusion

The implemented methods successfully identify patterns, relationships, and structural changes in wearable activity data.  
The results demonstrate that simple statistical learning techniques can provide meaningful insights into human behavior.
