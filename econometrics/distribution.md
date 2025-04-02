# Comparison of Distributions for Simulation (Econometrics Focus)

| Distribution    | Library Name (NumPy/SciPy)                         | Typical Use Case                                     | Pros                                              | Cons                                                  |
| --------------- | -------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------- | ----------------------------------------------------- |
| **Normal**      | `numpy.random.normal` / `scipy.stats.norm`         | Modeling errors, returns, test statistics            | Well-understood, CLT applies                      | Assumes symmetry, poor fit for skewed/fat-tailed data |
| **Uniform**     | `numpy.random.uniform` / `scipy.stats.uniform`     | Random choice, naive simulation                      | Simple, bounded                                   | Unrealistic for most natural data                     |
| **Exponential** | `numpy.random.exponential` / `scipy.stats.expon`   | Time between events, waiting times                   | Memoryless, simple                                | Unrealistic for complex inter-arrival processes       |
| **Poisson**     | `numpy.random.poisson` / `scipy.stats.poisson`     | Count of rare events                                 | Discrete, easy for event modeling                 | Assumes equal mean and variance                       |
| **Binomial**    | `numpy.random.binomial` / `scipy.stats.binom`      | Binary outcomes over trials                          | Straightforward for binary processes              | Limited to fixed n and p                              |
| **Log-Normal**  | `numpy.random.lognormal` / `scipy.stats.lognorm`   | Skewed data, income modeling                         | Captures multiplicative processes                 | Not defined for negatives                             |
| **Beta**        | `scipy.stats.beta`                                 | Probabilities, proportions, priors in Bayesian stats | Flexible shape, defined on [0,1]                  | Can be unintuitive to parameterize                    |
| **Gamma**       | `numpy.random.gamma` / `scipy.stats.gamma`         | Duration modeling, survival, Bayesian priors         | Skewed, only positive, flexible shape             | Complex parameter tuning                              |
| **Weibull**     | `numpy.random.weibull` / `scipy.stats.weibull_min` | Lifetime modeling, survival, reliability             | Handles increasing/decreasing hazard rates        | Interpretation of shape parameter can be tricky       |
| **Pareto**      | `scipy.stats.pareto`                               | Modeling wealth, heavy-tailed distributions          | Power-law, heavy tails                            | Infinite mean/variance depending on shape             |
| **Gumbel**      | `scipy.stats.gumbel_r`                             | Extreme value theory, modeling max returns or losses | Useful for modeling extremes                      | Only models one tail (min or max)                     |
| **Triangular**  | `scipy.stats.triang`                               | Simulation when only min, max, and mode are known    | Intuitive parameters, bounded                     | Simple, but may not fit real-world data well          |
| **Laplace**     | `scipy.stats.laplace`                              | Fat-tailed alternative to normal; Bayesian modeling  | Sharp peak, fatter tails than normal              | Less common in traditional modeling                   |
| **Student's t** | `scipy.stats.t`                                    | Modeling fat tails, robust regression residuals      | Accounts for heavy tails, useful in small samples | Assumes symmetry, tail parameter needs tuning         |

## Overview of Distributions in `scipy.stats`

The `scipy.stats` module includes **dozens of continuous and discrete probability distributions**, making it a powerful tool for simulations, statistical modeling, and hypothesis testing in Python.

### Key Features:

- **Over 100 distributions** supported (both continuous and discrete).
- Unified interface: `.pdf()`, `.cdf()`, `.rvs()`, `.fit()`, etc.
- Used for **simulation, MLE fitting, hypothesis testing**, and more.

### Categories of Distributions

#### Continuous Distributions (Real-Valued)

| Type                    | Example Distributions                 | Common Uses                                 |
| ----------------------- | ------------------------------------- | ------------------------------------------- |
| Symmetric / Bell-shaped | `norm`, `t`, `laplace`                | Central limit theory, robust modeling       |
| Skewed / Right-tailed   | `gamma`, `lognorm`, `weibull_min`     | Income, duration, survival modeling         |
| Heavy-tailed            | `pareto`, `t`, `logistic`             | Financial data, outlier-prone datasets      |
| Bounded / Limited Range | `beta`, `triang`, `uniform`           | Probabilities, constrained simulations      |
| Extreme Value / Max-Min | `gumbel_r`, `genextreme`              | Modeling rare, extreme events               |
| Miscellaneous           | `rice`, `rayleigh`, `chi`, `dweibull` | Physics, signal processing, niche use cases |

#### Discrete Distributions (Integer-Valued)

| Type                   | Example Distributions        | Common Uses                               |
| ---------------------- | ---------------------------- | ----------------------------------------- |
| Binary / Trial-based   | `bernoulli`, `binom`         | Binary outcomes, trials                   |
| Count-based            | `poisson`, `nbinom`, `geom`  | Event counts, waiting times               |
| Finite outcomes        | `hypergeom`, `multinomial`   | Sampling without replacement, categorical |
| Integer-valued special | `zipf`, `planck`, `dlaplace` | Power-law models, text/data modeling      |

### Common Methods for Any Distribution

For a distribution like `scipy.stats.norm` (normal):

- `rvs(size=...)` – Generate random samples
- `pdf(x)` – Probability density function
- `cdf(x)` – Cumulative distribution function
- `ppf(q)` – Percent-point function (inverse CDF)
- `fit(data)` – Fit distribution parameters to data
- `mean()`, `var()`, `std()`, `moment(n)`, etc.

### 💡 Tip:

Use `scipy.stats.rv_continuous` or `rv_discrete` subclasses to create **custom distributions** if needed.
