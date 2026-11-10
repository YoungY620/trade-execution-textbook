# Measurement, calibration and the engineering loop

Implementation shortfall is defined relative to a price fixed before the outcome was known. That makes execution an engineering discipline: the tape must record the decision price, every child fill and the terminal mark; the cost model's parameters must come from data the desk holds; and the estimate must be refreshed, because it drifts.

## What a fill must record

The four-way split of Chapter 4 is computable only if capture happened at trade time.

| Field | Meaning | Why attribution needs it |
|---|---|---|
| `parent_id`, side, `Q` | The intended trade | Defines the paper portfolio |
| `t_d`, `P_d` | Decision time and the midquote then | Fixes the benchmark before the outcome exists |
| `t_1`, midquote at first release | The **arrival price** | Separates delay from execution cost |
| Child fill: time, price, size, venue, aggressive/passive, fee or rebate | The realised path | Spread, temporary impact and fees |
| `filled_qty`, completion flag, `t_T`, `P_T` | Remainder marked at the terminal midquote | Opportunity cost |
| Fee-schedule version, maker/taker flag | Which schedule applied | Fees are not a constant |
| Benchmark series (interval VWAP/TWAP, the close) | Alternative references | Reporting against the chosen benchmark |

Two fields carry disproportionate weight. The decision price must be timestamped live, because the NBBO can change within milliseconds and Hasbrouck notes an institution "may suspect that the broker is 'gaming' the measure, choosing from a set of nearly simultaneous NBBO records the one that gives the most favorable IS". And the terminal mark must be recorded even when the order fails, or the opportunity term disappears from the sample — exactly the term failed limit orders produce. Kissell's report format is the natural output: order characteristics, then the shortfall split into Fixed, Delay, Trading, Opportunity and Total, then performance against Arrival, Open, Close and VWAP. One 1,864-order sample gives fixed −7.5 bp, delay 13.6 bp, trading −27.4 bp, opportunity −0.2 bp, total −21.6 bp; the total hides which source to attack.

## Benchmarks, and what each one counts

| Benchmark | Reference | What counts as cost | Behaviour induced |
|---|---|---|---|
| Decision / arrival | Midquote at release | Delay, impact, spread, opportunity | Requires a cost forecast |
| VWAP | Interval volume-weighted average | Only deviation from the volume curve | Follow the curve; front-load volume to beat it |
| TWAP | Interval time-weighted average | Only deviation from a constant rate | Trade at a constant rate |
| Close | The closing price | Everything before the close, and how late the start was | Volume toward the close |
| POV | No price reference | The rate is the only decision variable | Horizon follows from the rate |

VWAP strips the market trend from the measurement — Mittal's "moving target, and hence a more forgiving benchmark than arrival prices", needing no cost forecast — and for the same reason lets a trader beat it by front-loading volume while raising true impact. The close does the opposite: Labadie and Lehalle show the target-close schedule weights executed shares by *forward* volatility, so the schedule stiffens and volume moves toward the close. POV fixes no price, and Guéant's closed form makes the participation rate $\rho^\ast$ the only decision, the horizon following from $\int_0^T\rho V_s\,ds=q_0$.

Benchmark choice decides who bears the drift. In Hasbrouck's example a fund pays USD 10.20 on a three-day order whose decision midpoint was USD 10.00 while VWAP was USD 10.15, so the broker attributes USD 0.15 to the trend and leaves USD 0.05 unexplained; his reading is that shortfall against VWAP "is easily measured, interpreted, and assigned to a particular agent (the broker)". Treating that assignment as a principal–agent problem is **this chapter's judgement**; the mechanism rests on that argument and on Mittal's contrast between VWAP, needing no cost forecast, and arrival-price execution, needing one.

## Calibrating the impact law

Individual-trade impact is sublinear,

$$R(\Delta t, v) \propto v^{\psi(\Delta t)}, \qquad \psi(\Delta t) \le 1,$$

with $\psi \simeq 0.1$ to $0.3$ for single trades, rising toward $1$ over thousands of trades. A scheduler needs the **metaorder** — "the bundle of orders corresponding to a single trading decision" — whose average relative price change from first to last child order obeys the square-root law

$$\Delta(Q) = Y\,\sigma\sqrt{\frac{Q}{V}},$$

with $\sigma$ daily volatility, $V$ daily volume measured contemporaneously, and $Y$ of order unity. The general power law $\Delta(Q)\propto Q^\delta$ has $\delta$ between 0.4 and 0.7: Almgren et al. find $\approx 0.6$ over 700,000 Citigroup US stock orders; Moro et al. $\approx 0.5$ in Madrid, $\approx 0.7$ in London; CFM $\approx 0.5$ small-tick, $\approx 0.6$ large-tick futures. BARRA writes the same law as $R=A\sigma\sqrt{v/V}$ with $A$ of order unity.

Zarinelli et al. show the square-root form fits over only about two orders of magnitude in size, while

$$I(\pi) = a\log_{10}(1+b\pi)$$

fits over almost five, with $\hat a=0.028\pm0.001$, $\hat b=465\pm33$ against a power-law fit of $\hat Y=0.15$, $\hat\delta=0.47\pm0.01$; the error falls from 6.70 to 2.80. Impact is also not a function of the daily fraction $\pi$ alone: conditioning jointly on participation rate $\eta$ and duration $F$ gives an impact surface,

$$g(\eta,F) = a\log_{10}(1+b\eta)\cdot\log_{10}(1+cF),$$

with $\hat a=0.035\pm0.001$, $\hat b=60\pm3$, $\hat c=61\pm2$ and error 1.44, whereas the double power law leaves structure in the residuals. For a scheduler, Lehalle's per-bin temporary impact is the parameterisation to fit,

$$\eta(v_n) = a\,\psi_n + \kappa\,\sigma_n\sqrt{\delta t}\left(\frac{v_n}{V_n}\right)^{\gamma},$$

with $\psi_n$ the half bid–ask spread.

Four data layers are needed: the **child-order tape**; **order-book snapshots** for depth, since impact is inversely proportional to depth and Cont et al. relate the intraday price change to net order-flow imbalance with an average $R^2$ of about 65%; a **volume series**; and a **volatility series** for $\sigma_D$ and the same-horizon $\sigma_n$. Metaorders are reconstructed, not observed, so Zarinelli's definitions are the ones to use:

$$\eta \coloneqq \frac{Q}{V(t_e)-V(t_s)}, \qquad F \coloneqq v(t_e)-v(t_s), \qquad \pi \coloneqq \frac{Q}{V(t_c)}, \qquad \pi=\eta F,$$

where $[t_s,t_e]$ is the execution window, $V(t)$ cumulative volume, $t_c$ the close and $v(t)=V(t)/V(t_c)$ volume time. Impact is measured as a volatility-normalised signed return,

$$I(v\mid\Omega) \coloneqq \mathbb E\big[\epsilon\,(s(v)-s(v_s))\mid\Omega\big], \qquad s(v) \coloneqq \log S(v)/\sigma_D,$$

with $\Omega=\pi$ for the curve and $\{\eta,F\}$ for the surface; dividing by $\sigma_D$ is what makes stocks and periods comparable in one regression.

The parameters move. Lehalle states the binding constraint: $(a,\kappa,\gamma)$ and the bin width $\delta t$ cannot be chosen independently, because the parameters must match the decay of impact so that most of it stays inside one bin. Estimates also move with tick regime ($\delta\approx0.5$ small ticks against $\approx0.6$ large), with participation rate and duration, with capitalisation — Lillo et al. rescale single-trade impact approximately as $R\approx M^{-0.3}F(M^{0.3}v/\bar v)$ — and with order-flow persistence, the propagator obeying $G(\ell)\propto\ell^{-\beta}$, $\beta=(1-\gamma)/2$, when trade signs decay as $\ell^{-\gamma}$. A parameter file therefore has a vintage and a scope. The forms above are the sources'; this sequence is **engineering practice**:

1. Fix $\delta t$ at the horizon over which impact largely relaxes, and record it with the parameters.
2. Build signed volume $v_n$ and market volume $V_n$ per bin; form $\rho_n=v_n/V_n$.
3. Read the half-spread term $a\psi_n$ from quoted half-spreads rather than fitting it.
4. Fit $\kappa$ and $\gamma$ by regressing the shortfall of filled bins on $\rho_n$ in the form $\eta=a\psi+\kappa\sigma_n\sqrt{\delta t}\,\rho^{\gamma}$; $\gamma$ is the log-log slope, $\kappa$ the level.
5. Fit the permanent component on a long-horizon regression of price change on cumulative signed flow. Guéant's arbitrage-free specification makes the macroscopic impact $-F(q_0)$ with $F(z)=\int_0^z f(|y|)dy$, and for $f(q)=q^{1-\alpha}$ gives $F(z)=z^{2-\alpha}/(2-\alpha)$, generalising the slippage-based estimator so that the fitted permanent part stays consistent with the concave shape.
6. Estimate $\sigma$ at the same $\delta t$, so that $\kappa\sigma_n\sqrt{\delta t}$ is a price.
7. Re-fit on a rolling window and keep the standard errors, which are the cheapest uncertainty input to the pre-trade model.

## Pre-trade cost models

Kissell's I-Star is the standard multi-asset forecast,

$$I^{*} = a_1\cdot\mathrm{Size}^{a_2}\cdot\sigma^{a_3}, \qquad \mathrm{MI} = b_1\,I^{*}\,\mathrm{POV}^{a_4} + (1-b_1)\,I^{*},$$

with Size the percent of ADV as a decimal, $\sigma$ annualised volatility, POV the participation rate, $a_k>0$, $0\le b_1\le1$. The split is the point: $b_1 I^\ast\mathrm{POV}^{a_4}$ is temporary and scales with participation, $(1-b_1)I^\ast$ is permanent. The global equity table runs from US large-cap ($a_1=1507.5$, $a_2=0.38$, $a_3=0.94$, $a_4=1.05$, $b_1=0.97$) to emerging Asia ($a_1=2431.9$, $a_2=0.52$, $b_1=0.84$); at 10% of ADV, 25% volatility and 9% participation the forecast impact is 18.4 bp for US large-cap against 48.5 bp for emerging Asia. Separate coefficients cover other asset classes — $a_1$ falling to 0.22 for futures, 0.19 for bonds and 0.15 for currency at roughly constant $a_2\approx0.38$ — so a multi-asset desk should not use one model per symbol. A widely repeated claim that optimising against a full fee schedule saves more than 60% of cost has no support in the material assembled for this book.

The same algebra prices a block. Guéant's indifference price for $q_0$ shares liquidated over $[0,T]$,

$$P(T,q_0,S_0) = q_0 S_0 - \int_0^{q_0} F(z)\,dz - \psi q_0 - I_0(q^{*}),$$

is mark-to-market minus permanent impact, minus the linear cost $\psi$ per share, minus the optimised risk-and-impact objective. With $L(\rho)=\eta|\rho|^{1+\varphi}$ and no time constraint,

$$P(q,S) = qS - \int_0^{q} F(z)\,dz - \psi q - \left(\frac{\eta}{\varphi}\right)^{\frac{1}{1+\varphi}}\frac{(1+\varphi)^2}{1+3\varphi}\left(\frac{\gamma\sigma^2}{2V}\right)^{\frac{\varphi}{1+\varphi}} q^{\frac{1+3\varphi}{1+\varphi}}.$$

As a quote this separates a permanent term growing roughly linearly in $q$, the proportional term $\psi q$, and a convex, superlinear risk-and-liquidity charge. In Guéant's example the discount required to buy a block rises from 16.5 bp at 10% of daily volume — 12 permanent, 1 linear cost, 3.5 risk — to more than 27 bp at 20%. The participation-rate version gives premia of 45.1 to 99.9 bp for Total, Axa and Danone at $\gamma=3\times10^{-6}$, against 43.0 to 96.8 bp for the classical IS curve, close enough that Guéant finds "only little difference in terms of costs between a POV strategy and an IS strategy" for those stocks.

A point estimate is the mistake. The parameters carry standard errors, the block-trade price already isolates a risk term, and the surface is convex in size; the forecast belongs in a range built from the parameter covariance and the residual spread by size bucket, with the risk component shown separately so the decision-maker sees how much of the width is parameter uncertainty and how much is market risk. Propagating the covariance into an interval is **engineering practice**.

## Post-trade attribution

Hasbrouck's two-term split, $\text{IS} = \text{price impact} + \text{cost of delay}$, is the minimal version; the four-term split of Chapter 4 is the operative one, with fees removed first because they need no model. Where quotes are recorded, effective cost is measured against the midpoint at the order's arrival and realised cost against the midpoint five minutes later, and their difference approximates the order's own impact. One BATS Rule 605 record gives an effective spread of USD 0.0930 and a realised spread of USD 0.0685, an implied impact of USD 0.0122 per share.

Three traps stand between that arithmetic and a conclusion. Costs are **noisy** relative to their mean and not symmetrically so: Hasbrouck notes realised cost is negative for individual trades because "the five-minute delay used to set the benchmark price can contain substantial price variation". Orders are not **assigned** to algorithms at random, and Hasbrouck's example is the cleanest available — if rebalancing orders go to one broker to be worked gradually and momentum orders to another as urgent, "we'd expect broker R to have the lower costs. This is not due to any special ability, but rather to the sort of orders we send". His remedy is the clinical-trial protocol of randomised orders, stocks, quantities, directions and strategies, which is unbiased but expensive, so most firms analyse the orders they happen to generate; the mitigation is to control for what correlates with assignment, as Kissell's breakdown by side, size, capitalisation, volatility and urgency does, and the effect matters since the aggressive bucket shows 57.2 bp of delay against 3.0 bp for the passive one. Finally, order flow is **correlated** and comparisons are **multiple**: other firms trade the same signals, so part of a price move attributed to one order is everyone else's. Hasbrouck describes the resulting "race for order execution, a race that can only have a few top-ranked finishers", and Wagner's 2003 estimate of institutional delay cost, 77 bp, rests on that basis. Comparing many algorithms across many symbols and periods makes the winner partly a maximum of a noisy sample, so the comparison must be fixed before the data are read, use one agreed metric and a common order universe, report intervals, and be adjusted for the number of hypotheses. The correction is **engineering practice**.

## Why backtesting execution algorithms is hard

A backtest is a counterfactual, and five failure modes are structural.

**The counterfactual is interactive.** An order never sent would still have moved the market, so replaying a fixed tape understates the cost of trading more aggressively than the data did. Mitigation: a simulator that responds to order flow, plus honesty about how it responds; Lehalle frames the algorithm as sitting "inside a closed loop of liquidity" and proposes stress-testing it through the derivatives of its payoff with respect to the half-spread, trading frequency and book liquidity.

**The simulator's impact model is not the market's.** A static impact function cannot reproduce the two facts that dominate realised cost: impact is inversely proportional to depth, with an intraday coefficient that tracks it; and the optimal schedule contains discrete lumps at the start and end with a constant interior rate once trade timing is endogenous. A backtest that forbids front-loading misranks strategies that differ in how they handle resilience.

**Self-calibration biases impact downward.** Fitting the law on the desk's own fills uses a sample selected by its own execution decision, and Bouchaud names the selection: large market orders "are only submitted when there is a large prevailing volume at the best quote, a conditioning that mitigates the impact of these large orders". Fit instead on market-wide reconstructed metaorders, or on randomised subsets of one's own flow.

**Unfilled orders are missing.** A sample built from fills excludes limit orders that never executed, exactly those with the largest opportunity cost; Hasbrouck notes a pegged limit order has "some nonzero failure probability, which must be considered in the implementation shortfall analysis". Record the terminal mark for cancelled orders too.

**Model form is selected on the evaluation sample.** Choosing between the square-root and logarithmic forms, or between a curve and a surface, is itself a fit — a form of data snooping. Mitigation: a walk-forward split — fit on one window, score on the next, choose the form once. Impact decay makes this urgent, since on average roughly a third of peak impact relaxes after the order ends, and a backtest scoring only the last child order's price prefers strategies that trade into the peak.

## The loop

1. **Capture.** Every order writes the record of the first section, including decision price, terminal mark and fee schedule.
2. **Attribute.** Compute the shortfall against a benchmark fixed in advance; split it into delay, trading, opportunity and fees.
3. **Audit.** Aggregate by order characteristics, compare with the pre-trade forecast, flag orders outside the forecast interval.
4. **Re-estimate.** Refit the impact parameters on a rolling window at the bin width in use; compare with the previous vintage.
5. **Re-optimise.** Feed the new parameters into the scheduler, order placement and the router's allocation weights, and re-derive the block-trade premium quoted to clients.
6. **Deploy and monitor.** Return to step 1.

Because $(a,\kappa,\gamma)$ are valid only at the $\delta t$ they were fitted on, the window is pinned by the bin width, and because the estimates move with regime and participation it must be short enough to track regimes and long enough to keep standard errors small. A quarterly refit with a monthly check is a defensible default, and a **judgement**. Off-cycle triggers are more objective: a parameter leaving its confidence interval; a fee-schedule or tick-size change, which moves $\psi$; a volatility regime change, moving every term through $\sigma$; residual bias by size bucket, which points at model form; and a change in the desk's order mix, which invalidates the fitting sample.

## Engineering and operations

Nothing above is computable without an order-management layer recording state transitions. The machine is small — a parent with children moving through new, acknowledged, partially filled, filled, replaced, cancelled and rejected — but every transition needs a timestamp, because delay is a difference of times and opportunity needs the moment the order stopped working. Partial fills are normal: the remainder is marked at the terminal midquote, and a repricing child is a new transition, not an edit of the previous one. Latency enters through the same bookkeeping, since benchmark quotes move within milliseconds and an institution that cannot observe the NBBO depends on broker reports for the decision price — the reason the timestamp is fixed at capture. Routing adds a cost that is easy to drop: a market centre charges to route out, on the order of USD 0.0030 per share in Hasbrouck's schedule, comparable to a maker rebate, and it appears in the shortfall only if fees are captured per child fill.

The monitoring set is largely standardised, since Rule 605 requires every US market centre to publish monthly, by stock and trade size, order counts, share counts, execution rates, cancellation rates, effective and realised spreads. The desk-side counterparts are the fill rate against the parent quantity, the realised spread, the participation rate against its limit, the reject rate, and an alert when the realised decomposition leaves the forecast interval of step 3. The thresholds are **engineering common sense**; that the alert watches the *decomposition* rather than the total follows from the attribution section, since a total within tolerance can hide a delay term and a trading term that cancel.

## Crypto and non-equity adaptations

**The fee schedule is part of the cost model, not a constant.** Conventional maker/taker pricing charges takers and pays a liquidity rebate to makers, and the schedules are neither uniform nor static. NASDAQ's terms were USD 0.0029 per share to a visible maker, USD 0.0015 to a hidden one and USD 0.0030 from a taker, with USD 0.0001 retained; BATS-X paid a rebate rising from USD 0.0025 to USD 0.0029 for members at 1% of consolidated ADV; BATS-Y ran inverted, charging USD 0.0030 for providing liquidity and paying USD 0.0020 for removing it. The access fee, the per-share cap a US venue may charge for removing liquidity, was USD 0.003 and falls to USD 0.001 under the 2024 Reg NMS amendments. A maker/taker spread of a fraction of a cent changes which posted offer is genuinely cheapest, so the schedule must be carried per child fill and the effective benchmark fee-adjusted before any cross-venue comparison. Crypto venues tier the same structure by realised volume: Binance's spot schedule runs from 0.1000% maker and taker for a regular user to 0.0110% maker and 0.0230% taker at the top tier, with a 25% discount for BNB payment, and negotiated zero-fee market-maker contracts appear in the perpetual-futures literature. On MtGox, with 0.6% fees per transaction, spreads and fees of roughly 60 bp frequently exceeded the peak impact of a typical metaorder, so impact was not the dominant term at all.

**A 24/7 market breaks the interval and close benchmarks.** With no close, benchmarks referencing "the close" or "the next open" do not exist as Hasbrouck's taxonomy assumes; the substitutes are point-in-time references — the midpoint at the decision instant and a fixed interval later — which keep the effective/realised decomposition intact. Perpetual futures add a term with no equity counterpart: the funding rate is paid on the *remaining* position, so it acts as a carry that tilts the optimal liquidation path away from Almgren–Chriss, and the funding mechanism is what anchors the perpetual to spot, the no-arbitrage price being the risk-neutral expectation of spot at a random time whose intensity is set by the funding strength. A perp benchmark must therefore be funding-adjusted before a schedule is compared against it. DEX legs carry a fixed cost per transaction rather than a proportional one, so gas dominates small trades and pool and protocol fees dominate large ones — the size-dependent venue choice of Chapter 11. Where the answers depend on protocol mechanics, Chapter 11 is the reference; what matters here is that the same three requirements hold: the record carries the fee and the funding, the calibration is fitted on the venue's own tape, and the loop re-runs when the schedule changes.

## Sources

- Bouchaud, J.-P. (2010). *Price Impact.* Encyclopedia of Quantitative Finance. arXiv:0903.2428.
- Tóth, B., Lemperière, Y., Deremble, C., de Lataillade, J., Kockelkoren, J. & Bouchaud, J.-P. (2011). *Anomalous price impact and the critical nature of liquidity in financial markets.* arXiv:1105.1694.
- Zarinelli, E., Treccani, M., Farmer, J. D. & Lillo, F. (2014). *Beyond the square root: Evidence for logarithmic dependence of market impact on size and participation rate.* arXiv:1412.2152.
- Cont, R., Kukanov, A. & Stoikov, S. (2014). *The price impact of order book events.* Journal of Financial Econometrics 12(1).
- Lehalle, C.-A. (2013). *Market Microstructure Knowledge Needed for Controlling an Intra-Day Trading Process.* In *Handbook on Systemic Risk*. arXiv:1302.4592.
- Guéant, O. (2014). *Optimal Execution and Block Trade Pricing: a General Framework.* arXiv:1210.6372.
- Guéant, O. (2013). *Permanent market impact can be nonlinear.* arXiv:1305.0413.
- Guéant, O. (2014). *Execution and Block Trade Pricing with Optimal Constant Rate of Participation.* Journal of Mathematical Finance 4(3). arXiv:1210.7608.
- Labadie, M. & Lehalle, C.-A. (2013). *Optimal Starting Times, Stopping Times and Risk Measures for Algorithmic Trading.* arXiv:1205.3482.
- Mittal, H. / ITG (2005). *Implementation Shortfall — One Objective, Many Algorithms.* ITG white paper.
- Kissell, R. (2015). *Trading Cost Models Across Multiple Asset Classes.* Kissell Research Group white paper.
- Hasbrouck, J. (2025). *Securities Trading: Principles and Procedures* v14c, NYU Stern teaching text (Ch. 16, 17, 19).
- Donier, J. & Bonart, J. (2015). *A million metaorder analysis of market impact on the Bitcoin.* arXiv:1412.4503.
- He, S., Manela, A., Ross, O. & von Wachter, V. (2026). *Fundamentals of Perpetual Futures.* arXiv:2212.06888.
- Binance. *Fees & Transactions Overview, Spot & Margin fee tiers* (accessed 2026-11-10).

## Key points

- Attribution is decided at capture time: without the decision price, the arrival price, the terminal mark on the remainder and the fee schedule per child fill, neither the shortfall nor its four-way split can be computed.
- The benchmark chooses what "cost" means and who bears the drift risk; VWAP invites volume-curve front-loading, the close pushes volume toward the close, POV makes the rate the only decision, and responsibility follows from the choice.
- The supported laws are the concave power law for individual trades and the square-root law $\Delta(Q)=Y\sigma\sqrt{Q/V}$ for metaorders, with a logarithmic form fitting over a wider range and participation rate and duration entering through a surface.
- Impact parameters hold only at the bin width and regime they were fitted on, so a parameter file needs a vintage and a scope; fix $\delta t$ from the decay, read the half-spread, fit $\kappa$ and $\gamma$ on participation rate, and fit the permanent part on long-horizon flow.
- Pre-trade cost belongs in a range: I-Star separates temporary and permanent components by asset class, and Guéant's block-trade price splits the required discount into permanent impact, proportional cost and a convex risk-and-liquidity term.
- Post-trade attribution turns on noise, selection and correlation rather than on the arithmetic, since orders are not assigned to algorithms at random and the delay attributed to one order is partly other participants' correlated trading.
- Backtests of execution are counterfactuals and fail structurally: the interactive counterfactual, a simulator ignoring depth, downward bias from fitting impact on the desk's own fills, the lost opportunity cost of unfilled orders, and model-form selection on the evaluation sample.
- The loop is capture, attribute, audit, re-estimate, re-optimise and monitor; off-cycle triggers are a parameter leaving its interval, a fee or tick change, a volatility regime change, residual bias by size bucket, or a change in the desk's order mix.
