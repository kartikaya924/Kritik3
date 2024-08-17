# Kritik3
A case study involving the implementation of decision tree.

# Decision Tree Model Analysis

## 1. Background and Data Exploration

**a. Medicare Overview**
- Medicare is a U.S. federal health care program providing coverage for individuals aged 65 and older, those under 65 on Social Security Disability Insurance (SSDI), or those under 65 with End-Stage Renal Disease (ESRD).

**b. Key Data Exploration Points**
- The states most represented in the dataset are TX, FL, CA, and IL, respectively (Fig 1).
- The age column is left-skewed, with the median age of the consumer base between 70-80 years (Fig 2).
- The prediction class (`profit_b`) seems to be equally represented, indicating no class imbalance (Fig 3).

**c. Data Preprocessing**
- **Handling Missing Values:**
  - Decision trees are robust in handling missing values by bypassing null values and considering other feature values.
  - Given the randomness of null values, no specific handling was required.
  
- **Correct Data Type:**
  - All columns have the correct datatype based on their attributes and our goals.
  
- **Encoding Categorical Variables:**
  - Encoding was deemed unnecessary as decision trees are typically unaffected by the scaling of features.
  - The model focuses on feature contribution rather than magnitude.

## 2. Decision Tree Model

**a. Data Splitting**
- The dataset was split into training and validation sets with a 70/30 split, using a random seed of '12345'.

**b. Model Building**
- **Target Variable:** `Profit_b`
- **Hyper-parameter Tuning:**
  - Experimental tuning involved testing various parameter combinations to optimize results.
  - **Parameters Used:**
    - `Criterion: gini`: Measures impurity at each node, with lower impurity indicating better splits.
    - `Max_depth: 8`: The tree can grow to make 8 splits to reach the final classification.
    - `Min_sample_leaf: 8`: Each leaf node will have at least 8 data points.
    - `Min_samples_split: 8`: A split will occur only when there are at least 8 sample points.
   
      <img width="509" alt="image" src="https://github.com/user-attachments/assets/9834b2b8-0772-4a12-834c-4cdfc2fcb7f9">


**c. Overfitting Prevention**
- Overfitting is a risk for non-parametric models like decision trees. 
- K-fold cross-validation was used to ensure the model generalizes well to larger datasets.

## 3. Model Performance Metrics

- The classification report, along with the confusion matrix, provides detailed insights into the model's performance.

  <img width="463" alt="image" src="https://github.com/user-attachments/assets/39d3ecfb-7566-449c-9868-d6537567ecb6">


## 4. Business Metrics

**a. Gain and Lift Metrics**

**i. Implementation:**
1. Obtain model prediction probabilities for class '1' based on test data and sort the DataFrame in descending order.
2. Add true labels (`y_test`) to the sorted DataFrame, divide the data into deciles (10% subsets), and calculate the cumulative number of positive instances in each decile.
3. Calculate cumulative gain and lift at each decile.

   <img width="517" alt="image" src="https://github.com/user-attachments/assets/a1190748-b251-4637-90ff-d338133b24fe">


**ii. Recommendations:**
- Till decile 6, the model achieved a gain of over 50%, indicating that these segments contain a higher concentration of positive instances.
- Lift values of ~1 suggest the model's performance is comparable to a random model, signaling a need for adjustments.

## 5. Analysis and Interpretation

**i. Traditional Metrics:**
- **Precision for class '1':** 0.68, meaning 68% of instances predicted as '1' are correct.
- **Recall for class '1':** 0.84, indicating the model effectively captures a significant portion of profit instances.

**ii. Key Predictive Factors:**
- The top 5 features predicting above-average profits, in descending order of importance, are highlighted.

  <img width="541" alt="image" src="https://github.com/user-attachments/assets/290e9b92-4d91-43c1-9950-c73abc764932">


**iii. Strategic Decision Rules:**
- Regions with an average number of total visits provided by the HHA during a non-LUPA episode less than 16.75 may be profitable.
- Regions with values <= 8 have a higher chance of being profitable.
- A cancer condition prevalence of less than 0.16% might indicate a profitable region.
- Specific provider IDs <= 57711 are significant in predicting above-average profits, indicating that certain providers in those regions impact financial outcomes.
