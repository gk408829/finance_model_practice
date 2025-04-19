# Options Pricing Tool

A comprehensive Python program for options pricing, Greeks calculation, and strategy analysis using functional programming principles.

## Overview

This tool provides a robust framework for pricing options using various models, calculating Greeks, analyzing option strategies, and visualizing results. It's designed with a functional programming approach for flexibility and composability.

## Features

### Pricing Models
- Black-Scholes-Merton model for European options
- Binomial tree model for American and European options
- Monte Carlo simulation for standard and path-dependent options
- Advanced models:
  - Heston stochastic volatility model
  - Merton jump-diffusion model
  - CEV (Constant Elasticity of Variance) model
  - SABR (Stochastic Alpha Beta Rho) model

### Greeks Calculation
- Delta ($\frac{\partial V}{\partial S}$): price sensitivity to underlying changes
- Gamma ($\frac{\partial^2 V}{\partial S^2}$): delta sensitivity to underlying changes
- Theta ($\frac{\partial V}{\partial t}$): price sensitivity to time decay
- Vega ($\frac{\partial V}{\partial \sigma}$): price sensitivity to volatility changes
- Rho ($\frac{\partial V}{\partial r}$): price sensitivity to interest rate changes

### Visualization Tools
- Option prices vs. underlying price
- Greeks profiles across strike prices
- 3D option price surfaces (price vs. strike vs. time)
- Volatility smile/skew visualizations
- Monte Carlo simulation convergence analysis
- Model comparison charts

### Options Strategy Builder
- Create and analyze custom multi-leg option strategies
- Built-in templates for common strategies:
  - Covered calls and protective puts
  - Vertical spreads (bull/bear call/put spreads)
  - Straddles and strangles
  - Butterflies and condors
- Profit/loss profiles at expiration
- Break-even points calculation
- Combined Greeks analysis for strategies

### Additional Tools
- Implied volatility calculation from market prices
- American vs. European option comparison
- Pricing model accuracy comparison

## Installation

```bash
# Dependencies
pip install numpy matplotlib scipy pandas seaborn
```

## Usage Examples

### Basic Option Pricing

```python
# Import the library
from options_pricing import *

# Set parameters
S = 100.0     # Current stock price
K = 100.0     # Strike price
r = 0.05      # Risk-free rate
sigma = 0.2   # Volatility
T = 1.0       # Time to expiration (1 year)

# Price a European call option using Black-Scholes
call_price = bsm_call_price(S, K, r, sigma, T)
print(f"Call option price: {call_price:.4f}")

# Price a European put option
put_price = bsm_put_price(S, K, r, sigma, T)
print(f"Put option price: {put_price:.4f}")

# Calculate Greeks
call_delta = delta_call(S, K, r, sigma, T)
gamma_value = gamma(S, K, r, sigma, T)
print(f"Call delta: {call_delta:.4f}")
print(f"Gamma: {gamma_value:.4f}")
```

### American Options with Binomial Tree

```python
# Price an American put option using binomial tree
am_put_price = binomial_tree_option(S, K, r, sigma, T, steps=100, option_type='put', exercise='american')
print(f"American put option price: {am_put_price:.4f}")

# Compare with European put
eu_put_price = binomial_tree_option(S, K, r, sigma, T, steps=100, option_type='put', exercise='european')
print(f"European put option price: {eu_put_price:.4f}")
```

### Option Strategy Analysis

```python
# Create a bull call spread strategy
bull_spread = OptionStrategy(S, r, sigma, T)
bull_spread.add_position('call', 95, 1)   # Buy lower strike call
bull_spread.add_position('call', 105, -1)  # Sell higher strike call

# Analyze the strategy
S_range = np.linspace(80, 120, 100)  # Range of stock prices to analyze
result, metrics = bull_spread.analyze_strategy(S_range, plot=True)

# Display key metrics
print(f"Strategy premium: ${metrics['Total Premium']:.2f}")
print(f"Max profit: ${metrics['Max Profit']:.2f}")
print(f"Max loss: ${metrics['Max Loss']:.2f}")
print(f"Break-even points: {[f'${be:.2f}' for be in metrics['Break-even Points']]}")
```

### Visualization

```python
# Plot option price vs stock price
S_range = np.linspace(50, 150, 100)
plot_option_price_vs_spot(S_range, K, r, sigma, T)

# Plot Greeks
plot_greeks_vs_spot(S_range, K, r, sigma, T, option_type='call')

# 3D price surface
T_range = np.linspace(0.1, 2, 50)
plot_3d_option_surface(bsm_call_price, S_range, T_range, K, r, sigma, 'call')
```

### Advanced Models

```python
# Heston model for stochastic volatility
kappa = 2.0      # Mean reversion speed
theta = 0.04     # Long-term variance
vol_of_vol = 0.3  # Volatility of variance
rho = -0.7        # Correlation
v0 = 0.04         # Initial variance

heston_price, _ = heston_model_option(S, K, r, T, kappa, theta, vol_of_vol, rho, v0, 
                                      simulations=5000, steps=100, option_type='call')
print(f"Heston model call price: {heston_price:.4f}")

# Merton jump-diffusion model
lam = 1.0    # Jump intensity (avg number of jumps per year)
mu_j = -0.1  # Mean jump size
sigma_j = 0.2  # Jump size volatility

jump_price = merton_jump_diffusion(S, K, r, sigma, T, lam, mu_j, sigma_j, 'call')
print(f"Jump-diffusion model call price: {jump_price:.4f}")
```

## Running Demonstrations

The tool includes two comprehensive demonstration functions:

```python
# Demonstrate core options pricing and Greeks
demonstrate_options_pricing()

# Demonstrate option strategies
demonstrate_option_strategies()
```

## Function Reference

### Core Pricing Functions
- `bsm_call_price(S, K, r, sigma, T)` - Black-Scholes-Merton call price
- `bsm_put_price(S, K, r, sigma, T)` - Black-Scholes-Merton put price
- `binomial_tree_option(S, K, r, sigma, T, steps, option_type, exercise)` - Binomial tree model
- `monte_carlo_option(S, K, r, sigma, T, simulations, option_type)` - Monte Carlo simulation
- `heston_model_option(S, K, r, T, kappa, theta, sigma, rho, v0, simulations, steps, option_type)` - Heston model
- `merton_jump_diffusion(S, K, r, sigma, T, lam, mu_j, sigma_j, option_type, simulations)` - Jump diffusion model
- `cev_model_option(S, K, r, beta, sigma, T, option_type, simulations, steps)` - CEV model
- `sabr_implied_vol(F, K, T, alpha, beta, rho, volvol)` - SABR model for implied volatility

### Greeks Functions
- `delta_call(S, K, r, sigma, T)` - Call option delta ($\Delta_{\text{call}}$)
- `delta_put(S, K, r, sigma, T)` - Put option delta ($\Delta_{\text{put}}$)
- `gamma(S, K, r, sigma, T)` - Gamma ($\Gamma$, same for calls and puts)
- `theta_call(S, K, r, sigma, T)` - Call option theta ($\Theta_{\text{call}}$)
- `theta_put(S, K, r, sigma, T)` - Put option theta ($\Theta_{\text{put}}$)
- `vega(S, K, r, sigma, T)` - Vega ($\mathcal{V}$, same for calls and puts)
- `rho_call(S, K, r, sigma, T)` - Call option rho ($\rho_{\text{call}}$)
- `rho_put(S, K, r, sigma, T)` - Put option rho ($\rho_{\text{put}}$)

### Visualization Functions
- `plot_option_price_vs_spot(S_range, K, r, sigma, T, option_types)` - Plot option prices vs stock price
- `plot_greeks_vs_spot(S_range, K, r, sigma, T, option_type)` - Plot option Greeks vs stock price
- `plot_3d_option_surface(option_pricer, S_range, T_range, K, r, sigma, option_type)` - 3D option price surface
- `plot_volatility_smile(strikes, market_prices, S, r, T, option_type)` - Plot volatility smile from market prices
- `plot_binomial_vs_bsm(S, K, r, sigma, T, option_type, exercise, max_steps)` - Compare binomial and BSM prices
- `plot_monte_carlo_convergence(S, K, r, sigma, T, option_type, max_sims, steps)` - Plot Monte Carlo convergence
- `plot_heston_paths(S, K, r, T, kappa, theta, sigma, rho, v0, simulations, steps)` - Plot Heston model paths
- `compare_pricing_models(S, K, r, sigma, T, option_type)` - Compare different pricing models
- `plot_sabr_vol_surface(F_range, K_range, T_range, alpha, beta, rho, volvol)` - Plot SABR vol surface

### Strategy Analysis
- `OptionStrategy` class - Create and analyze option strategies
  - `add_position(option_type, K, quantity, premium)` - Add option position
  - `analyze_strategy(S_range, plot)` - Analyze strategy payoff and Greeks
  - Various helper methods for calculations and plotting

## Mathematical Foundation

The tool implements the following mathematical models:

### Black-Scholes-Merton Model
- Call price: $C = S \cdot N(d_1) - K \cdot e^{-rT} \cdot N(d_2)$
- Put price: $P = K \cdot e^{-rT} \cdot N(-d_2) - S \cdot N(-d_1)$
- Where:
  - $d_1 = \frac{\ln(S/K) + (r + \sigma^2/2) \cdot T}{\sigma \cdot \sqrt{T}}$
  - $d_2 = d_1 - \sigma \cdot \sqrt{T}$

### Binomial Tree Model
- Uses risk-neutral valuation with discrete time steps
- Allows for early exercise (American options)
- Up factor: $u = e^{\sigma\sqrt{\Delta t}}$
- Down factor: $d = e^{-\sigma\sqrt{\Delta t}}$
- Risk-neutral probability: $p = \frac{e^{r\Delta t} - d}{u - d}$

### Greeks Formulas
- **Delta** ($\Delta$): 
  - Call: $\Delta_{\text{call}} = N(d_1)$
  - Put: $\Delta_{\text{put}} = N(d_1) - 1$
- **Gamma** ($\Gamma$):  
  - $\Gamma = \frac{N'(d_1)}{S \cdot \sigma \cdot \sqrt{T}}$
- **Theta** ($\Theta$): 
  - Call: $\Theta_{\text{call}} = -\frac{S \cdot \sigma \cdot N'(d_1)}{2 \cdot \sqrt{T}} - r \cdot K \cdot e^{-rT} \cdot N(d_2)$
  - Put: $\Theta_{\text{put}} = -\frac{S \cdot \sigma \cdot N'(d_1)}{2 \cdot \sqrt{T}} + r \cdot K \cdot e^{-rT} \cdot N(-d_2)$
- **Vega** ($\mathcal{V}$):  
  - $\mathcal{V} = S \cdot \sqrt{T} \cdot N'(d_1)$
- **Rho** ($\rho$): 
  - Call: $\rho_{\text{call}} = K \cdot T \cdot e^{-rT} \cdot N(d_2)$
  - Put: $\rho_{\text{put}} = -K \cdot T \cdot e^{-rT} \cdot N(-d_2)$

### Heston Model
- Stock price:  
    $dS_t = \mu S_t dt + \sqrt{v_t} S_t dW^S_t$
- Variance:  
    $dv_t = \kappa(\theta - v_t)dt + \sigma_v \sqrt{v_t}dW^v_t$
- With correlation:  
    $\text{Corr}(dW^S_t, dW^v_t) = \rho$

### Merton Jump-Diffusion Model
- Stock price:  
    $dS_t = (\mu - \lambda k)S_t dt + \sigma S_t dW_t + S_t dJ_t$
- Jump component: $dJ_t$ is a compound Poisson process with intensity $\lambda$
- Jump sizes: $(Y_i - 1)$ are i.i.d. with expected value $k = E[Y - 1]$

## Contributing / Commenting

Contributions and comments are welcome! 

## License

This project is available under the MIT License.