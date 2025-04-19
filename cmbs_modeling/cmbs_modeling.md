# CMBS Modeling and Analysis

A Python library for modeling, analyzing, and visualizing Commercial Mortgage-Backed Securities (CMBS) structures and cash flows.

## Overview

This library provides tools for structured finance professionals to:

- Model CMBS deals with multiple tranches and underlying commercial mortgage loans
- Simulate cash flows under various default and prepayment scenarios
- Calculate key performance metrics for loans and tranches
- Generate visualizations for analytics and reporting

## Features

- **Loan Modeling**: Create and manage commercial mortgage loans with configurable parameters
- **Tranche Structure**: Build multi-tranche CMBS structures with customizable payment priorities
- **Cash Flow Simulation**: Project cash flows over time with realistic default and prepayment assumptions
- **Performance Metrics**: Calculate key metrics like Weighted Average Life (WAL), yields, and credit enhancement
- **Visualization Suite**: Generate comprehensive visualizations for CMBS performance analysis

## Installation

```bash
pip install cmbs-modeling
```

## Quick Start

```python
from cmbs_modeling import create_sample_cmbs

# Create a sample CMBS structure
cmbs = create_sample_cmbs()

# Run simulation for 24 periods (months)
for _ in range(24):
    cmbs.advance_period()

# Display results
tranche_metrics = cmbs.calculate_tranche_metrics()
print(tranche_metrics)

pool_metrics = cmbs.calculate_pool_metrics()
print(pool_metrics)

# Generate visualizations
from cmbs_modeling import plot_tranche_balances, plot_waterfall

# Plot tranche balance over time
plot_tranche_balances(cmbs)
plt.savefig('tranche_balances.png')

# Plot cash flow waterfall for the latest period
plot_waterfall(cmbs)
plt.savefig('waterfall.png')
```

## Core Components

### Loan Class

The `Loan` class represents a commercial mortgage loan with properties like:

- Balance and amortization details
- Interest rate and payment terms
- Property information (type, location)
- Credit metrics (LTV, DSCR)
- Default and prepayment characteristics

### Tranche Class

The `Tranche` class represents CMBS bond tranches with:

- Balance and coupon rate
- Payment priority in the waterfall
- Credit enhancement and attachment points
- Support for interest-only (IO) structures

### CMBSStructure Class

The `CMBSStructure` class manages the overall CMBS deal:

- Holds collections of loans and tranches
- Implements the payment waterfall logic
- Processes defaults, recoveries, and prepayments
- Tracks and reports on performance over time

## Visualization Suite

The library includes several visualization functions:

- `plot_tranche_balances`: Visualize tranche balance changes over time
- `plot_waterfall`: Create waterfall charts showing cash flow distribution
- `plot_loan_performance_heatmap`: Generate heatmaps of loan attributes
- `plot_credit_metrics`: Analyze loan credit characteristics
- `plot_losses_and_recoveries`: Track cumulative losses and recoveries
- `plot_tranche_yield_profile`: Compare tranche yields and credit enhancement
- `create_dashboard`: Generate a comprehensive multi-chart dashboard

## Example

```python
from cmbs_modeling import create_sample_cmbs, visualize_cmbs_example

# Generate a complete set of visualizations
visualize_cmbs_example()
```

This will create several visualization files including:
- tranche_balances.png
- waterfall.png
- loan_heatmap.png
- losses_recoveries.png
- yield_profile.png
- credit_metrics.png

## Advanced Usage

### Custom Loan Pool Creation

```python
from cmbs_modeling import Loan, Tranche, CMBSStructure
from datetime import datetime

# Create custom loans
loans = [
    Loan(
        id="L1",
        original_balance=10_000_000,
        current_balance=10_000_000,
        rate=0.045,  # 4.5%
        term_months=120,  # 10 year term
        amortization_months=360,  # 30 year amortization
        origination_date=datetime(2023, 1, 1),
        maturity_date=datetime(2033, 1, 1),
        property_type="Office",
        location="NY",
        ltv=0.65,  # 65%
        dscr=1.3,  # 1.3x
        probability_of_default=0.01,  # 1%
        loss_given_default=0.3  # 30%
    ),
    # Add more loans as needed
]

# Create custom tranches
tranches = [
    Tranche(
        id="A",
        original_balance=7_000_000,  # 70% of pool
        current_balance=7_000_000,
        rate=0.04,  # 4% coupon
        payment_priority=1,
        credit_enhancement=0.30,  # 30% CE
        attachment_point=0.30,
        detachment_point=1.0
    ),
    # Add more tranches as needed
]

# Create the CMBS structure
custom_cmbs = CMBSStructure(
    name="Custom CMBS 2023-1",
    cutoff_date=datetime(2023, 1, 1),
    tranches=tranches,
    loans=loans
)

# Run simulation
custom_cmbs.run_to_maturity()
```

## Requirements

- Python 3.7+
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Dataclasses (standard in Python 3.7+)

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.