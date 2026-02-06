# Models of Model Uncertainties

This repository provides an overview of core techniques for uncertainty quantification (UQ) in machine learning models. These methods are critical when predictions must be reliable, interpretable, and decision-aware, especially in high-stakes domains such as finance, healthcare, energy, and large-scale operations.

We focus on methods that estimate predictive uncertainty, covering both aleatoric (data noise) and epistemic (model uncertainty) perspectives.

## Conformal Prediction

What it is
A distribution-free framework that constructs prediction intervals by calibrating against historical prediction errors.

**Strengths**

- Model-agnostic: works with any predictive model
- Finite-sample coverage guarantees under mild assumptions
- Robust to misspecification of data distribution

**Limitations**

- Interval width depends heavily on calibration set quality
- Computational overhead increases with large datasets or repeated recalibration
- Provides marginal, not conditional, coverage guarantees

**Best used when**

- You need guaranteed coverage and can tolerate wider intervals
- Model interpretability or architecture flexibility matters more than tight uncertainty bounds

## Gaussian Processes (GPs)

What it is
A Bayesian non-parametric approach that models uncertainty through learned covariance functions over the input space.

**Strengths**

- Strong theoretical grounding
- Well-calibrated predictive uncertainty
- Naturally captures epistemic uncertainty

**Limitations**

𝑂
(
𝑛
3
)
O(n
3
) computational complexity limits scalability

- Kernel choice strongly influences performance
- Less practical for high-dimensional or massive datasets without approximations

**Best used when**

- Dataset size is moderate
- You need high-quality uncertainty estimates, not just point predictions

## Quantile Regression

What it is
Directly estimates conditional quantiles of the target variable instead of modeling its full distribution.

**Strengths**

- No distributional assumptions
- Easy to implement and explain
- Produces asymmetric prediction intervals naturally

**Limitations**

- Requires separate models (or heads) per quantile
- Captures aleatoric uncertainty only
- No probabilistic interpretation beyond quantiles

**Best used when**

- Interpretability is key
- Noise is heteroscedastic
- You care about tail behavior (e.g. risk bounds)

## Bayesian Linear Regression

What it is
Extends linear regression by placing probability distributions over model parameters.

**Strengths**

- Closed-form solutions under conjugate priors
- Built-in regularization
- Clear separation between data noise and parameter uncertainty

**Limitations**

- Sensitive to prior selection
- Limited expressiveness for complex, non-linear relationships
- Inference cost grows with feature dimensionality

**Best used when**

- Model simplicity and interpretability matter
- Data is limited but structured

## Markov Chain Monte Carlo (MCMC)

What it is
A class of algorithms that sample from the exact posterior distribution using stochastic transition kernels.

Strengths
- Asymptotically exact posterior inference
- Gold standard for Bayesian uncertainty estimation
- Applicable to a wide range of models

Limitations

- Extremely slow for large or high-dimensional models
- Convergence diagnostics are non-trivial
- Often impractical in production environments

Best used when
- Correctness > speed
- You need a benchmark posterior for research or validation

## Normalizing Flows

What it is
Flexible density estimators that learn complex distributions via invertible transformations.

Strengths

- Exact likelihood computation
- Highly expressive posterior approximations
- Efficient sampling once trained

Limitations

- Architecture design is non-trivial
- Training can be numerically unstable
- Requires careful regularization and initialization

Best used when

- Posterior distributions are multi-modal or highly non-Gaussian
- You need both expressiveness and tractability

## Variational Inference (VI)

What it is
Approximates Bayesian inference by solving an optimization problem over a restricted family of distributions.

Strengths

- Scales well to large datasets and deep models
- Fast inference compared to MCMC
- Widely supported in modern probabilistic frameworks

Limitations

- Systematically underestimates uncertainty
- Approximation quality depends heavily on variational family
- Optimization objectives may favor bias over variance

Best used when

- You need Bayesian-style uncertainty at scale
- Approximate uncertainty is acceptable

## Method Selection Cheat Sheet

| Scenario                                | Recommended Approach       |
| --------------------------------------- | -------------------------- |
| Guaranteed coverage                     | Conformal Prediction       |
| Small–medium datasets, high fidelity    | Gaussian Processes         |
| Heteroscedastic noise, interpretability | Quantile Regression        |
| Simple models, strong priors            | Bayesian Linear Regression |
| Research-grade Bayesian inference       | MCMC                       |
| Complex posteriors                      | Normalizing Flows          |
| Large-scale deep learning               | Variational Inference      |

## Final Notes

No single uncertainty method dominates across all regimes. In practice:

- Hybrid approaches (e.g. Quantile Regression + Conformal Prediction) often outperform pure methods
- Calibration quality matters as much as interval construction
- Computational constraints frequently dominate theoretical elegance
- Treat uncertainty estimation as a modeling decision, not a post-hoc add-on.