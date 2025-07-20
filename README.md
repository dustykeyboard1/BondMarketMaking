# Bond Market Making Simulator

This repository simulates a fixed income market-making strategy based on short rate dynamics, CIR-based bond pricing, and spread-sensitive trade arrivals. The code is organized into modular notebooks.

## Contents

### CIR_Calibration.ipynb
Calibrates the Cox–Ingersoll–Ross (CIR) short rate model:

- Short rate SDE:  
  `dr = a(b - r)dt + σ√r dW`
- Closed-form zero-coupon bond price:  
  `P(t, T) = A(t, T) * exp(-B(t, T) * r(t))`

Parameters `a`, `b`, `σ`, and `r0` are fitted to market yield curves using CIR pricing formulas.

---

### Simulated_bond_data.ipynb
Simulates second-by-second short rates over 252 trading days using Euler discretization:

- `r[t+1] = r[t] + a(b - r[t])dt + σ√r[t] * √dt * Z`
- Bond mid-price computed at each timestep using CIR pricing formula with rolling `T`.

---

### market_making_backtest.ipynb
Simulates a market-making strategy quoting around CIR-based mid prices:

- Bid/ask quotes skewed based on inventory:
  `skew = γ * ln(|inventory|)`
  `bid = mid - spread/2 +- skew`
  `ask = mid + spread/2 +- skew`
- Trade arrival rates follow:  
  `λ = A * exp(-k * spread)`
- Buys/sells are drawn from Poisson distributions per second:
  - `λ_bid = 0.5 * A_bid * exp(-k_bid * (mid - bid))`
  - `λ_ask = 0.5 * A_ask * exp(-k_ask * (ask - mid))`
- Flow toxicity is estimated using a sigmoid function:
  - `probability trade is toxic = 1 / (1 + exp{-x})`
  - `x` represents the spread times sensitivity
  - larger spread, more likely to be toxic 

Liquidity parameters `A` and `k` are re-estimated every 1200 seconds using rolling regression of log volume vs. spread.

---

### InterestRateModels.pdf
Reference for CIR model equations (Hull). All simulation and pricing logic conforms to the continuous-time formulations specified in the source.

---
