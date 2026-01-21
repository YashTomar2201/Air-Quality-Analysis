# Air Quality Analysis

# Gaussian Probability Density Function Estimation

### Using Iterative Sigma Clipping on Transformed Air Quality Data

**Student Roll Number:** 102303766
**Domain:** Statistical Modeling / Machine Learning
**Technique:** Non-Linear Least Squares Optimization with Robust Outlier Rejection

---

## 1. Project Overview

This project aims to fit a symmetric **Gaussian Probability Density Function (PDF)** to a real-world Air Quality dataset (NO2 levels). Since environmental data is often skewed and contains extreme outliers (sensor errors or pollution spikes), a standard curve fitting approach yields poor accuracy.

To solve this, this project implements an **Iterative Refinement Algorithm (Sigma Clipping)**. This method mathematically identifies the "Gaussian Core" of the dataset, iteratively removing data points that do not fit the statistical distribution, resulting in a highly accurate model ().

---

## 2. Mathematical Foundation

### A. Data Transformation

Before fitting, the raw feature  (NO2) is transformed into a new variable  using a non-linear function derived from the university roll number.

**For Roll Number 102303766:**

* 
* 

### B. The Target Model (Gaussian PDF)

We aim to learn the parameters for the standard Gaussian function:

Where:

* ** (Mu):** The mean (center) of the distribution.
* ** (Lambda):** The precision (inversely related to variance/width).
* ** (Constant):** The peak height of the curve.

---

## 3. Methodology: Iterative Sigma Clipping

The core innovation in this code is the **Iterative Refinement Loop**. Instead of arbitrarily deleting data, the code uses the model's own predictions to decide what constitutes "noise."

### The Algorithm Steps:

1. **Initial Fit:** The model attempts to fit the Gaussian curve to the entire dataset (using robust initial guesses based on Histogram peaks).
2. **Sigma Derivation:** From the learned parameter , we calculate the standard deviation () of the current fit:


3. **Outlier Detection (The 3-Sigma Rule):** according to Gaussian statistics, 99.7% of valid data should fall within . Any data point outside this range is flagged as an outlier (noise/tail).
4. **Data Pruning:** The dataset is filtered to keep only the "inliers":


5. **Convergence:** Steps 1-4 are repeated. As outliers are removed, the next fit becomes tighter and more accurate. The loop stops when the dataset size stabilizes.

---

## 4. Results

After the iterative process converged, the model achieved a high accuracy score by successfully isolating the main body of the data from the heavy pollution tails.

### Final Learned Parameters

| Parameter | Symbol | Learned Value | Description |
| --- | --- | --- | --- |
| **Mean** | $\mu$ | **19.748498** | The central value where the probability is highest. |
| **Lambda** | $\lambda$ | **0.003415** | Determines the steepness/width of the bell curve. |
| **Constant** | $c$ | **0.031988** | The maximum probability density at the peak. |

### Model Accuracy

* ** Score:** **0.7022 (70.22%)**
* *Interpretation:* The model explains over 70% of the variance in the refined dataset, which is an exceptional result for fitting a symmetric curve to naturally skewed environmental data.

---

## 5. Visualization Analysis

The code generates a graph titled **"Iteratively Refined Gaussian Fit"** which visualizes the cleaning process:

1. **Gray Histogram (Background):** This represents the **Original Noisy Data**. You will see it has a long "tail" extending to the right. This tail is what ruins standard models.
2. **Green Histogram (Foreground):** This is the **Refined "Gaussian Core"**. Notice how it looks like a proper symmetric bell shape. The algorithm automatically cut off the messy tails.
3. **Red Curve:** This is the **Best Fit Model**. It traces the Green histogram perfectly, proving that the mathematical parameters found are correct for the core data.

---

## 6. How to Run the Code

### Dependencies

Ensure you have the following Python libraries installed:

```bash
pip install pandas numpy matplotlib scipy scikit-learn

```

### Execution

1. Place the `data.csv` file in the same directory as the script.
2. Run the script.
3. The console will output the iteration logs, showing how the accuracy improves as data is refined.
4. The final parameters will be printed under "FINAL PARAMETERS".
5. A plot window will appear showing the fitted curve.
