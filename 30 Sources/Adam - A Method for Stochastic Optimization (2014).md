---
type: source
source-type: paper
authors: [Diederik P. Kingma, Jimmy Ba]
year: 2014
url: https://consensus.app/papers/details/36c2166a0a825faeb70f7fac42d5f82d/?utm_source=claude_desktop
doi: 10.48550/arxiv.1412.6980
topics: [deep-learning]
status: processed
rating: 5
created: 2026-08-25
aliases: [Adam paper, Kingma & Ba 2014]
---

# Adam - A Method for Stochastic Optimization (2014)

(~170,000 citations — likely the most-cited optimization paper in existence.)

## Summary

**Adam** (adaptive moment estimation): keep exponential moving averages of the gradient (first moment $m$) and its element-wise square (second moment $v$), correct their initialization bias, and take steps normalized per-parameter:

$$m_t = \beta_1 m_{t-1} + (1{-}\beta_1)g_t, \quad v_t = \beta_2 v_{t-1} + (1{-}\beta_2)g_t^2$$
$$\hat m_t = \tfrac{m_t}{1-\beta_1^t}, \;\; \hat v_t = \tfrac{v_t}{1-\beta_2^t}, \qquad \theta_{t+1} = \theta_t - \eta\, \frac{\hat m_t}{\sqrt{\hat v_t} + \epsilon}$$

Defaults $\beta_1{=}0.9$, $\beta_2{=}0.999$, $\epsilon{=}10^{-8}$. Effective step size is bounded ≈ $\eta$ regardless of gradient scale (invariant to diagonal rescaling).

## Key properties & results

- Suited to **noisy, sparse gradients and non-stationary objectives**; little tuning in practice
- Regret bound in the online convex framework (later shown flawed and repaired by AMSGrad — the fix rarely matters in practice)
- Empirically outperformed SGD-family on the tested tasks

## Impact

With the [[Decoupled Weight Decay Regularization - AdamW (2017)|AdamW]] correction, the de-facto optimizer of every transformer LM from [[BERT (2019)]] to the frontier. Cost: two fp32 state tensors per parameter — optimizer state, not weights, dominates training memory (motivating ZeRO-style sharding and 8-bit optimizers).

## Concepts extracted

- [[AdamW]] — the algorithm
