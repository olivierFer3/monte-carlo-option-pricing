# Monte Carlo Option Pricing

Monte Carlo pricing of European and Asian call options under Geometric Brownian Motion, validated against closed-form solutions. The project measures the convergence rate of the estimator, computes Greeks with the pathwise method, and compares variance reduction techniques.

All code and results are in [`monte_carlo_option_pricing.ipynb`](monte_carlo_option_pricing.ipynb).

## Key results

Parameters: S0 = 100, K = 100, T = 1 year, r = 3%, σ = 20%.

| Result | Value |
|---|---|
| Black-Scholes price (European call) | 9.4134 |
| Plain Monte Carlo price (N = 100,000) | 9.3626 ± 0.0870 (95% CI) |
| Fitted convergence rate (log-log slope of the error) | −0.50 (theory: −0.5) |
| Pathwise Delta / Vega vs Black-Scholes | 0.5985 / 38.64 vs 0.5987 / 38.67 |
| Variance reduction, antithetic variates | 1.8x |
| Variance reduction, control variates (discounted stock price) | 5.9x |
| Variance reduction, arithmetic Asian call with geometric Asian control | 1,373x |

All Monte Carlo estimates fall within their confidence intervals around the closed-form values. An automated test cell at the end of the notebook checks this, along with put-call parity, the convergence rate and no-arbitrage bounds.

## Contents

1. **GBM simulation.** Exact simulation of terminal values and full paths (no discretization bias).
2. **Black-Scholes benchmark.** Closed-form price, Delta and Vega.
3. **Monte Carlo pricer.** Price, standard error and 95% confidence interval.
4. **Convergence study.** RMSE over 100 independent runs per sample size, with the convergence rate estimated by regression.
5. **Greeks.** Pathwise estimators of Delta and Vega, with confidence intervals, and a note on when the method fails (discontinuous payoffs).
6. **Antithetic variates.** Variance reduction using the symmetry of the normal distribution.
7. **Control variates.** The discounted stock price, whose expectation is known exactly, used as a control, with β estimated on a separate pilot run to avoid bias.
8. **Comparison of estimators.** Accuracy, run time and efficiency (variance reduction per unit of computing cost).
9. **Arithmetic Asian call.** A path-dependent option with no simple closed form, priced with the geometric Asian call (which has an exact formula) as a control variate.
10. **Sanity checks.** Automated assertions on all results.

## How to run

```bash
git clone https://github.com/olivierFer3/monte-carlo-option-pricing.git
cd monte-carlo-option-pricing
pip install -r requirements.txt
jupyter notebook monte_carlo_option_pricing.ipynb
```

The notebook runs in under a minute on a laptop. All experiments use fixed random seeds, so results are reproducible regardless of cell execution order. Run times in the comparison table depend on the machine.

## Tech stack

Python, NumPy (fully vectorized simulations), SciPy, Matplotlib.
