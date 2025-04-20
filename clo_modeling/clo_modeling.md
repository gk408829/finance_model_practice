# CLO Modeling and Analysis Tool

## Overview

This Python library provides a comprehensive framework for modeling, simulating, and analyzing Collateralized Loan Obligations (CLOs). It allows users to create realistic CLO structures, run simulations under various market conditions, and perform advanced risk analysis on CLO tranches.

## Features

- **Loan Modeling**: Create individual loans with customizable parameters including principal, interest rate, maturity, default probability, recovery rate, and prepayment probability.
- **Collateral Pool Management**: Build diversified loan pools with industry and rating classifications.
- **CLO Structure Creation**: Define multi-tranche CLO capital structures with customizable subordination levels and coupon rates.
- **Cash Flow Waterfall**: Implement the complete CLO waterfall mechanism including interest and principal allocation, coverage tests, and reinvestment periods.
- **Simulation Capabilities**: 
  - Run deterministic simulations to maturity
  - Perform Monte Carlo simulations for probabilistic analysis
  - Apply stress scenarios to test CLO resilience
- **Advanced Analytics**:
  - Performance metrics calculation (IRR, default rates, recovery rates)
  - Coverage test tracking (OC and IC ratios)
  - Tranche performance analysis
  - Risk metrics (VaR, Expected Shortfall, concentration risk)
- **Visualization Tools**:
  - Capital structure waterfall charts
  - Cash flow diagrams
  - Coverage test trends
  - Sensitivity analysis plots

## Requirements

- Python 3.6+
- NumPy
- Pandas
- Matplotlib
- SciPy

## Installation

```bash
# Install required packages
pip install numpy pandas matplotlib scipy
```

## Quick Start

```python
from clo_modeling import create_sample_clo, CLOAnalyzer

# Create a sample CLO structure
clo = create_sample_clo()

# Run simulation to maturity
clo.simulate_to_maturity()

# Get performance summary
summary = clo.get_performance_summary()
print(f"Equity IRR: {summary['equity_irr']:.2%}")
print(f"Default rate: {summary['default_rate']:.2%}")

# Generate visualizations
waterfall_plot = CLOAnalyzer.plot_waterfall(clo)
cashflow_plot = CLOAnalyzer.plot_cash_flows(clo)
coverage_plot = CLOAnalyzer.plot_coverage_tests(clo)

# Show plots
waterfall_plot.show()
```

## Core Components

### Loan Class

Models individual loans in the collateral pool:

```python
loan = Loan(
    principal=1000000,           # Loan amount
    interest_rate=0.06,          # 6% annual interest
    maturity_years=5,            # 5-year maturity
    default_prob=0.03,           # 3% annual default probability
    recovery_rate=0.60,          # 60% recovery in default
    prepayment_prob=0.05,        # 5% annual prepayment probability
    industry="Technology",       # Industry classification
    rating="BB"                  # Credit rating
)
```

### CollateralPool Class

Manages a collection of loans:

```python
pool = CollateralPool()

# Add individual loans
pool.add_loan(loan)

# Add multiple random loans
pool.add_random_loans(
    count=100,                   # 100 loans
    avg_principal=1e6,           # $1M average size
    avg_rate=0.06,               # 6% average rate
    avg_maturity=5               # 5-year average maturity
)
```

### CLOStructure Class

Defines the complete CLO structure:

```python
clo = CLOStructure(pool)

# Add tranches
clo.add_tranche(70000000, 0.035, 0.95, "Class A")  # Senior tranche
clo.add_tranche(10000000, 0.045, 0.85, "Class B")  # Mezzanine tranche
clo.add_tranche(5000000, 0.065, 0.73, "Class D")   # Junior tranche

# Set equity amount
clo.set_equity(5000000)
```

### CLOAnalyzer Class

Provides analysis tools:

```python
analyzer = CLOAnalyzer()

# Run stress tests
stress_scenarios = {
    'baseline': {'default_multiplier': 1.0, 'recovery_multiplier': 1.0},
    'severe_stress': {'default_multiplier': 3.0, 'recovery_multiplier': 0.5}
}
results = analyzer.stress_test(clo, stress_scenarios)

# Generate visualizations
analyzer.plot_waterfall(clo)
analyzer.plot_cash_flows(clo)
```

### CLORiskAnalyzer Class

Performs advanced risk analysis:

```python
risk_analyzer = CLORiskAnalyzer()

# Calculate Value at Risk
var_results = risk_analyzer.calculate_var(clo, confidence_level=0.95)
print(f"Equity VaR (95%): {var_results['equity_var']:.2%}")

# Analyze concentration risk
concentration = risk_analyzer.calculate_concentration_risk(clo.collateral_pool)
```

## Advanced Usage Examples

### Running Monte Carlo Simulations

```python
# Run 1000 Monte Carlo simulations
mc_results = CLOAnalyzer.run_monte_carlo_simulation(clo, num_simulations=1000)

# Analyze distribution of results
equity_returns = [result['equity_irr'] for result in mc_results]
mean_return = sum(equity_returns) / len(equity_returns)
```

### Advanced Scenario Modeling

```python
scenarios = {
    'base_case': 0.5,    # 50% probability
    'recession': 0.3,    # 30% probability
    'expansion': 0.2     # 20% probability
}

stress_factors = {
    'base_case': {'default': 1.0, 'recovery': 1.0, 'prepayment': 1.0},
    'recession': {'default': 2.5, 'recovery': 0.6, 'prepayment': 0.5},
    'expansion': {'default': 0.7, 'recovery': 1.2, 'prepayment': 1.5}
}

scenario_results, expected_values = advanced_scenario_modeling(clo, scenarios, stress_factors)
```

### Sensitivity Analysis

```python
variables = ['default_rate', 'recovery_rate', 'interest_rates']
ranges = {
    'default_rate': [0.01, 0.03, 0.05, 0.08, 0.12],
    'recovery_rate': [0.3, 0.5, 0.6, 0.7, 0.8],
    'interest_rates': [0.03, 0.05, 0.06, 0.07, 0.08]
}

sensitivity_results = perform_sensitivity_analysis(clo, variables, ranges)
sensitivity_plot = plot_sensitivity_analysis(sensitivity_results)
```

## CLO Concepts

The tool models the following key CLO concepts:

- **Waterfall Structure**: Sequential payment of interest and principal according to tranche seniority
- **Coverage Tests**: Overcollateralization (OC) and Interest Coverage (IC) tests that protect senior noteholders
- **Reinvestment Period**: Initial period where principal proceeds are reinvested rather than used to pay down notes
- **Cash Flow Diversion**: Redirecting cash flows to protect senior tranches when coverage tests fail

## Model Limitations

- The model uses simplified assumptions for loan amortization (interest-only loans by default)
- Default correlation modeling is basic and can be enhanced for more sophisticated analysis
- The current implementation does not model detailed CLO manager behavior for reinvestment choices
- Market value calculations and mark-to-market tests are not currently implemented

## Contributing

Contributions are welcome! Areas for potential enhancement include:

- Incorporating more sophisticated correlation models for defaults
- Adding market value calculations and tests
- Implementing detailed reinvestment logic
- Building a GUI interface for easier usage

## License

[MIT License](LICENSE)