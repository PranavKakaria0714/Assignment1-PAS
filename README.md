# Probability Density Function Estimation on Air Quality Data

This project implements a non-linear data transformation and parameter estimation pipeline as required by Assignment-1. The objective is to model the probability density of a transformed air quality feature (`NO2`) using a specific exponential function parameterized by a university roll number.

## Dataset
* **Source:** India Air Quality Data
* **Feature Used:** `NO2` (Nitrogen Dioxide)
* **File:** `city_day.csv`

## Methodology

The solution proceeds in two distinct phases: data transformation based on a unique identifier and statistical parameter estimation.

### 1. Non-Linear Transformation
The raw feature vector $x$ (NO2 levels) is transformed into a new variable $z$ using a roll-number-dependent function. This introduces non-linearity and personalizes the dataset for the assignment.

The transformation logic is:
$$z = x + a_r \sin(b_r x)$$

Where coefficients $a_r$ and $b_r$ are derived from the University Roll Number ($r$) as follows:
* $$a_r = 0.05 \times (r \pmod 7)$$
* $$b_r = 0.3 \times ((r \pmod 5) + 1)$$

### 2. Parameter Estimation
We fit a specific probability density function (PDF) to the distribution of the transformed variable $z$. The target model is given by:

$$\hat{p}(z) = c \cdot e^{-\lambda(z - \mu)^2}$$

**Estimation Process:**
1.  **Histogram Generation:** The continuous variable $z$ is discretized into 100 bins to form an empirical density distribution.
2.  **Curve Fitting:** A non-linear least squares optimization (Levenberg-Marquardt algorithm) fits the theoretical model $\hat{p}(z)$ to the empirical histogram.
3.  **Parameters:** The algorithm iteratively optimizes three parameters:
    * $\lambda$ (Shape/Spread parameter)
    * $\mu$ (Location/Mean parameter)
    * $c$ (Scaling factor)

### 3. Validation Metrics
To ensure the solution is robust, the code calculates two goodness-of-fit metrics:
* **R-squared ($R^2$):** Measures the proportion of variance in the dependent variable explained by the model. A value closer to 1.0 indicates a better fit.
* **Chi-square ($\chi^2$):** Tests the difference between observed frequencies (histogram) and expected frequencies (model). An epsilon term ($\epsilon = 1e-9$) handles potential division-by-zero errors in empty bins.

## Results

### Parameter Table
The script outputs the calculated transformation coefficients and the estimated model parameters.

| Parameter | Symbol | Description |
| :--- | :---: | :--- |
| **Roll Number** | $r$ | User Input |
| **Coefficient A** | $a_r$ | Transformation Amplitude |
| **Coefficient B** | $b_r$ | Transformation Frequency |
| **Lambda** | $\lambda$ | Estimated Spread |
| **Mu** | $\mu$ | Estimated Peak Location |
| **Scaling** | $c$ | Estimated Height |

### Graphical Output
The script generates a plot containing:
1.  **Histogram (Gray):** Represents the actual density distribution of the transformed data $z$.
2.  **Fitted Curve (Red):** Represents the predicted PDF $\hat{p}(z)$ using the estimated parameters.

Visual alignment between the red curve and the gray histogram bars indicates a successful estimation.
