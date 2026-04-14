# Deep Reinforcement Learning Trading Agent

**Module:** 25COC102 Advanced AI — Loughborough University  
**Author:** Hardhik Tottempudi

---
<img src="https://github.com/user-attachments/assets/a82c3299-f490-43a3-be78-90dee74bcb77" width="50%">

## Overview

This project implements a **Deep Q-Network (DQN) trading agent** in PyTorch, trained on AAPL stock (2012–2026) to make daily buy/hold decisions. The notebook serves as an end-to-end tutorial covering:

- Data preparation and feature engineering
- Custom trading environment design (with transaction costs, slippage, and drawdown penalties)
- Double DQN agent training
- Leakage-safe chronological backtesting
- Hyperparameter sensitivity experiments
- Benchmarking against Buy & Hold and moving-average baselines

---

## Results

### Final Test Set Performance

| Strategy     | Total Return | Annual Return | Sharpe  | Max Drawdown | Win Rate |
|--------------|:------------:|:-------------:|:-------:|:------------:|:--------:|
| DQN (Tuned)  |   +12.7%     |    +4.4%      | +0.62   |   -9.37%     |  12.6%   |
| Buy & Hold   |   +82.6%     |   +24.2%      | +1.59   |  -18.76%     |  57.9%   |

> **Key takeaway:** Even after significant hyperparameter tuning, Buy & Hold outperformed the DQN agent — a reminder that complex RL strategies do not automatically beat simple baselines in trending markets.

### Improvement Journey (DQN only)

| Metric         | Baseline DQN | Tuned DQN  | Change        |
|----------------|:------------:|:----------:|:-------------:|
| Total Return   |   -32.7%     |  +12.7%    | **+45.4 pp**  |
| Sharpe Ratio   |   -1.10      |  +0.62     | **+1.72**     |
| Max Drawdown   |   -39.3%     |   -9.4%    | **+29.9 pp**  |

---

## Repository Structure

```
├── final/
│   ├── Hardhik_Tottempudi_F312851.ipynb   # Main Jupyter notebook (submit version)
│   └── Hardhik_Tottempudi_F312851.html    # Rendered HTML export
├── MODEL_IMPROVEMENT.md                    # DQN tuning journey & hyperparameter analysis
├── image.png                               # Equity curve chart
├── image-1.png                             # Drawdown curve chart
└── README.md
```

---

## Key Features

- **Custom `TradingEnv`** — gym-style environment with:
  - Transaction costs + slippage
  - Minimum hold period to reduce overtrading
  - Drawdown penalty in the reward signal
  - Long-only mode for bull-market stability
- **Double DQN** — experience replay, target network, gradient clipping
- **No data leakage** — strict chronological 60/20/20 train/val/test split; scaler fitted on train only
- **Hyperparameter search** — Sharpe-based model selection on validation set
- **Honest evaluation** — risk-adjusted metrics (Sharpe, max drawdown) alongside raw returns

---

## Tech Stack

| Tool | Purpose |
|------|---------|
| Python 3.x | Core language |
| PyTorch | DQN model and training |
| yfinance | AAPL historical price data |
| pandas / numpy | Data manipulation |
| matplotlib | Visualisation |
| scikit-learn | Feature scaling (`StandardScaler`) |

---

## How to Run

1. **Install dependencies:**
   ```bash
   pip install yfinance pandas numpy matplotlib torch scikit-learn
   ```

2. **Open the notebook:**
   ```bash
   jupyter notebook "final/Hardhik_Tottempudi_F312851.ipynb"
   ```

3. Run all cells top-to-bottom. The notebook will:
   - Download AAPL data from Yahoo Finance automatically
   - Engineer features, split data, and train the DQN agent
   - Run backtests and display results tables + equity/drawdown charts

---

## Hyperparameter Tuning Summary

| Parameter        | Baseline → Tuned | Effect |
|------------------|:----------------:|--------|
| `long_only`      | `False` → `True` | Reduced harmful shorts in a bullish market |
| `min_hold`       | `3` → `5`        | Less overtrading, lower drawdown |
| `trade_penalty`  | `0.0005` → `0.0002` | Better net returns per trade |
| `drawdown_lambda`| `0.05` → `0.02`  | Less aggressive penalty, better upside |
| `lr`             | `5e-4` → `3e-4`  | More stable, less noisy learning |
| `hidden`         | `128` → `64`     | Simpler model, less overfitting |

---

## References

1. Mnih et al. (2015). *Human-level control through deep reinforcement learning.* Nature, 518, 529–533.
2. PyTorch DQN Tutorial (CartPole): https://docs.pytorch.org/tutorials/intermediate/reinforcement_q_learning.html
3. TorchRL DQN coding tutorial: https://docs.pytorch.org/rl/stable/tutorials/coding_dqn.html
4. FinRL library: https://github.com/manojravindran90/FinRL-Library
5. Dataset: Yahoo Finance via `yfinance` — AAPL, 2012-01-01 to 2026-01-01

---

*Submitted for 25COC102 Advanced AI, Loughborough University, 2026.*
