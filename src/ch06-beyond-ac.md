# Beyond Almgren–Chriss: robustness, supply and demand, stochastic control

The Almgren–Chriss model of Chapter 5 answers one question exactly and hides four others. It assumes the unaffected price has no drift and a constant volatility, that permanent and temporary impact are linear in the trading rate, and that liquidity is a fixed constant the trader can neither exhaust nor replenish. It measures performance by expected cost plus a variance penalty, a criterion that is not time-consistent. And it fixes a schedule in advance. Each assumption has been relaxed, and each relaxation changes both the model and the shape of the optimal strategy. This chapter follows that map, using the classification of Donnelly (2022) to place each result.

## A map of the alternatives

Donnelly (2022) fixes the baseline in discrete and continuous time and organises the later literature into alternative *dynamics* (stochastic impact, stochastic volatility, resilience and transient impact, non-linear impact), alternative *objectives* (signals, hedging, VWAP targeting) and other *phenomena* (multiple agents, robustness, limit-order strategies). Its value is as much negative as positive: it is a catalogue of exactly which assumption each paper relaxes, and of what that costs. The continuous baseline most results below modify takes trading speed $\nu_t$, inventory $dQ^\nu_t = \nu_t\,dt$, midprice $dS^\nu_t = f(\nu_t)\,dt + dM_t$ and execution price $\tilde S^\nu_t = S^\nu_t + g(\nu_t)$, maximising $H^\nu = \mathbb{E}\left[X^\nu_T + Q^\nu_T\left(S^\nu_T - l(Q^\nu_T)\right)\right]$.

## Dependence on drift and direction

The mean–variance objective has two defects that Gatheral and Schied (2011) set out to remove: it is time-inconsistent, and a mean–variance optimum under a non-constant price law needs heavy numerics, with counterintuitive comparative statics as a bonus. They replace the variance penalty by the expectation of a time-averaged Value-at-Risk of the position, minimising

$$\mathbb{E}\left[C(x) + \int_0^T \tilde\lambda\, x_t\left(S^0_t + \gamma x_t\right)dt\right]$$

under a geometric Brownian unaffected price. For $\gamma>0$ the optimal adaptive strategy is

$$x^*_t = \sinh\!\left(\nu(T-t)\right)\left[\frac{X}{\sinh(\nu T)} - \frac{\lambda}{2\nu}\int_0^t \frac{S^0_s}{1 + \cosh(\nu(T-s))}\,ds\right],$$

with $\lambda = \tilde\lambda/\eta$ and $\nu^2 = \tilde\lambda\gamma/\eta$; for $\gamma = \nu = 0$ it is $x^0_t = \frac{T-t}{T}X - \frac{\lambda T}{4}\int_0^t S^0_s\,ds$. Both are *aggressive in the money* — shares are sold faster when the price rises — and both are *independent of the volatility* $\sigma$.

Schied (2013) proves a general statement: if $S^0$ is *any* right-continuous square-integrable martingale, the unique minimiser of $\mathbb{E}\int_0^T(\dot x_t^2 + \lambda S^0_t x_t + \nu^2x_t^2)\,dt$ is exactly $x^*_t$, and the same strategy minimises the robust functional $\sup_{Q\in\mathcal Q}\mathbb{E}_Q[\cdot]$ over any set of equivalent martingale measures. So optimality rests on the *martingale property*, not on the particular law. Dropping that assumption does not discard the drift: it enters *in integrated form*. For a general square-integrable semimartingale, writing $Y_t := -(S^0_t - S_0)/\eta + \lambda\int_0^t S^0_s\,ds$, the optimal strategy of Theorem 2 integrates conditional expectations of $\int \sinh(\nu(T-u))\,dY_u$; the pure expected-cost corollary, $x^0_t = \frac{T-t}{T}X - \frac{1}{2\eta}\int_0^t\frac{T}{(T-s)^2}\mathbb{E}\!\left[\int_s^T (T-u)\,dS^0_u \mid \mathcal F_s\right]ds$, contains the drift $A$ of the decomposition $S^0 = S_0 + M + A$ only in integrated form. Schied's reading is that drift misspecification "may average out".

Under *transient* impact the picture reverses. Lorenz and Schied (2013) solve the same expected-cost problem in the Obizhaeva–Wang linear transient-impact model with a general drift $A$. Their Theorem 1 is sharp: if $A$ is not absolutely continuous the infimum of expected costs is $-\infty$ — no optimal strategy exists, because the trader can exploit the lag between a shock and the book's recovery without bound. If $A$ is absolutely continuous with square-integrable derivative $A'$, the bounded-variation and semimartingale classes share the same finite infimum,

$$\inf \mathbb{E}[C(X)] = -xS_0 + \frac{x^2}{2+\rho T} + \frac{xY_0}{2+\rho T} - \frac{\rho}{4}\mathbb{E}\int_0^T\!\left(\frac{Y_s}{2+\rho(T-s)} - \frac{1}{\rho}A'_s\right)^2 ds,$$

and when $A'$ is bounded the unique optimum has the position depending directly on the *derivative of the drift* $A'_t$, so it can fluctuate strongly. Under AC the drift enters in integrated form and misspecification is benign; under transient impact it enters through its derivative and misspecification is dangerous.

## Linear versus non-linear impact

AC takes both impact functions linear; the empirical record does not. Bouchaud (2010) reports that impact is concave in volume, a power law $R \propto v^{\psi}$ with $\psi\le1$: roughly 0.1 to 0.3 for individual trades and $\psi\simeq 1/2$ for a firm's own metaorders, the square-root law $R = A\,\sigma\sqrt{v/V}$ also found by Almgren et al. (2005). Concavity is an *empirical regularity*, not a theorem, and it is exactly the regime the linear model excludes.

Two responses. The first is theoretical. Gatheral's theorem, restated by Guéant (2013), says that in the AC decomposition $dS_t = \sigma\,dW_t - k(v_t)\,dt$, if $k(v) = kv$ then every closed loop $q(0)=q(T)$ has $\mathbb{E}[X_T]\le X_0$, while if the temporary-impact cost $h$ vanishes and $k$ is not linear a dynamic arbitrage exists: linear permanent impact is the only manipulation-free specification. Guéant reconciles this with concave impact by letting the *microscopic* impact depend on the volume already executed, $dS_t = \sigma\,dW_t - f(|q_0-q_t|)v_t\,dt$ with $f>0$ decreasing. His Proposition 2 shows that any absolutely continuous closed loop has $\mathbb{E}[X_T] = X_0 - \int_0^T h(t,v_t)v_t\,dt \le X_0$: no dynamic arbitrage. His Proposition 3 gives the macroscopic impact of a completed trade, $S_{T'} - S_0 = -F(q_0) + \sigma W_{T'}$ with $F(z) = \int_0^z f(|y|)dy$, which under the power law $f(q) = q^{1-\alpha}$ is $F(z) = z^{2-\alpha}/(2-\alpha)$ — concave, and so able to fit the square-root law while remaining arbitrage-free. The concavity moves out of the *assumed* impact function and into the *state dependence* of the microscopic one.

The second response is Almgren (2003), which gives the temporary impact a random component, $\tilde S_n = S_n + g(V_n) + h(V_n)\xi_{n+1}$. With deterministic trades the terminal-wealth variance becomes $V[X_T] = \sum_n \sigma^2 Q_n^2 + \sum_n V_n^2 h^2(V_n)$: the second term is **trading-enhanced risk** — cost variance grows with the size of the trades, because large trades are more exposed to variability in the liquidity they meet — and it brakes the schedule. This paper was not obtained; the model and paths below are taken from Donnelly's (2022) §4.4 account. For power temporary impact $g(\nu) = k\nu^m$ with no stochastic part, the optimal path is $Q^{\nu*}_t = q_0\left(1 + \frac{1-m}{1+m}\frac{t}{T^*}\right)^{(1+m)/(1-m)}$ for $0<m<1$, the exponential $q_0 e^{-t/T^*}$ for $m=1$, and $q_0\left(1 - \frac{m-1}{m+1}\frac{t}{T^*}\right)^{(m+1)/(m-1)}$ for $m>1$; for stochastic but size-independent impact $h(\nu) = a$ it is again exponential, with $T^* = (2k+\gamma a^2)/(\gamma\sigma^2)$. Stochastic impact slows trading: the trader accepts more price risk to reduce the risk of meeting bad liquidity.

## Random liquidity and volatility

If impact coefficients and volatility move, the constant-parameter schedule is wrong. The plug-in heuristic recomputes a constant-parameter strategy with the current values, $k\to k_t$ and $\sigma\to\sigma_t$: trade quickly when impact is low, volatility high, or the horizon near. This is the relaxation studied by Almgren (2009), reported here from Donnelly (2022) §4.1–4.2 because the paper itself was not obtained.

Forsyth (2011) shows why these problems need numerical methods. He embeds the *pre-commitment* mean–variance objective — the trade-off evaluated at the initial time, which the trader then commits to — into a linear–quadratic problem via a Lagrange multiplier, and solves the resulting non-linear HJB equation with a **semi-Lagrangian** scheme that is monotone, consistent and stable, hence convergent to the **viscosity solution** provided a strong comparison principle holds. One non-linear solve plus one linear solve produces the *entire* efficient frontier under geometric Brownian motion, without requiring a quadratic impact function. The headline numerical finding warns about the criterion rather than the schedule: "in some cases there are many different trading strategies which generate almost identical efficient frontiers". The AC schedule is one member of a large nearly-optimal family.

## Supply and demand: why deterministic strategies stop being optimal

Every model so far assumes the trader's own trades are the only thing that moves the book, and that impact is a function of the trade. Obizhaeva and Wang (2013) discard both. They model a one-sided limit order book: a trade of size $x_0$ shifts the mid-quote linearly, $V_{0+} = F_0 + \lambda x_0$, the ask jumps to $A_{0+} = F_0 + s/2 + x_0/q$ and then relaxes to a steady state exponentially,

$$A_t = V_t + s/2 + \sum_{t_i<t} x_{t_i}\,\kappa\,e^{-\rho(t-t_i)}, \qquad \kappa = 1/q - \lambda,$$

where $\rho$ is the **resilience**, the speed at which new orders replenish the gap, and $D_t = A_t - V_t - s/2$ is the deviation of the ask from its steady state. The degenerate AC/Bertsimas–Lo limit, in which cost collapses to the strategy-independent $(F_0+s/2)X_0 + \frac\lambda2 X_0^2$, is repaired by modelling the *dynamics* of supply rather than a static impact function.

Their Proposition 3 gives the value function $J_t = (F_t+s/2)X_t + \lambda X_0 X_t + \alpha_t X_t^2 + \beta_t D_t + \gamma_t D_t^2$ and the optimal schedule

$$x_0 = x_T = \frac{X_0}{\rho T + 2}, \qquad \mu_t = \frac{\rho X_0}{\rho T + 2}\ \text{ for } t\in(0,T).$$

Two claims sit in that formula. First, the optimum is not continuous: two discrete lumps at $t=0$ and $T$ with continuous trading at a *constant rate* in between, so the AC conclusion that a smooth deterministic schedule is optimal is false once trade timing is endogenous and supply replenishes. Second, the strategy does not depend on the static impact parameters $\lambda$ and $q$ at all, only on the dynamic property $\rho$ and the horizon $T$: static supply and demand do not determine the schedule, the resilience of the book does. As $\rho\to0$ the strategy degenerates to two pure blocks, and as $\rho\to\infty$ to a purely continuous flow.

The optimum also lowers cost. Against the conventional constant-rate benchmark, whose excess over the fundamental value is $(\lambda/2)X_0^2 + \kappa\frac{\rho T - (1-e^{-\rho T})}{(\rho T)^2}X_0^2$, the optimal strategy pays $(\lambda/2)X_0^2 + \frac{\kappa}{\rho T+2}X_0^2$, and the difference is always non-negative. The magnitude is regime-dependent: for $X_0 = 100{,}000$ shares, an initial ask of 100, book depth $q = 5{,}000$ and one trading day, the boundary lumps fall from 49,975 shares at $\rho = 0.001$ to 14,286 at $\rho = 5$ and 1,921 at $\rho = 50$, while the continuous portion grows to fill the gap.

## Transient impact and price manipulation

Obizhaeva–Wang assume a block-shaped book. Predoiu, Shaikhet and Shreve (2011) generalise to an arbitrary one-sided book, described by a shadow measure with cumulative distribution $F$, so the ask is $A_t + D_t$ with $D_t = \psi(E_t)$ and $\psi$ the inverse of $F$. The **volume effect process** $E$ — the part of the trade still visible in the book — follows

$$E_t = X_t - \int_0^t h(E_s)\,ds,$$

with $h$ the book's recovery rate. With the unaffected price normalised to zero, the cost of a deterministic non-decreasing strategy is $C(X) = \Phi(E_T) + \int_0^T D_t h(E_t)\,dt$ with $\Phi(y) = \int_0^y\psi(\eta)\,d\eta$. Their main result (Theorems 4.2 and 4.5) says that when $g(y) := y\psi(h^{-1}(y))$ is convex there is a unique optimal strategy of **Type A**: lump purchases at $0$, at an intermediate time $t_0$ and at $T$, with constant-rate purchasing in between, chosen so the ask keeps a constant spread over the recovery path. When $g$ is not convex the optimum is the convex hull of $g$ and a two-lump "Type B" construction attains it. The structural conclusion — discrete boundary lumps plus a constant interior rate — is robust across book shapes, a strong argument for shape-independent execution algorithms.

What rules out pathologies here is the prohibition on **price manipulation**: starting from zero, trading back and forth, and ending at zero with a profit. Alfonsi and Schied (2010) are the reference for the general condition; their paper was not obtained, and I report its conclusion only as Predoiu–Shaikhet–Shreve state it, that with finitely many trading times "price manipulation is not possible under very general conditions", and that intermediate sells are never advantageous when executing an overall buy order. In the linear transient-impact model, Donnelly (2022) notes that concavity requires $2\alpha > b$ (liquidation penalty against permanent impact); under the opposite inequality the agent "could make infinite profits through price manipulations". In the AC decomposition, Gatheral's theorem as restated by Guéant (2013) gives the exact boundary: linear permanent impact is manipulation-free, and any non-linear one with vanishing temporary cost admits a dynamic arbitrage.

## Block trades: turning execution cost into a price

A schedule answers *how* to trade; Guéant (2015) answers *what a block is worth*. His framework is a CARA investor liquidating $q_0$ shares with a general strictly convex, superlinear execution-cost function $L$ and a deterministic volume curve $V_t$, with

$$dS_t = \sigma\,dW_t - f(|q_0-q_t|)v_t\,dt, \qquad dX_t = v_t S_t\,dt - V_t L\!\left(\tfrac{v_t}{V_t}\right)dt - \psi|v_t|\,dt,$$

so that AC is the case $L(\rho) = \eta\rho^2$. Theorem 2.1 gives a unique minimiser $q^*$, non-negative, non-increasing and independent of the linear fee $\psi$; Proposition 2.2 characterises it by the Hamiltonian system $\dot p = \gamma\sigma^2 q$, $\dot q = V_t H'(p)$ with $H$ the Legendre transform of $L$ and boundary conditions $q(0)=q_0$, $q(T)=0$ — a characterisation that does not assume $q^*$ is smooth and that recovers the $\sinh$ path in the AC case. Theorem 2.3 makes the discrete-time intuition precise: no stochastic strategy beats the best deterministic one, $\sup_{v\in\mathcal A}\mathbb{E}[-\exp(-\gamma X_T)] = \sup_{v\in\mathcal A^{0,\det}}\mathbb{E}[-\exp(-\gamma X_T)]$ — which fails for Black–Scholes prices, outside CARA, or with stochastic volume.

The pricing result is indifference pricing. The maximum price for a portfolio of $q_0$ shares that must be liquidated over $[0,T]$ is

$$P(T,q_0,S_0) = q_0S_0 - \int_0^{q_0} F(z)\,dz - \psi q_0 - I_0(q^*),$$

with $F(z) = \int_0^z f(|y|)dy$. The four terms are the mark-to-market value, the strategy-independent permanent-impact discount, the linear cost, and the *optimised* cost of risk and impact $I_0(q^*)$ — the only part that trading well can reduce. Here $I_0(q^*)$ comes from the value function

$$\theta_T(t,q) = \inf_{q\in\mathcal{A}_{q,0}(t,T)} \int_t^T \left( V_s L(\dot q / V_s) + \tfrac12\gamma\sigma^2 q^2 \right) ds,$$

so the price of liquidating $q$ shares over a window of length $t$ is $qS - \int_0^qF(z)dz - \psi q - \theta_T(T-t,q)$. Execution cost has become a quoted number: the liquidity premium a broker charges for warehousing a block.

## Time inconsistency and pre-commitment

The variance penalty is the AC model's deepest structural problem. Because $\mathrm{Var}(C)$ contains a squared expectation, the mean–variance criterion is not time-consistent: a strategy optimal at time 0 is no longer optimal later, so dynamic programming does not apply directly. Forsyth (2011) makes the pre-commitment interpretation explicit — solve the initial-time trade-off, then commit — and notes that it is optimal only in the averaged sense: over many identical trades it dominates every alternative in expected gain for a given standard deviation. Forsyth, Kennedy, Tse and Windcliff (2012) instead replace variance by **quadratic variation**, $\mathbb{E}[\text{cost}] + \kappa_1\mathbb{E}\int_0^T(\text{inventory})^2dt + \kappa_2\mathbb{E}[\text{variation}]$, which is time-consistent and takes a standard HJB treatment. Their striking result is that the exact arithmetic-Brownian optimum *coincides with the static AC solution*, while under geometric Brownian motion the optimum becomes price-dependent, though the static strategy remains an excellent approximation even at high volatility.

Schied and Schöneborn (2009) keep time consistency by replacing the criterion altogether: they maximise expected utility of terminal wealth under a general von Neumann–Morgenstern utility with bounded absolute risk aversion, characterising the adaptive strategy by a non-linear HJB equation whose optimal feedback is $c(X,R) = -v_X/(2\lambda v_R)$. Their Theorem 5 ties the direction of adaptation to the utility function: the selling speed is increasing in wealth if and only if absolute risk aversion is increasing, so decreasing, constant and increasing absolute risk aversion correspond respectively to passive-, neutral- and aggressive-in-the-money behaviour. Two consequences matter. Under CARA the optimal strategy is exactly the deterministic exponential path $X_0\exp(-t\sqrt{\sigma^2A/(2\lambda)})$, identical to the mean–variance optimum, so adaptivity buys nothing without an information signal. And away from CARA the comparative statics turn counterintuitive: a higher temporary-impact coefficient can *increase* the selling speed, and a larger position can slow liquidation.

## From a schedule to a feedback rule

The common thread of these corrections is the shift from a precomputed schedule to a rule that reacts to the market. Gatheral–Schied and Schied (2013) give closed-form aggressive-in-the-money rules that use the price without knowing its law; Schied–Schöneborn characterise an adaptive rule by a PDE and show when it differs from the deterministic one. Brigo and Piat (2016) show the deterministic optimum reduces to the AC/TWAP schedule when the signal is absent, and that the adapted strategy is at least as good, strictly better when the signal is informative — the practitioner version being Almgren and Lorenz's *Adaptive Arrival Price*, which was not obtained and is developed here from those two papers.

For benchmark-tracked orders the closed loop has a distinctive shape. Frei and Westray (2015) solve the VWAP problem — minimise the mean and variance of slippage against the market VWAP, with the benchmark itself random — using a gamma-bridge model of the intraday relative-volume curve and linear temporary impact. Their optimal rate $\hat u(s) = -\left(2a(s)\hat X(s) + b(s)\gamma(s) + c(s)\right)/(2\kappa)$ decomposes into a deterministic TWAP component plus an adjustment driven by the observed relative-volume curve, and it can run against its own side at most once. Cartea and Jaimungal's closed-form VWAP-targeting strategies were not obtained; the VWAP-as-closed-loop case is treated here from Frei–Westray.

## Key points

- Gatheral–Schied's aggressive-in-the-money rule is volatility-independent, and Schied (2013) proves it optimal for any square-integrable martingale; under a general semimartingale the drift enters in integrated form, so its misspecification averages out.
- Under transient impact this reverses: the problem is ill-posed for a non-absolutely-continuous drift, and the optimal position depends on the drift's *derivative*.
- Concave impact is an empirical regularity, not a theorem; Guéant (2013) shows a volume-dependent microscopic permanent impact can be concave and still arbitrage-free, while linear permanent impact is the only manipulation-free constant-coefficient choice.
- Obizhaeva–Wang show that with endogenous timing and book replenishment $\rho$, the optimum is two discrete lumps plus a constant interior rate, depending only on $\rho$ and $T$, and beats the conventional continuous schedule.
- Predoiu–Shaikhet–Shreve extend that shape to arbitrary books under convexity of $g(y) = y\psi(h^{-1}(y))$; Guéant turns execution cost into an indifference price for a block.
- Mean–variance is time-inconsistent; pre-commitment (Forsyth 2011) and mean–quadratic-variation (Forsyth et al. 2012) repair it, the latter recovering the AC static schedule exactly under arithmetic Brownian motion.
- Closed-loop rules replace the schedule: Schied–Schöneborn tie adaptation to the shape of risk aversion, and Frei–Westray split a VWAP tracker into a TWAP component plus a volume-driven adjustment.

## Sources

- Ryan Donnelly, "Optimal Execution: A Review", *Applied Mathematical Finance* 29(3), 181–212 (2022), Sections 3–4.4 and 6.2. Local PDF: `materials/papers/cluster-c/2022-donnelly-optimal-execution-review.pdf`.
- Alexander Schied, "Robust Strategies for Optimal Order Execution in the Almgren–Chriss Framework", *Applied Mathematical Finance* 20(3), 264–286 (2013), Theorems 1 and 2 and Corollaries 1–2. Local PDF: `materials/papers/cluster-c/2013-schied-robust-strategies-ac.pdf`.
- Christopher Lorenz and Alexander Schied, "Drift Dependence of Optimal Trade Execution Strategies under Transient Price Impact", *Finance and Stochastics* 17(4), 743–770 (2013), Theorem 1 and Corollaries 1–2. Local PDF: `materials/papers/cluster-c/2013-lorenz-schied-drift-dependence.pdf`.
- Anna Obizhaeva and Jiang Wang, "Optimal Trading Strategy and Supply/Demand Dynamics", *Journal of Financial Markets* 16(1), 1–32 (2013), Sections 3, 6–7 and Table 1. Local PDF: `materials/papers/cluster-c/2013-obizhaeva-wang-supply-demand-dynamics.pdf`.
- Silviu Predoiu, Gennady Shaikhet and Steven Shreve, "Optimal Execution in a General One-Sided Limit-Order Book", *SIAM Journal on Financial Mathematics* 2(1), 183–212 (2011), Theorem 3.2 and Theorems 4.2–4.5. Local PDF: `materials/papers/cluster-c/2011-predoiu-shaikhet-shreve-onesided-lob.pdf`.
- Olivier Guéant, "Permanent Market Impact Can Be Nonlinear", arXiv:1305.0413 (2013), Theorem 1 and Propositions 2–3. Local PDF: `materials/papers/cluster-c/2013-gueant-permanent-impact-nonlinear.pdf`.
- Olivier Guéant, "Optimal Execution and Block Trade Pricing: A General Framework", *Applied Mathematical Finance* 22(4), 336–365 (2015), Theorems 2.1–2.3 and Section 3. Local PDF: `materials/papers/cluster-c/2015-gueant-block-trade-general-framework.pdf`.
- Peter Forsyth, "A Hamilton–Jacobi–Bellman Approach to Optimal Trade Execution", *Applied Numerical Mathematics* 61(2), 241–265 (2011), Sections 2 and 8. Local PDF: `materials/papers/cluster-c/2011-forsyth-hjb-optimal-execution.pdf`.
- P. Forsyth, J. Kennedy, S. Tse and H. Windcliff, "Optimal Trade Execution: A Mean–Quadratic-Variation Approach", *Journal of Economic Dynamics and Control* 36(12), 1971–1991 (2012), Section 8 and Proposition 2. Local PDF: `materials/papers/cluster-c/2012-forsyth-mean-quadratic-variation.pdf`.
- Alexander Schied and Torsten Schöneborn, "Risk Aversion and the Dynamics of Optimal Liquidation Strategies in Illiquid Markets", *Finance and Stochastics* 13(2), 181–204 (2009), Theorems 1, 5 and 6 and Corollary 4. Local PDF: `materials/papers/cluster-c/2009-schied-schoeneborn-risk-aversion.pdf`.
- Christoph Frei and Nicholas Westray, "Optimal Execution of a VWAP Order: A Stochastic Control Approach", *Mathematical Finance* 25(3), 612–639 (2015), Theorem 3.1. Local PDF: `materials/papers/cluster-b/2015-frei-optimal-execution-vwap-order.pdf`.
- Damiano Brigo and Clément Piat, "Static vs Adapted Optimal Execution Strategies in Two Benchmark Trading Models", arXiv:1609.05523 (2016), Theorems 2.8–2.10 and Propositions 2.11–2.17. Local PDF: `materials/papers/cluster-c/2016-brigo-piat-static-vs-adapted.pdf`.
- Jean-Philippe Bouchaud, "Price Impact", in *Encyclopedia of Quantitative Finance* (Wiley, 2010), the sections on concave impact. Local PDF: `materials/papers/cluster-b/2010-bouchaud-price-impact.pdf`.
- Robert Almgren, "Optimal Execution with Nonlinear Impact Functions and Trading-Enhanced Risk", *Applied Mathematical Finance* 10(1), 1–18 (2003) — **not obtained**; content reported from Donnelly (2022) §4.4.
- Robert Almgren, "Optimal Trading with Stochastic Liquidity and Volatility", *SIAM Journal on Financial Mathematics* 3(1), 163–181 (2009) — **not obtained**; content reported from Donnelly (2022) §4.1–4.2.
- Aurélien Alfonsi and Alexander Schied, "Optimal Trade Execution and Absence of Price Manipulations in Limit Order Book Models", *SIAM Journal on Financial Mathematics* 1(1), 490–522 (2010) — **not obtained**; its no-manipulation conclusion is reported as stated by Predoiu–Shaikhet–Shreve (2011) §1.
- Robert Almgren and Julian Lorenz, "Adaptive Arrival Price", *Journal of Trading* 2(1) (2007) — **not obtained**; the adaptive-in-the-money concept is developed here from Schied–Schöneborn (2009) and Brigo–Piat (2016).
