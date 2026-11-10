# The Almgren–Chriss framework

Implementation shortfall, as Chapter 4 measures it, is a random quantity: the cost of a program depends on where prices happen to go while the order is worked. A trader therefore faces two things at once — the transaction cost of trading, and the price risk of not having traded yet. Almgren and Chriss (2000) turned that observation into the canonical model of execution: choose the whole schedule to minimise expected cost plus a penalty on its variance. This chapter sets out their model, their closed-form optimal trajectory and the efficient frontier of schedules, and states the risk-neutral predecessor of Bertsimas and Lo (1998) that the variance penalty generalises.

## The liquidation problem

A block of $X$ units is to be completely liquidated before a fixed horizon $T$. Time is divided into $N$ intervals of length $\tau = T/N$, with grid times $t_k = k\tau$ for $k = 0,\dots,N$. A trading trajectory is the list $x_0,\dots,x_N$, where $x_k$ is the number of units still held at $t_k$; it starts at $x_0 = X$ and ends at $x_N = 0$. Equivalently the trajectory is specified by the trade list $n_1,\dots,n_N$, where

$$n_k = x_{k-1} - x_k$$

is the number of units sold between $t_{k-1}$ and $t_k$; the two are related by $x_k = X - \sum_{j=1}^{k} n_j = \sum_{j=k+1}^{N} n_j$. A *static* strategy fixes the whole list in advance, from information available at $t_0$; a *dynamic* strategy may condition $n_k$ on everything observed up to $t_{k-1}$. The equal interval lengths are only a notational convenience; the continuous-time limit $N\to\infty$, $\tau\to0$ is immediate in every result below.

## The price and impact model

The security price carries an exogenous volatility and an endogenous impact:

$$S_k = S_{k-1} + \sigma\tau^{1/2}\xi_k - \tau\,g\!\left(\frac{n_k}{\tau}\right), \qquad k=1,\dots,N. \tag{1}$$

Here $\sigma$ is the volatility, the $\xi_k$ are independent draws with zero mean and unit variance, and $g(\cdot)$ is the **permanent impact**: a function of the average trading rate $v=n_k/\tau$ over the interval. Permanent impact moves the price that later trades see. There is no drift in (1) — the model assumes no information about the direction of future price moves. A second, **temporary impact** $h(\cdot)$ is charged on the units sold in the interval but does not feed into the next market price:

$$\tilde S_k = S_{k-1} - h\!\left(\frac{n_k}{\tau}\right). \tag{2}$$

Temporary impact is the concession needed to exhaust the liquidity available at successively worse levels while the order is worked, and is assumed to dissipate so that the next interval starts from the equilibrium price again.

Summing trade size times execution price gives the capture of a trajectory,

$$\sum_{k=1}^{N} n_k \tilde S_k = X S_0 + \sum_{k=1}^{N}\left(\sigma\tau^{1/2}\xi_k - \tau g\!\left(\frac{n_k}{\tau}\right)\right)x_k - \sum_{k=1}^{N} n_k h\!\left(\frac{n_k}{\tau}\right). \tag{3}$$

The cost of trading is the difference between the initial book value and the capture, $XS_0 - \sum_k n_k\tilde S_k$; this is the standard ex-post transaction-cost measure and is essentially what Perold (1988) calls implementation shortfall. Before trading it is a random variable with mean and variance

$$E(x) = \sum_{k=1}^{N} \tau x_k\, g\!\left(\frac{n_k}{\tau}\right) + \sum_{k=1}^{N} n_k\, h\!\left(\frac{n_k}{\tau}\right), \qquad V(x) = \sigma^2 \sum_{k=1}^{N} \tau x_k^2. \tag{4,5}$$

$E$ is in dollars, $V$ in dollars squared. The first sum of $E$ is the permanent-impact loss on the position still held, the second the temporary-impact cost on what is traded; $V$ is the variance of the unhedged position while it is held. The cost is exactly Gaussian when the $\xi_k$ are, and nearly Gaussian for large $N$ in any case.

## The linear-impact special case

Closed forms need $g$ and $h$ linear in the rate:

$$g(v) = \gamma v, \qquad h\!\left(\frac{n_k}{\tau}\right) = \epsilon\,\mathrm{sgn}(n_k) + \frac{\eta}{\tau}n_k. \tag{6,7}$$

$\gamma$ is the **permanent-impact coefficient**, in (USD/share)/share: selling $n$ units depresses the price by $\gamma n$ per share whatever the pace. $\epsilon$ is the fixed part of the temporary cost, in USD/share, reasonably estimated as half the bid–ask spread plus fees. $\eta$ is the **temporary-impact coefficient**, in (USD/share)/(share/time); it is much harder to estimate because it depends on transient microstructure, and it is exactly where the linear approximation is most doubtful. Substituting (6,7) into (4),

$$E(x) = \tfrac12\gamma X^2 + \epsilon\sum_{k=1}^{N}|n_k| + \frac{\tilde\eta}{\tau}\sum_{k=1}^{N} n_k^2, \qquad \tilde\eta = \eta - \tfrac12\gamma\tau. \tag{8}$$

The tilded $\tilde\eta$ absorbs the part of the permanent impact that the discretisation mistimes; it differs from $\eta$ by an $O(\tau)$ correction and $\tilde\eta\to\eta$ as $\tau\to0$. The form is convex exactly when $\tilde\eta>0$, and $\epsilon\sum|n_k| = \epsilon|X|$ when all the $n_k$ share a sign, as in a pure sell or buy program. Because $E$ separates into a constant, a term linear in the amount traded, and a penalty on the square of each slice, the model is sometimes called a quadratic cost model.

Two extreme trajectories bound what is achievable. Selling at a constant rate,

$$n_k = \frac{X}{N}, \qquad x_k = (N-k)\frac{X}{N}, \tag{9}$$

gives

$$E = \tfrac12\gamma X^2 + \epsilon X + \left(\eta - \tfrac12\gamma\tau\right)\frac{X^2}{T}, \qquad V = \tfrac13\sigma^2 X^2 T\left(1-\frac{1}{N}\right)\left(1-\frac{1}{2N}\right), \tag{10,11}$$

the *minimum-impact* trajectory, which minimises expected cost but whose variance grows with the horizon. The other extreme, selling everything in the first interval ($n_1 = X$, $x_1=\dots=x_N=0$), gives

$$E = \epsilon X + \eta\frac{X^2}{\tau}, \qquad V = 0. \tag{12,13}$$

Variance is zero because the position is held for an instant, and the temporary cost on the whole block is as large as it can be. The interesting trajectories lie between these two.

## The objective: expected cost plus a variance penalty

A strategy is **efficient** if no other strategy has lower variance for the same or lower expected cost — equivalently, no strategy with lower expected cost for the same or lower variance. The frontier of efficient strategies is generated by

$$\min_{x:\,V(x)\le V_*} E(x), \tag{14}$$

which, since $V(x)$ is convex and $E(x)$ strictly convex, has a unique solution $x^*(V_*)$ for every variance budget $V_*$. Solving it through a Lagrange multiplier $\lambda$ means solving

$$\min_x \left(E(x) + \lambda V(x)\right). \tag{15}$$

For $\lambda>0$ the objective is strictly convex with a unique minimiser $x^*(\lambda)$, and as $\lambda$ ranges over $[0,\infty)$ these minimisers sweep out the frontier. The multiplier has a direct meaning: $\lambda$ measures how heavily variance is penalised relative to expected cost. This is the model's central move. Bertsimas and Lo minimise $E$ alone and cannot distinguish two schedules with the same expected cost but different risk; adding $\lambda V$ prices that risk.

## The optimal trajectory

With (8) and (5), the objective $U = E + \lambda V$ is a strictly convex quadratic in the free holdings $x_1,\dots,x_{N-1}$, so setting its derivatives to zero is exact. That yields, for $j = 1,\dots,N-1$, a linear difference equation,

$$\frac{1}{\tau^2}\left(x_{j-1} - 2x_j + x_{j+1}\right) = \tilde\kappa^2 x_j, \qquad \tilde\kappa^2 = \frac{\lambda\sigma^2}{\tilde\eta} = \frac{\lambda\sigma^2}{\eta\left(1 - \dfrac{\gamma\tau}{2\eta}\right)}. \tag{16}$$

Its solutions are combinations of $\exp(\pm\kappa t_j)$, where $\kappa$ satisfies $\tfrac{2}{\tau^2}\left(\cosh(\kappa\tau)-1\right) = \tilde\kappa^2$. Applying $x_0 = X$ and $x_N = 0$ singles out the **hyperbolic-sine trajectory**

$$x_j = \frac{\sinh\!\left(\kappa(T-t_j)\right)}{\sinh(\kappa T)}\,X, \qquad j = 0,\dots,N, \tag{17}$$

with the corresponding trade list

$$n_j = \frac{2\sinh\!\left(\tfrac12\kappa\tau\right)}{\sinh(\kappa T)}\cosh\!\left(\kappa\left(T - t_{j-\frac12}\right)\right)X, \qquad j = 1,\dots,N, \tag{18}$$

where $t_{j-\frac12} = \left(j-\tfrac12\right)\tau$. The tildes in (16) are again $O(\tau)$ corrections, so for short intervals

$$\kappa \sim \sqrt{\frac{\lambda\sigma^2}{\eta}} + O(\tau), \qquad \tau\to 0. \tag{19}$$

The rate of trading is $v(t) = -\dot x(t)$, proportional to $\cosh(\kappa(T-t))$: the program starts fastest and slows as the horizon approaches. For $X>0$ every $n_j$ is positive, so the optimal way to sell a block never involves buying any of it back — a property that fails as soon as drift or serial correlation is admitted. The closed forms for the optimal expected cost and variance are

$$E(X) = \tfrac12\gamma X^2 + \epsilon X + \tilde\eta X^2 \frac{\tanh\!\left(\tfrac12\kappa\tau\right)\left(\tau\sinh(2\kappa T) + 2T\sinh(\kappa\tau)\right)}{2\tau^2\sinh^2(\kappa T)}, \tag{20}$$

$$V(X) = \tfrac12\sigma^2 X^2 \frac{\tau\sinh(\kappa T)\cosh\!\left(\kappa(T-\tau)\right) - T\sinh(\kappa\tau)}{\sinh^2(\kappa T)\sinh(\kappa\tau)},$$

and in the limits $\kappa\to0$ and $\kappa\to\infty$ they reduce to (10–13).

## The half-life and comparative statics

Almgren and Chriss call

$$\theta = \frac{1}{\kappa}$$

the **half-life** of the trade: the time in which the position decays by a factor $e$. Two features matter. First, $\theta$ does not depend on the imposed horizon $T$; it is set by the price dynamics and the impact coefficients alone. So long as the trader is risk-averse ($\lambda>0$) the half-life is finite, and with no external deadline ($T\to\infty$) the position is still liquidated on the scale $\theta$. It is the intrinsic time of the trade.

Second, the ratio $\kappa T = T/\theta$ tells us which force dominates. When $T \gg \theta$ — temporary costs small, volatility or risk aversion large — the half-life is short relative to the deadline, the bulk of trading is done well before $T$, and on the scale of $T$ the trajectory resembles the minimum-variance spike (12). When $T \ll \theta$ the trade is constrained by its deadline and dominated by temporary impact; as $T/\theta\to0$ it approaches the straight-line minimum-cost strategy (9). Reading (19) gives the comparative statics directly: a larger risk aversion $\lambda$ or volatility $\sigma$ raises $\kappa$ and front-loads the schedule, while a larger temporary-impact coefficient $\eta$ lowers $\kappa$ and stretches it out.

One consequence is easy to miss. Because both the variance and the temporary impact scale quadratically with block size under linear impact, baskets of the same security are liquidated in exactly the same way, on the same time scale, whatever their size — contrary to the intuition that large baskets are less liquid and should be worked more slowly. Almgren and Chriss treat this as a defect of the linear-impact assumption and suggest a size-dependent $\eta$ in practice.

## The efficient frontier

Plotted as variance against expected cost, the frontier is a smooth convex curve, differentiable at its minimum. That minimum is the **naive strategy** — the equal-split trajectory (9) that minimises expected cost without regard to variance, which Bertsimas and Lo show is optimal when risk is ignored. Differentiability at the minimum is the practical punchline: moving slightly off the naive strategy costs only a second-order increase in expected cost while buying a first-order reduction in variance, so a trader who is at all risk-averse should never trade the naive strategy. The curve has a hyperbolic shape, and the paper reads it in two coordinate systems. Figure 1 plots variance against expected loss and marks the frontier strategies for $\lambda = 10^{-6}$ and its neighbours $\lambda = 2\times10^{-6}$ (more urgent, sold faster) and $\lambda = -2\times10^{-7}$ (risk-seeking, postponed and sold faster at the end). Figure 3 replots the same frontier against the standard deviation of cost, $\sqrt{V}$, a plane in which the tangents that select a value-at-risk-optimal strategy have constant slope. The curvature of the frontier at its minimum is a measure of the security's liquidity.

For the value-at-risk reading, define the VaR of a strategy at confidence level $p$ as

$$\mathrm{VaR}_p(x) = \lambda_v\sqrt{V(x)} + E(x), \tag{22}$$

the level of cost exceeded only a fraction $1-p$ of the time, where $\lambda_v$ is the number of standard deviations fixed by the inverse normal CDF — $1.645$ at 95% confidence. Once the one-parameter frontier is in hand, the VaR-minimising strategy is a one-dimensional problem, and the paper calls the result the **liquidity-adjusted Value at Risk** (L-VaR): the smallest VaR attainable by any static liquidation strategy over a given horizon. Note the asymmetry with the mean-variance frontier: because the cost charged to uncertainty scales linearly with portfolio size while temporary impact scales quadratically, the VaR-optimal schedule trades large portfolios closer to the straight line.

## Calibration

Section 3.4 calibrates the model on a worked case, and the units exercise is worth following literally. A stock trades at $S_0 = 50$; the position is $X = 10^6$ shares, worth USD 50M. Annual volatility of 30% over 250 trading days gives a daily volatility $0.3/\sqrt{250} = 0.019$, so $\sigma = 0.95$ in (USD/share)/day$^{1/2}$; annual growth of 10% gives $\alpha = 0.02$ in (USD/share)/day. The bid–ask spread is $1/8$, so the fixed temporary cost is $\epsilon = 0.0625$ per share. Daily volume is 5 million shares. Assuming that trading 1% of daily volume costs one spread,

$$\eta = \frac{1/8}{0.01 \cdot 5\times10^6} = 2.5\times10^{-6}$$

in (USD/share)/(share/day). A rule of thumb that price effects become significant at 10% of daily volume, with "significant" meaning one spread, gives

$$\gamma = \frac{1/8}{0.1 \cdot 5\times10^6} = 2.5\times10^{-7}$$

in USD/share². With risk aversion $\lambda = \lambda_u = 10^{-6}$ per dollar, $\kappa \approx 0.6$ per day, so over a one-week horizon $\kappa T \approx 3$: an intermediate pace, neither the naive line nor the minimum-variance spike. Holding the position for that week untouched would carry a one-standard-deviation value fluctuation of $\sigma\sqrt{T} = 2.12$ per share. So $\sigma$ is read off realised price variation while $\eta$ and $\gamma$ are read off price impact measured against participation in volume — a spread per 1% of volume, and a spread per 10% of volume, here.

## Where the model is weakest

Optimal trajectories are computed by static optimisation, so the schedule can be fixed before trading begins; the justification is that with independent price increments and a symmetric risk penalty the strategy is *time-homogeneous*. Almgren and Chriss prove this as a theorem: a strategy optimal over $[0,T]$ remains optimal over every subinterval, so at any later time the trader continues the original plan rather than re-optimising. This is time *consistency*, not the time-inconsistency that later critiques of mean-variance execution would raise, and it rests on the assumed absence of serial correlation. Relaxing that assumption is the subject of the paper's Section 4: a drift $\alpha$ shifts the target holding to $\bar x = \alpha/(2\lambda\sigma^2)$, about 1,100 shares (0.11% of the position) in the calibrated example; serial correlation lets a trader shift slices between periods, but the paper estimates the gain as small — tens of dollars a day at institutional sizes — and vanishing as a fraction of the program as the portfolio grows; a scheduled news event makes the optimal strategy only piecewise static.

Two modelling limits recur. The linear impact functions (6,7) are what make the exponential solution available; with nonlinear $g$ and $h$ the framework still applies but the trajectory is found by solving a non-quadratic optimisation, and the size-independence above disappears. And the continuous-time limit $\tau\to0$ is "at best a mathematical convenience", since the model is a coarse-grained description of a limit order book rather than of individual orders — the place where the microstructure of Chapter 3 re-enters.

## The predecessor: Bertsimas and Lo

Almgren and Chriss generalise Bertsimas and Lo (1998), who asked for the dynamic strategy minimising the *expected* cost of executing a fixed block $S$ over a fixed number of periods, and solved it by stochastic dynamic programming. Their law of motion is a linear price-impact random walk,

$$P_t = P_{t-1} + h S_t + \varepsilon_t, \qquad h>0, \qquad \mathbb{E}\!\left[\varepsilon_t \mid S_t, P_{t-1}\right] = 0, \tag{2.3}$$

with remaining shares $\nu_t = \nu_{t-1} - S_{t-1}$, $\nu_1 = S$, $\nu_{T+1} = 0$, minimising $\mathbb{E}\left[\sum_t P_t S_t\right]$ subject to $\sum_t S_t = S$ through the Bellman equation $\mathcal{V}_t(P_{t-1},\nu_t) = \min_{S_t}\mathbb{E}_t\left[P_t S_t + \mathcal{V}_{t+1}(P_t,\nu_{t+1})\right]$. The punchline is a negative: under exactly this law of motion the unique optimum is the naive equal-split strategy $S_t^* = S/T$, with optimal expected cost $P_0 S + \tfrac{hS^2}{2}\left(1 + \tfrac{1}{T}\right)$. The per-period impact $hS_t$ is permanent, the same trade-off in every period, and independent of the unexecuted order, so marginal execution costs are equalised by setting the slices equal; the cumulative permanent impact does not vanish even as $T\to\infty$, tending to $hS^2/2$. The paper stresses that these are "rather restrictive and empirically implausible" conditions, and that outside them the naive strategy is not optimal by construction. Adding a serially correlated information variable $X_t$ to the price makes the best-execution strategy a linear function of both remaining shares and the signal, $S^*_{T-k} = d_{w,k}\nu_{T-k} + d_{x,k}X_{T-k}$ with $d_{w,k} = 1/(k+1)$; the first term is the naive strategy, the second vanishes when the signal is unforecastable ($\rho=0$). Monte Carlo experiments in the paper find the best-execution strategy beating the naive one by roughly 25% to 40% in cost for at least one empirically plausible specification.

Two gaps in Bertsimas and Lo are exactly what the Almgren–Chriss objective fills. Their trader is risk-neutral — the paper is explicit that expected cost is the only objective and that higher moments are analytically tractable only in special cases — so the model cannot separate a fast schedule from a slow one when their expected costs are equal; being risk-neutral, in the AC framework it has an infinite half-life and trades every basket at the same rate regardless of liquidity. Adding $\lambda V(x)$ supplies the missing risk preference, and the sinh trajectory and the half-life are what follow.

## Key points

- The Almgren–Chriss problem liquidates $X$ units over $[0,T]$ by choosing the holdings trajectory $x_k$ to minimise $E(x)+\lambda V(x)$; the cost is Perold's implementation shortfall, with mean and variance given by equations (4) and (5).
- Under linear permanent impact $\gamma$ and temporary impact $\eta$, the mean cost is $\tfrac12\gamma X^2 + \epsilon\sum|n_k| + (\tilde\eta/\tau)\sum n_k^2$ with $\tilde\eta = \eta - \tfrac12\gamma\tau$, so large slices are penalised quadratically.
- The optimal trajectory solves the difference equation (16) and is the hyperbolic-sine path $x_j = X\sinh(\kappa(T-t_j))/\sinh(\kappa T)$, with $\kappa = \sqrt{\lambda\sigma^2/\eta}$ in the continuous-time limit.
- The trade's half-life $\theta = 1/\kappa$ is independent of the imposed horizon: it is the intrinsic liquidation time, set only by liquidity, volatility and risk aversion.
- Higher risk aversion or volatility front-loads the schedule and shortens the half-life; a higher temporary-impact coefficient slows it down; $\kappa T$ small approaches linear liquidation and $\kappa T$ large approaches immediate liquidation.
- The efficient frontier is a smooth, convex curve whose minimum is the naive equal-split strategy; because it is flat there, a small increase in expected cost buys a first-order reduction in variance, so a risk-averse trader should never use the naive strategy.
- Bertsimas and Lo's risk-neutral, linear-impact dynamic program makes the naive strategy optimal only under restrictive conditions, and otherwise a linear function of remaining shares and information; treating risk through $\lambda V$ is the addition Almgren and Chriss make.
- The model's assumptions are its limits: static precomputed schedules justified by time homogeneity, linear impact that makes schedule choice size-independent, and a continuous-time limit that is a convenience rather than a description of the order book.

## Sources

- Robert Almgren and Neil Chriss, "Optimal Execution of Portfolio Transactions", *Journal of Risk* 3(2), 5–39 (December 2000 preprint), especially Sections 1–3 and the Conclusions. Local PDF: `materials/papers/cluster-b/2000-almgren-optimal-execution-portfolio-transactions.pdf`.
- Dimitris Bertsimas and Andrew W. Lo, "Optimal Control of Execution Costs", *Journal of Financial Markets* 1(1), 1–50 (1998), Sections 2.3–2.5, 7.3 and the Conclusion. Local PDF: `materials/papers/cluster-b/1998-bertsimas-optimal-control-execution-costs.pdf`.
