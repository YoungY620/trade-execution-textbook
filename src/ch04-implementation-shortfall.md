# Implementation shortfall and transaction cost analysis

The framework that makes execution quality measurable begins from one observation: the *paper* portfolio a manager decided to hold is almost never achievable at the prices that prevailed when the decision was made. Commissions, spreads and market impact are paid, and part of the intended position is never filled at all. The difference between the paper and the real portfolio is the **implementation shortfall** (IS).

## The paper portfolio and the decision price

Leinweber states the construction plainly. A paper portfolio is "an imaginary holding consisting of all the security positions the investor decides to hold, acquired at the midquote price that prevailed at the time they decided to hold them"; it incurs no commissions, taxes, spreads, impact or opportunity cost. Measuring trading cost is then simply "the difference between real and paper portfolios." Mittal records the same definition in Perold's own words, as "the difference in return between a theoretical portfolio and the implemented portfolio," and stresses that the prices a manager sees while building a portfolio — the **decision prices** — are not the prices that execution delivers.

Two times therefore matter. Let $t_d$ be the decision time and $P_d$ the **decision price** (the midquote at $t_d$); let $t_1$ be the moment the first trade is released. Labadie and Lehalle make the choice explicit: if the slippage between $t_d$ and $t_1$ is counted as cost, the IS benchmark is the decision-time price $S_0$; if it is neglected, the benchmark is $S_1$, the price at the first trade. Practitioner language calls the midquote at that release instant the **arrival price**.

Fix notation. The target is $Q$ shares; the desk executes $q=\sum_i v_i$ shares in lots $v_i$ at prices $P_i$; the remainder $Q-q$ is left unfilled at a terminal price $P_T$, and $F$ denotes explicit fees. For a buy, the paper portfolio would have cost $Q P_d$; the real portfolio costs $\sum_i v_i P_i + P_T(Q-q)$ plus $F$. The shortfall is their difference,

$$
\mathrm{IS} = \sum_i v_i P_i + P_T(Q-q) - Q P_d + F,
$$

with the convention that a positive number is adverse. For a sell every sign reverses. The sell-side capture appears in Almgren and Chriss as $\sum_k n_k\tilde S_k$, and they note that the total $XS_0-\sum_k n_k\tilde S_k$ "is essentially what Perold (1988) calls implementation shortfall."

## The four components

The single difference above splits into four terms, each with its own behavioural meaning.

**Delay cost.** The shares remain exposed between the decision and the first release of the order:

$$
\text{delay} = q\,(P_{t_1}-P_d).
$$

It vanishes only if the desk trades immediately at the decision price.

**Execution (market-impact) cost.** The slippage between the price when execution began and the prices actually paid:

$$
\text{execution} = \sum_i v_i P_i - q\,P_{t_1} = \sum_i v_i\,(P_i-P_{t_1}).
$$

Spread, temporary impact and permanent impact all land here.

**Opportunity cost.** The unexecuted remainder, marked at the terminal price:

$$
\text{opportunity} = (Q-q)(P_T-P_d).
$$

It is zero only when the order fills completely; economically it is a foregone gain on a favourable move. Mittal's ITG whitepaper measures it as the *standard deviation* of the trading cost against the arrival price, because dispersion — not the mean — is what the unfilled portion drives.

**Explicit fees.** Commissions, taxes, exchange and clearing charges, the component that needs no price model.

The four terms sum back to the shortfall. This "expanded IS" split — delay, execution/impact, opportunity, fees — is the form desks use, with the delay term credited by Kissell (2006) to Wagner (1991).

Which term dominates depends on speed. A patient schedule accumulates delay and opportunity cost while the price drifts away from the decision level and the risk of never completing the order grows; an aggressive schedule accumulates impact. This is exactly the trade-off the Almgren–Chriss frontier quantifies, where a risk-aversion parameter balances expected cost against cost variance (Chapter 5).

## A note on provenance

Perold's 1988 article *The Implementation Shortfall: Paper vs. Reality* is behind a paywall and was not obtained for this book. The definition and the decomposition above are therefore **reconstructed from secondary literature that obtained, uses and attributes it**: Mittal/ITG quoting the definition, Leinweber using paper-minus-real portfolios as an operational cost measure, Labadie and Lehalle for the relative-wealth statement, Almgren and Chriss for the sell-side capture, and the standing description of the Kissell/Wagner four-component split. No sample, number or formula is attributed to Perold that a secondary source does not itself report.

## Where the benchmark sits

IS with the arrival price is one choice among several. Each algorithm pins the reference price somewhere different, and that choice — not only the schedule — fixes what it optimises.

- **TWAP** takes the time-weighted average price over the window as its reference, and its target is a uniform trade rate. It is optimal in expectation only under Bertsimas and Lo's (1998) linear permanent-impact random walk, which yields the equal-split strategy $S_t^\ast = S/T$.
- **VWAP** takes the volume-weighted average price as its reference. Berkowitz, Logue and Noser proposed it as the execution-quality benchmark; it requires no cost forecast, and Mittal calls it "a moving target, and hence a more forgiving benchmark than arrival prices."
- **POV** (percentage of volume) fixes no price reference at all: it constrains the trade to a constant fraction of market volume, and the horizon is implied.
- **Arrival / IS** takes the decision-time price as its reference and targets the minimum expected shortfall for a given risk.

The distinction that matters most is *random versus fixed* benchmark. The arrival price is known at the decision instant. VWAP and target close are random by construction, being functions of the price path itself; comparing an execution against them adds the benchmark's own variance to the measured cost. Two consequences follow: comparing to VWAP can be gamed by front-loading the volume distribution, which lifts the realised average price while raising true impact; and minimising dispersion against a random benchmark is not the same as minimising shortfall against the arrival price.

### VWAP and guaranteed VWAP

Guéant and Royer give VWAP execution its rigorous form. With a market volume curve $V_t$, price $S_t$, an execution-cost function $L(v_t/V_t)$ on the participation rate and a permanent impact $F(\cdot)$ on the price, a broker who signs a *guaranteed VWAP* contract must deliver the period VWAP $q_0\,\mathrm{VWAP}_T$ against terminal cash $X_T$. The premium is the indifference price

$$
\sup_{v\in\mathcal{A}_{\det}} \mathbb{E}\!\left[-\exp\!\left(-\gamma\left(X_T - q_0\,\mathrm{VWAP}_T\right)\right)\right], \tag{3.1}
$$

and Lemma 3.1 shows that $X_T - q_0\,\mathrm{VWAP}_T$ is normal with mean

$$
-\int_0^{q_0} F(z)\,dz - \int_0^T V_t L\!\left(\frac{v_t}{V_t}\right)dt + q_0\int_0^T \frac{V_t}{Q_T}\,F(q_0-q_t)\,dt
$$

and variance $\sigma^2 q_0^2\int_0^T\left(\frac{q_t}{q_0}-\left(1-\frac{Q_t}{Q_T}\right)\right)^2 dt$. The exponential objective then collapses to a deterministic variational problem,

$$
\inf_{v}\ \int_0^T V_t L\!\left(\frac{v_t}{V_t}\right)dt - q_0\!\int_0^T \frac{V_t}{Q_T}F(q_0-q_t)\,dt + \frac{\gamma}{2}\sigma^2 q_0^2\!\int_0^T\!\left(\frac{q_t}{q_0}-\left(1-\frac{Q_t}{Q_T}\right)\right)^2 dt. \tag{3.2}
$$

Their central result is that in the **absence of permanent impact** ($f=0$) the optimal curve is

$$
q^\ast(t)=q_0\left(1-\frac{Q_t}{Q_T}\right),
$$

so the trading curve has the same shape as the relative volume curve, and the premium is $\pi(q_0)=Q_T L(q_0/Q_T)$. Two practical readings follow. Tracking VWAP reduces to forecasting the *relative* volume curve, which is stable across days, rather than the total volume, which is not; and the VWAP algorithm is only exactly volume-shaped when permanent impact can be ignored — otherwise the two curves diverge by a quantifiable amount.

### POV

Guéant closes a gap that mattered in practice, since POV strategies had almost no academic treatment. The trader executes $v_t=\rho V_t$ so that the horizon is implied by $\int_0^T \rho V_s\,ds=q_0$, and chooses the single parameter $\rho$. With execution cost $L(\rho)=\eta\rho^{1+\varphi}$ and a flat volume curve $V_t\equiv V$, the mean-variance criterion is

$$
J(\rho)=\eta\rho^{\varphi} q_0 + \frac{\gamma}{6}\sigma^2\frac{q_0^3}{\rho V},
$$

whose first-order condition $\eta\varphi\rho^{\ast\,\varphi-1}=\frac{\gamma}{6}\sigma^2\frac{q_0^2}{\rho^{\ast 2}V}$ yields the closed-form optimal rate

$$
\rho^\ast=\left(\frac{\gamma\sigma^2 q_0^2}{6\,\eta\,\varphi\,V}\right)^{\frac{1}{1+\varphi}}.
$$

The rate rises with risk aversion $\gamma$, volatility $\sigma$ and order size $q_0$, and falls with liquidity $V$; it is not bounded by one, and it is independent of the linear execution-cost constant and of the linear permanent-impact coefficient, because those are paid regardless of how the order is sliced.

### VWAP as a stochastic-control problem

Frei and Westray minimise both the mean and the variance of the slippage of a VWAP order *against the random market VWAP*, with linear temporary impact $\kappa$ and a **gamma-bridge** model of the relative intraday volume curve. For a buy of $Y$ shares the total expenditure is

$$
TE^u = YP(0) - \sigma\!\int_0^T X^u(t)\,dW(t) + \sigma Y W(T) + \kappa\!\int_0^T u^2(t)\,dt. \tag{1}
$$

They obtain an explicit value function $v(t,x,\gamma)=a(t)x^2+b(t)\gamma x+c(t)x+d(t)\gamma^2+f(t)\gamma+g(t)$ and the optimal trading rate

$$
\hat u(s) = -\frac{2a(s)\hat X(s)+b(s)\gamma(s)+c(s)}{2\kappa}. \tag{3}
$$

Corollary 3.4 shows the optimal rate decomposes as $\hat u=\hat u_1+\hat u_2$, where $\hat u_1=1/T$ is exactly a TWAP rate and $\hat u_2$ reacts to the deviation of the gamma bridge from its mean. A VWAP tracker is therefore TWAP plus a volume-driven correction, and the optimal rate can cross to the far side of the market only once before returning.

## Target close and implementation shortfall are mirror images

Labadie and Lehalle place target close (TC) and IS in one framework and show they are "two sides of the same coin." TC has a fixed end (the close) and an unknown start, and benchmarks on the closing price; IS has a fixed start and an unknown stop, and benchmarks on the price when execution begins. Writing the IS **relative wealth**, with benchmark $S_1$,

$$
\widetilde W = W - S_1\sum_{n=1}^{N} v_n,
$$

they obtain mean $\mathbb{E}(\widetilde W)=\sum_{n=1}^{N}\kappa\sigma_n\frac{(x_n-x_{n+1})^{\gamma+1}}{V_n^\gamma}$ and variance $\mathbb{V}(\widetilde W)=\sum_{n=2}^{N}x_n^2\sigma_n^2$, with impact exponent $\gamma$ and market volume $V_n$. Minimising $\mathbb{E}(\widetilde W)+\lambda\mathbb{V}(\widetilde W)$ yields the explicit recursive trading curve

$$
v_{n-1} = V_{n-1}\left[\frac{\sigma_n}{\sigma_{n-1}}\left(\frac{v_n}{V_n}\right)^{\gamma} + \frac{2\lambda(n,1)\,\sigma_n^2}{\kappa(\gamma+1)\,\sigma_{n-1}}\sum_{i=n}^{N} v_i\right]^{1/\gamma}. \tag{14}
$$

The TC recursion is the mirror image: with constant volatility the two are identical under time reversal. With *time-varying* volatility they differ in a way that matters to a desk. A TC trader must weight already-executed shares by the *forward* volatility $\sigma_{n+1}$, because the close is fixed and the daily slice grows toward the end, so the schedule is stiff and must anticipate volatility one step ahead. An IS trader weights by the spot volatility $\sigma_n$ and keeps more room to adjust near the end. Choosing the close as the benchmark is therefore not a cosmetic choice: it changes the shape of the optimal schedule and its sensitivity to volatility, and it pushes volume toward the close.

## What the trading data show

Leinweber's study is the canonical empirical record. It examines 13,651 equity *purchase* transactions, totalling nearly USD 2 billion, made by one of the largest U.S. corporate pension plans in 1991, with all orders filled, so opportunity cost is excluded and cost reduces to "the trade price less the decision price plus commissions." The findings confound the conventional wisdom. The largest trades cost far less than the Loeb block-cost model predicts; the highest and the lowest percentage costs sit on the *smallest* trades; and many trades show negative cost, that is, transaction profits. Smaller trades are responsible for a disproportionate share of aggregate cost. For one manager's portfolio, net trading cost came to 35 basis points for the year. The headline example is stark: from 1979 to 1991 the ValueLine paper portfolio returned 26.2% annualised while the real ValueLine fund returned 16.1%, the gap being the cost of implementation.

Two obtained sources record a different version of Perold's Value Line comparison. Bertsimas and Lo recount a paper portfolio on Value Line names outperforming the market by almost 20% per year over 1965–1986 while the real fund managed only 2.5% per year. The periods and magnitudes differ from Leinweber's 1979–1991 figures, so the two do not agree on the numbers; we report both and rely on neither for a precise claim.

Mittal's ITG whitepaper supplies the practitioner's synthesis. IS is the right benchmark because VWAP can be influenced, can misdirect risk aversion away from the arrival-price objective, and is more forgiving than the decision price. Yet VWAP and POV remain useful tools *for reducing IS*. The illustrative arithmetic: a 100,000-share order with hourly volume 300k/400k/300k should trade 30k/40k/30k to hold a constant 10% participation, and "this is exactly how a VWAP algorithm will trade"; a POV algorithm generalises the constant rate, so a trader willing to accept 10% participation can simply run a 10% participation algorithm. The charts in the paper show VWAP and IS algorithms performing similarly in *mean* P&L against the arrival price, but IS dominating in *dispersion*, most clearly for orders that are a small fraction of average daily volume — the empirical shape of the opportunity-cost/impact trade-off.

## Transaction cost analysis as a control loop

The measurement above closes a loop. *Pre-trade*, expected cost is forecast from order size, average daily volume, volatility, spread and intended participation, and that forecast chooses the schedule and the algorithm. *Post-trade*, realised shortfall is attributed back into delay, execution, opportunity and fees, against a decision price fixed in advance so that the attribution means something. *Benchmark choice* is itself a decision with consequences, since adopting a random benchmark changes both what is measured and which strategy is optimal. *Statistical significance* is the standing difficulty: per-order costs are extremely noisy relative to their mean, so a credible comparison needs many orders and controls for size, volatility and liquidity.

What the attribution produces feeds back into the scheduler and the order-placement logic: which slices were expensive, whether the cost came from waiting or from impact, and whether the participation rate was too timid or too aggressive. That loop — measurement, calibration and the engineering of the execution stack — is the subject of Chapter 13.

## Sources

- Leinweber, D. J. (2002). *Using Information from Trading in Trading and Portfolio Management: Ten Years Later.* Caltech Social Science Working Paper 1135. (Caltech HSS working-paper archive.)
- Mittal, H. / ITG (2005). *Implementation Shortfall — One Objective, Many Algorithms.* ITG Inc. white paper.
- Labadie, M. & Lehalle, C.-A. (2013). *Optimal Starting Times, Stopping Times and Risk Measures for Algorithmic Trading: Target Close and Implementation Shortfall.* arXiv:1205.3482.
- Guéant, O. & Royer, G. (2014). *VWAP Execution and Guaranteed VWAP.* SIAM Journal on Financial Mathematics 5(1), 445–474. arXiv:1306.2832.
- Guéant, O. (2014). *Execution and Block Trade Pricing with Optimal Constant Rate of Participation.* Journal of Mathematical Finance 4(3), 255–264. arXiv:1210.7608.
- Frei, C. & Westray, N. (2015). *Optimal Execution of a VWAP Order: a Stochastic Control Approach.* Mathematical Finance 25(3), 612–639.
- Almgren, R. & Chriss, N. (2000/2001). *Optimal Execution of Portfolio Transactions.* Journal of Risk 3(2), 5–39.
- Bertsimas, D. & Lo, A. W. (1998). *Optimal Control of Execution Costs.* Journal of Financial Markets 1(1), 1–50.
- Lehalle, C.-A. (2013). *Market Microstructure Knowledge Needed for Controlling an Intra-Day Trading Process.* In *Handbook on Systemic Risk*, Cambridge University Press. arXiv:1302.4592.
- Perold, A. F. (1988). *The Implementation Shortfall: Paper vs. Reality.* Journal of Portfolio Management 14(3), 4–9. **Not obtained; cited only through the secondary sources above.**

## Key points

- Implementation shortfall is the difference between a paper portfolio valued at the decision price and the real portfolio actually achieved; it is the quantity that converts "good execution" into a measurable cost.
- The shortfall decomposes into delay cost, execution/market-impact cost, opportunity cost on the unfilled remainder, and explicit fees, which sum back to the paper-minus-real difference; which term dominates depends on speed, since slow schedules pay delay and opportunity cost while fast schedules pay impact, and the Almgren–Chriss frontier is that trade-off made quantitative.
- Perold's 1988 original was not obtained; the definition and decomposition in this chapter are reconstructed from secondary sources that obtained and used them, and are labelled as such.
- Benchmarks differ in where they pin the reference price — TWAP to the time-weighted average, VWAP to the volume-weighted average, POV to no price at all, IS/arrival to the decision price — and because VWAP and target close are random while the arrival price is known at decision, a random benchmark adds its own variance to the measured cost and changes which strategy is optimal.
- Guéant–Royer show that without permanent impact the optimal VWAP curve is exactly the relative volume curve; Guéant gives a closed-form optimal POV rate; Frei–Westray show a VWAP tracker is TWAP plus a volume-driven correction.
- Labadie–Lehalle show target close and IS share one recursion run in opposite directions, so choosing the close as benchmark reshapes the optimal schedule and its volatility sensitivity.
- Leinweber's trade data show that implementation shortfall is large, that the smallest trades dominate aggregate cost, and that cost is not monotone in trade size.
- Transaction cost analysis closes a loop — pre-trade forecasting, post-trade attribution, benchmark choice and statistical significance — that feeds back into scheduling and order placement (Chapter 13).
