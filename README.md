### 🧠 Executive Overview

We are building a simulated bond market to test and evaluate market-making strategies. Since real bond trading data is costly and difficult to access, we are generating realistic synthetic data that mimics how bond prices and trading activity behave over time.

This project will help us:
- Understand how market makers quote prices and manage risk
- Experiment with how trading activity changes based on market conditions
- Build, test, and evaluate our own algorithm for making markets in bonds

This is a foundation we’ll build on. Over time, we’ll add complexity and realism. But first, we’re starting with a simple and controlled version of the market.

---

### 📈 Market Simulation – Version 1 (V1)

In Version 1, we generate 10 full trading days of second-by-second bond price and trade activity data.

Key components:

- **Mid-price**  
  Simulated using a Gaussian random walk:  
  `mid[t] = mid[t-1] + ε`, where `ε ~ N(0, 0.01²)`

- **Spread**  
  Random spread per second drawn from a uniform distribution:  
  `spread ~ U(0.01, 0.04)`

- **Bid/Ask**  
  `bid = mid - spread / 2`  
  `ask = mid + spread / 2`

- **k (spread sensitivity)**  
  `k ~ N(60, 2)` clipped to [30, 90]

- **Lambda (expected trade count)**  
  `λ = A * exp(-k * spread)`, where `A = 5`

- **Trade count**  
  `trade_count ~ Poisson(λ)`

- **Timestamps**  
  One row per second from 8:00 AM to 5:00 PM each day, across 10 days.
