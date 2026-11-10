# Glossary

Terms used throughout this book, with the one-line gloss that appears when you hover a highlighted term in the text. Each entry also gives the section where the term is first introduced.

[A](#gloss-a) · [B](#gloss-b) · [C](#gloss-c) · [D](#gloss-d) · [E](#gloss-e) · [F](#gloss-f) · [G](#gloss-g) · [H](#gloss-h) · [I](#gloss-i) · [K](#gloss-k) · [L](#gloss-l) · [M](#gloss-m) · [N](#gloss-n) · [O](#gloss-o) · [P](#gloss-p) · [Q](#gloss-q) · [R](#gloss-r) · [S](#gloss-s) · [T](#gloss-t) · [U](#gloss-u) · [V](#gloss-v) · [Z](#gloss-z)

<a id="gloss-a"></a>

## A

<a id="term-access-fee"></a>

**Access fee** — *接入费*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

The fee for hitting a venue's protected quotations, capped by Rule 610 at USD 0.003 per share so displayed prices stay close to true.

The cap is what lets a router compare displayed prices across venues. Where the quotation is below USD 1.00, the fee cannot exceed 0.3% of the quotation price.


<a id="term-actor-critic"></a>

**Actor–critic** — *演员–评论家*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

A policy-gradient design with two parts: an actor that proposes actions and a critic that estimates value and supplies the learning signal.

A continuous-action actor–critic (DDPG) allows execution policies to choose trade sizes continuously and to carry past actions as state under transient impact.


<a id="term-adverse-selection"></a>

**Adverse selection** — *逆向选择*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The risk that whoever trades with you does so because he knows something you do not, forcing your quotes to break even against him.

Glosten-Milgrom (1985) show that adverse selection alone creates a spread even for a risk-neutral, competitive, zero-profit specialist.


<a id="term-aggressive-in-the-money"></a>

**Aggressive in the money** — *随盈利而激进*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

Selling faster when the price moves favourably, i.e. when the position is in the money relative to the arrival price.

Gatheral–Schied's closed-form strategy is aggressive in the money yet independent of volatility. Schied–Schöneborn show a utility maximiser is aggressive in the money exactly when absolute risk aversion is increasing; under CARA the optimal strategy is deterministic.


<a id="term-alpha-signal"></a>

**Alpha signal** — *alpha 信号*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

A short-horizon predictor of the direction of the next price move, driven by order flow or imbalance, used to skew quotes or time trades.

Lehalle–Neuman model it as a Markovian signal and Cartea–Wang as momentum driven by market orders; in both the optimal quote reacts to it, and a large enough signal turns the maker into a taker.


<a id="term-arrival-price"></a>

**Arrival Price** — *到达价格*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

The midquote at the instant an order is released to the desk; the benchmark used by implementation shortfall algorithms.


<a id="gloss-b"></a>

## B

<a id="term-best-execution"></a>

**Best execution** — *最优执行义务*  <span class='bd-gloss-alpha'>First seen: Chapter 12</span>

A broker's regulatory duty to seek the best available terms for a client order, fixing the feasible routing set and the fair benchmark.

Because the arrival price is known at the decision instant while VWAP and the close are functions of the price path, a broker working to a random benchmark and a client holding it to the arrival price are measuring different things.


<a id="term-bid-ask-spread"></a>

**Bid-ask spread** — *买卖价差*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The gap between the best sell price and the best buy price; the toll a trader pays for demanding immediate execution.

Wyart et al. (2008) split it into order processing, adverse selection and inventory-risk components; none of the three is a fee.


<a id="gloss-c"></a>

## C

<a id="term-circuit-breaker"></a>

**circuit breaker** — *熔断机制*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

A market-wide halt triggered by a large index fall, meant to interrupt a collapse and let participants reassess.

US market-wide circuit breakers were first instituted in response to the October 1987 market "break", first triggered on 27 October 1997, and fired four times in March 2020. Crypto venues have no consolidated market, and this chapter judges that they have no direct analogue.


<a id="term-colocation"></a>

**colocation** — *共置*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

Placing a firm's computer in the exchange's data centre, beside the matching engine, to minimise the distance and latency to the market.

Collocation is the modern successor to the floor's informational and access advantage — the new "floor" is a blade server. The SEC's approval of IEX in 2016 introduced a 350-microsecond delay as a counter-measure.


<a id="term-concentrated-liquidity"></a>

**Concentrated liquidity** — *集中流动性*  <span class='bd-gloss-alpha'>First seen: Chapter 11</span>

An AMM design (Uniswap v3) where liquidity providers place liquidity within a chosen price range instead of the full curve.

A narrower range earns more fees while the marginal rate is inside it but stops earning when the rate exits, and predictable loss rises as the range narrows. Uniswap v3 offers fee tiers of 0.01%, 0.05% and 0.1%.


<a id="term-constant-function-market-maker"></a>

**Constant function market maker** — *恒定函数做市商*  <span class='bd-gloss-alpha'>First seen: Chapter 11</span>

A decentralised exchange that prices trades on a curve, accepting a trade only if a trading function over its reserves does not decrease.

A trade Delta is valid iff phi(R - Delta) >= phi(R). The trading function is concave in all practical cases (Uniswap, SushiSwap, Balancer, Curve), which makes optimal routing a convex optimisation problem.


<a id="gloss-d"></a>

## D

<a id="term-dark-pool"></a>

**Dark pool** — *暗池*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

A non-displayed trading venue that matches orders without publishing quotes, usually at the midpoint of the lit market.

Zhu (2014) shows that adding a dark pool can raise the exchange spread yet improve price discovery, because informed orders cluster on the heavy side and face lower fill probability in the dark.


<a id="term-data-snooping"></a>

**Data snooping** — *数据窥探*  <span class='bd-gloss-alpha'>First seen: Chapter 13</span>

Selecting a model or parameter on the same sample used to report its performance, which makes the reported result look better than it is.

Mitigated by a walk-forward split, fitting on one window and scoring on the next, with the functional form chosen once.


<a id="term-decimalisation"></a>

**decimalisation** — *最小报价单位十进制化*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

The change of the US equity pricing increment from one-eighth of a dollar to one cent, legislated in 1997 and completed in 2001.

Tick size is, in Hasbrouck's phrase, the price of time priority — a finer tick makes jumping a long queue at the same price much cheaper.


<a id="term-decision-price"></a>

**Decision Price** — *决策价格*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

The price, usually the midquote, prevailing when a manager decided to hold a position; the benchmark for implementation shortfall.


<a id="term-deep-q-network"></a>

**Deep Q-network** — *深度 Q 网络*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

A neural network that approximates the Q-function, so high-dimensional states such as order-book features need no lookup table.

It is trained with experience replay and a target network; because the admissible action set changes with inventory, Ning et al. feed the state and action to the network and read out $Q(s,x)$.


<a id="term-delay-cost"></a>

**Delay Cost** — *延迟成本*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

The cost of price exposure between the decision instant and the release of the first trade.

Hasbrouck splits implementation shortfall as price impact plus cost of delay, and reports Wagner's (2003) estimate of institutional delay cost at 77 basis points.


<a id="term-double-deep-q-learning"></a>

**Double deep Q-learning** — *双重深度 Q 学习*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

A deep Q-network variant that separates action selection from value estimation across two networks, reducing overestimation of Q-values.

It is the most common value-based method in the execution literature, used by Ning et al. and by Karpe et al. in a multi-agent limit-order-book simulation.


<a id="gloss-e"></a>

## E

<a id="term-ecn"></a>

**ECN** — *电子通信网络*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

A non-exchange electronic venue, such as Instinet or Island, that matched orders without taking on the regulation of an exchange.

Regulated as alternative trading systems (ATSs) after the SEC adopted Regulation ATS in 1997. By October 2009 the five active ECNs executed about 10.8% of US share volume.


<a id="term-effective-rebate"></a>

**Effective rebate** — *有效返佣*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

The exchange's limit-order rebate less a venue-specific penalty for adverse selection, the net figure that a passive fill actually earns.

Written $r_k=r_k^{e}+AS_k$ in Cont and Kukanov (2017), where $AS_k$ is typically estimated as average returns over a short interval after a limit-order execution.


<a id="term-effective-spread"></a>

**Effective spread** — *有效价差*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

Twice the gap between the execution price and the quote midpoint at the trade; the realized cost of an immediately executed order.

Hasbrouck's decomposition writes effective cost as p minus m and realized cost as p minus the five-minute midpoint m5; price improvement is the amount by which p beats the near-side quote.


<a id="term-efficient-frontier"></a>

**Efficient frontier** — *有效前沿*  <span class='bd-gloss-alpha'>First seen: Chapter 5</span>

The family of schedules with minimum expected cost at each level of cost variance, traced out as the risk-aversion parameter varies.

For execution it is a smooth convex curve whose minimum is the naive strategy; its curvature there measures the security's liquidity. Solving min E(x)+lambda*V(x) for a range of lambda reproduces the same family.


<a id="gloss-f"></a>

## F

<a id="term-fill-intensity"></a>

**Fill intensity** — *成交强度*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

The Poisson rate at which a resting quote is executed, a decreasing function of the quote's distance from the mid-price.

Avellaneda–Stoikov assume the exponential form A*exp(-k*delta), where A is the intensity at zero distance and k the price-decay rate; Cont–Stoikov–Talreja derive fill probabilities from the book state instead of assuming them.


<a id="term-funding-rate"></a>

**funding rate** — *资金费率*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

The periodic payment between longs and shorts of a perpetual, a clamped function of the futures-spot premium, which anchors it to spot.

Because funding accrues on the remaining position, it adds an effective carry to the holding cost, tilting the optimal liquidation path away from a pure Almgren-Chriss schedule.


<a id="gloss-g"></a>

## G

<a id="term-gamma-bridge"></a>

**Gamma Bridge** — *伽马桥*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

A process used by Frei and Westray to model the relative intraday volume curve; empirically tractable and independent of total volume.

Frei–Westray (2015) use it to solve the VWAP-targeting problem in closed form; the optimal rate splits into a deterministic TWAP component plus an adjustment driven by the observed volume curve.


<a id="term-gas-fee"></a>

**Gas fee** — *Gas 费*  <span class='bd-gloss-alpha'>First seen: Chapter 11</span>

The fixed on-chain cost of a blockchain transaction, borne per transaction rather than proportional to trade size.

Because it is fixed per transaction, its share of total cost falls as notional grows, which is why DEXs are cheaper only above about USD 10,000 and why routing with a fixed per-pool gas cost becomes a mixed-integer problem.


<a id="term-guaranteed-vwap"></a>

**Guaranteed VWAP** — *保证成交均价*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

A contract in which a broker promises the period VWAP for a client order, charging a premium equal to the indifference price.


<a id="gloss-h"></a>

## H

<a id="term-half-life-of-a-trade"></a>

**Half-life of a trade** — *交易的半衰期*  <span class='bd-gloss-alpha'>First seen: Chapter 5</span>

The time theta=1/kappa in which the position decays by a factor e, set only by liquidity, volatility and risk aversion, not by the horizon.

It is the intrinsic execution time of the trade — even with no deadline a risk-averse trader liquidates on the scale theta. The ratio kappa*T=T/theta says which force dominates small values giving linear liquidation, large values giving immediate liquidation.


<a id="term-hidden-order"></a>

**Hidden order** — *隐藏单*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

A limit order whose size is not displayed at all, so its whole size stays off the visible book, at the cost of time priority.

Hidden orders lose time priority to displayed orders at the same price on most venues. Empirical work shows they are used both to conceal large size and, by high-frequency traders, to undercut standing quotes and time volatility.


<a id="term-hjb-quasi-variational-inequality"></a>

**HJB quasi-variational inequality** — *拟变分不等式*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

The dynamic-programming equation for a mixed regular and impulse control problem, the minimum of a running equation and an impulse term.

It is needed when one control acts continuously and another at isolated stopping times, as with limit orders and market orders; ordinary HJB cannot represent the impulse channel.


<a id="term-hyperbolic-sine-trajectory"></a>

**Hyperbolic-sine trajectory** — *双曲正弦轨迹*  <span class='bd-gloss-alpha'>First seen: Chapter 5</span>

The closed-form optimal holdings path x(t)=X*sinh(kappa(T-t))/sinh(kappa*T) obtained under linear impact.

It solves the difference equation (1/tau^2)(x_{j-1}-2x_j+x_{j+1})=kappa^2*x_j, falls monotonically from X to zero, and never repurchases; kappa=sqrt(lambda*sigma^2/eta) in the continuous-time limit.


<a id="gloss-i"></a>

## I

<a id="term-iceberg-order"></a>

**Iceberg order** — *冰山单*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

A limit order that displays only a small peak size, refreshing a new peak from the hidden part each time a displayed peak executes.

Also called an undisclosed order. Because it must refresh, an iceberg leaves a detectable signature, and its displayed part outranks nothing at the same price on venues that give display priority.


<a id="term-imitation-learning"></a>

**Imitation learning** — *模仿学习*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

Learning a policy from demonstrations by another strategy rather than from a hand-designed reward, avoiding costly exploration.

In execution it is attractive because exploring in a real market costs money and cannot be repeated; FlowOE imitates an ensemble of classical and learned experts.


<a id="term-impact-surface"></a>

**Impact surface** — *冲击面*  <span class='bd-gloss-alpha'>First seen: Chapter 13</span>

Market impact as a function of both participation rate and duration, rather than of order size alone.

Zarinelli et al. fit $g(\eta,F)=a\log_{10}(1+b\eta)\log_{10}(1+cF)$ with $\hat a=0.035$, $\hat b=60$, $\hat c=61$, and find the double power law leaves structure in the residuals while this does not.


<a id="term-implementation-shortfall"></a>

**Implementation Shortfall** — *实施差额*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

The gap between the paper portfolio valued at decision prices and the portfolio actually executed; the standard measure of trading cost.

Coined by Perold (1988), whose original text this book did not obtain; the definition here is reconstructed from secondary sources.


<a id="term-impulse-control"></a>

**Impulse control** — *脉冲控制*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

A control that acts at isolated stopping times, moving the state by a discrete jump rather than continuously.

Sending a market order is an impulse control: it is instantaneous and moves inventory and cash by a discrete amount, at a cost equal to the half-spread plus a fee.


<a id="term-indifference-pricing"></a>

**Indifference pricing** — *无差异定价*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

Pricing a block by the cash amount that leaves the agent indifferent between accepting it and not, given that the block must be liquidated.

Guéant (2015) derives P = q*S minus the permanent-impact integral minus linear fees minus the optimised cost of risk and impact; the last term is the part trading well can reduce, i.e. the liquidity premium.


<a id="term-information-half-life"></a>

**Information half-life** — *信息半衰期*  <span class='bd-gloss-alpha'>First seen: Chapter 12</span>

The time a private valuation signal is expected to persist before the market prices it away; it sets the pace of an informed trade.

Distinct from the Almgren–Chriss trade half-life $\theta=1/\kappa$, which is set by liquidity, volatility and risk aversion. An active signal shortens the effective decay of the position below what a pure risk frame would choose.


<a id="term-instantaneous-impact-function"></a>

**Instantaneous impact function** — *瞬时冲击函数*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

Guéant's L(rho), the cost per unit of volume as a function of the participation rate; strictly convex, with L(0)=0 and super-linear growth.

In practice L(rho)=eta|rho|^(1+phi)+psi|rho|, nesting linear and square-root shapes; permanent impact k is only a change of variables, so the modelling freedom is in L.


<a id="term-intermarket-sweep-order"></a>

**Intermarket sweep order** — *跨市场扫单*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

A limit order marked as a sweep and sent with orders covering all better protected quotations, so it may execute without regard to them.

Defined in Rule 600(b)(30) and permitted by Rule 611(b)(5); it is the standard compliance mechanism for taking liquidity across several venues at once.


<a id="term-internalization"></a>

**Internalization** — *内部化*  <span class='bd-gloss-alpha'>First seen: Chapter 12</span>

Executing a client's marketable order against a broker-dealer's own inventory or flow instead of sending it to a public venue.

In US equities a large share of retail flow is internalized or routed off-exchange; the accompanying price improvement is real and most valuable when the spread is at its one-cent minimum.


<a id="term-inventory-risk"></a>

**Inventory risk** — *库存风险*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

The risk a liquidity provider bears because fills leave it holding a position whose price may move before it can be unwound.

It enters the spread through the maker's risk aversion and volatility, and it is what makes optimal quotes widen and shift against the position.


<a id="term-inventory-skew"></a>

**Inventory skew** — *库存偏移*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

The shift of the quoted mid away from the market mid because of the maker's inventory, equal to the reservation price minus the mid.

With exponential fill intensities the whole spread stays inventory-independent and the skew carries all the inventory information.


<a id="term-inverted-fee-venue"></a>

**Inverted fee venue** — *反向费率场所*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

A venue that reverses maker-taker pricing, charging for providing liquidity and paying a rebate to remove it.

Because the penny grid bars passing the rebate through as a better quoted price, inverted venues compete by creating a finer effective pricing grid inside the spread, which raised their market share from 15% to 28% in the 2016 Tick Size Pilot trade-at group.


<a id="gloss-k"></a>

## K

<a id="term-kyle-s-lambda"></a>

**Kyle's lambda** — *凯尔 λ*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The slope of price on aggregate order flow in Kyle's model; the canonical linear price-impact coefficient, whose reciprocal is market depth.

Half of (Sigma_0/sigma_u^2)^(1/2) in the single auction and the full value in the continuous limit; it is the equilibrium microfoundation of permanent impact.


<a id="gloss-l"></a>

## L

<a id="term-latency-arbitrage"></a>

**Latency arbitrage** — *延迟套利*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

Profiting from the tiny interval in which one venue's quote is stale and another's has moved, by being faster than everyone else.

Aquilina, Budish and O'Neill (2022) measure it in UK equities as about 0.42 to 0.53 basis points of trading volume, roughly a third of the effective spread, and estimate that eliminating it would cut the market's cost of liquidity by 17%.


<a id="term-latency-cost"></a>

**Latency cost** — *延迟成本*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

The value lost by reacting slowly, equal to the optimal value with immediate response minus the value achievable at a given latency.

Lehalle–Mounjid measure it as the difference in value between an agent and a slower one whose control can change only every tau book updates, and read it as the maximum a trader should pay for speed.


<a id="term-liquidity"></a>

**Liquidity** — *流动性*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The ability to trade when you want to trade; split into immediacy, tightness, depth and resiliency, and behaving as a network externality.

Harris's operative definition; Hasbrouck splits it into immediacy (speed), tightness (a cheap round trip), depth (size near the quote) and resiliency (price effects that die out).


<a id="term-liquidity-rebate"></a>

**Liquidity rebate** — *流动性返佣*  <span class='bd-gloss-alpha'>First seen: Chapter 13</span>

The payment an exchange gives a resting limit order when it later executes, funded by the fee charged to marketable orders.

NASDAQ's recorded terms were USD 0.0029 per share to a visible maker and USD 0.0030 from a taker; inverted venues reverse the two.


<a id="term-liquidity-supplier"></a>

**Liquidity supplier** — *流动性提供者*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

A profit-motivated trader who is the counterparty so others can trade when they want — dealers across time, arbitrageurs across markets.

Harris notes dealers restore inventory by shifting both bid and ask; adverse selection is the main problem they face, and it widens the spread they must charge.


<a id="term-liquidity-adjusted-value-at-risk"></a>

**Liquidity-adjusted Value at Risk** — *流动性调整 VaR*  <span class='bd-gloss-alpha'>First seen: Chapter 5</span>

L-VaR, the smallest value at risk VaR_p(x)=lambda_v*sqrt(V(x))+E(x) attainable by any static liquidation strategy over a given horizon.


<a id="term-loss-versus-rebalancing-lvr"></a>

**Loss-versus-rebalancing (LVR)** — *再平衡损失*  <span class='bd-gloss-alpha'>First seen: Chapter 11</span>

The unhedgeable loss a liquidity provider suffers because the pool mechanically sells the appreciating asset and buys the depreciating one.

On a concentrated-liquidity AMM this is measured as predictable loss, which increases as the liquidity range narrows. It is the floor on AMM execution cost, since a taker cannot trade cheaper than the fee plus the loss the trade causes.


<a id="gloss-m"></a>

## M

<a id="term-make-take-choice"></a>

**Make/take choice** — *挂单—吃单选择*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The decision to post a resting order or cross the spread; posting is cheaper but may never execute, taking costs the spread and is certain.

Order books rank the resulting competition by price, then time, with displayed orders ahead of hidden ones; only comparative speed has value in that race.


<a id="term-maker-taker-pricing"></a>

**Maker-taker pricing** — *挂单—吃单定价*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

An exchange model paying a rebate to resting limit orders and charging more to liquidity-removing orders, keeping the difference.

On the Nasdaq schedule Hasbrouck records, a visible resting order earns USD 0.0029 per share and a taker pays USD 0.0030, so an identical quoted price is not an identical net price across venues.


<a id="term-market-depth"></a>

**Market depth** — *市场深度*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The order flow needed to move price by one unit; the reciprocal of Kyle's lambda, proportional to noise trading over unresolved information.

In Kyle's continuous limit depth is constant over time, which is why prices end up as constant-volatility Brownian motion.


<a id="term-market-fragmentation"></a>

**Market fragmentation** — *市场碎片化*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

The dispersal of one security's trading across many venues; it must be split into dispersal among lit venues and the lit-versus-dark divide.

The SEC staff review treats visible fragmentation and dark fragmentation as distinct phenomena with different empirical signatures (SEC 2013).


<a id="term-market-impact-model"></a>

**Market impact model** — *市场冲击模型*  <span class='bd-gloss-alpha'>First seen: Chapter 14</span>

The assumed rule for how a trade moves the price, split into temporary and permanent parts; every execution scheduler optimises against it.

The linear permanent-impact form of Kyle (1985) is the theoretical benchmark; the empirical literature (Toth et al. 2011; Bouchaud 2010) finds impact is concave in size and transient in time. Gueant (2013) shows permanent impact can be nonlinear and still arbitrage-free.


<a id="term-market-making"></a>

**Market making** — *做市*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

Quoting both a bid and an ask around a reference price and earning the spread, while carrying the inventory risk the fills create.

In the stochastic-control formulation the market maker controls the two quote distances, not a trading rate; the objective is expected utility of terminal mark-to-market wealth.


<a id="term-market-microstructure-invariance"></a>

**Market microstructure invariance** — *市场微观结构不变性*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The hypothesis that bet sizes and transaction costs are constant across assets once measured per unit of business time.

Kyle-Obizhaeva (2016) derive bet count ~ W^(2/3) and an illiquidity index ~ (sigma^2/PV)^(1/3); the square root is the only impact function that makes cost depend on Q/V alone.


<a id="term-market-resilience"></a>

**Market resilience** — *市场弹性*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The speed with which prices recover after a random, uninformative shock; the reason temporary impact is recoverable at all.

Bouchaud-Farmer-Lillo (2009) find the spread reverts with a power-law exponent of 0.4-0.5; order flow is long-memory, so impact must be transient for prices to stay near martingales.


<a id="term-markov-decision-process"></a>

**Markov decision process** — *马尔可夫决策过程*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

The formal object for sequential decisions: states, actions, transition probabilities, a reward, and a finite horizon or discount.

An execution problem becomes one of these once the state, action, transition and reward are fixed; the state usually splits into private variables (elapsed time, remaining inventory) and market variables.


<a id="term-mean-quadratic-variation"></a>

**Mean-quadratic variation** — *均值–二次变差*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

A time-consistent objective penalising expected cost, the running inventory squared, and the quadratic variation of the strategy.

Forsyth, Kennedy, Tse and Windcliff (2012) show its exact arithmetic-Brownian optimum coincides with the static Almgren–Chriss solution, which explains why the deterministic sinh schedule works so well.


<a id="term-metaorder"></a>

**Metaorder** — *元订单*  <span class='bd-gloss-alpha'>First seen: Chapter 13</span>

The bundle of child orders that implements one trading decision; the parent order whose impact the square-root law describes.

Tóth et al. (2011) define it as "the bundle of orders corresponding to a single trading decision"; impact is measured from the first to the last child order.


<a id="term-microprice"></a>

**Microprice** — *微价格*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

The expected long-run price conditional on the current order-book imbalance, used as the benchmark when placing a limit order.

Lehalle–Mounjid define it as the limit of the imbalance-conditional expectation of the price; comparing it with the execution price encodes adverse-selection aversion.


<a id="term-microstructure-invariance"></a>

**Microstructure invariance** — *微观结构不变性*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

The empirical regularity that, measured in business time, the distribution of bet sizes and the cost of a bet are the same across assets.

It gives the units (time, size, price) in which one execution policy can be made comparable across assets; Kyle and Obizhaeva estimate bet arrival as $W^{2/3}$ and mean bet as $W^{1/3}$.


<a id="term-model-free-reinforcement-learning"></a>

**Model-free reinforcement learning** — *无模型强化学习*  <span class='bd-gloss-alpha'>First seen: Chapter 14</span>

The learning paradigm that treats execution as a Markov decision process and learns a policy from experience, without a known impact model.

State is typically elapsed time, remaining inventory and market features; the action is traded quantity, a limit price, or a fraction across order types and levels. Results are simulation-trained, so the out-of-distribution and sim-to-real questions remain open.


<a id="gloss-n"></a>

## N

<a id="term-naive-strategy"></a>

**Naive strategy** — *平均切分策略（naïve 策略）*  <span class='bd-gloss-alpha'>First seen: Chapter 5</span>

The equal-split schedule that trades equal slices over equal intervals, minimising expected cost when risk is ignored.

It is the minimum of the Almgren–Chriss efficient frontier; Bertsimas and Lo prove it optimal only under a linear permanent-impact arithmetic random walk. Because the frontier is flat there, a risk-averse trader should never use it.


<a id="term-national-best-bid-and-offer"></a>

**National best bid and offer** — *全国最优买卖报价*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

The best protected bid and offer across all automated trading centers (NBBO), the reference price for price protection.

Only one accessible best bid and offer per venue family is protected, so the NBBO is built from a small number of quotations rather than the whole consolidated book.


<a id="term-nbbo"></a>

**NBBO** — *全国最优买卖报价*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

The National Best Bid and Offer — the highest bid and lowest offer across all trading centres, the reference for best execution and routing.

For NYSE- and AMEX-listed stocks it is computed from the Consolidated Quote System; the SEC designates such operators as Securities Information Processors (SIPs).


<a id="gloss-o"></a>

## O

<a id="term-offline-reinforcement-learning"></a>

**Offline reinforcement learning** — *离线强化学习*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

Learning a policy from a fixed, pre-collected dataset without interacting with the environment, where online exploration is too costly.

The known hazard is distributional shift: a policy trained off a fixed dataset meets only the states its own errors create and can drift away from the market's states.


<a id="term-open-outcry"></a>

**open outcry** — *公开喊价*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

The pre-electronic pit protocol in which bids and offers are cried aloud and quotes stay passive until someone hits or lifts them.

On the CME a bid was announced price first, quantity second, and there was no time priority at a price — a seller could hit any trader bidding the same price. The protocol's successors — price priority, trade-through rules, the make-or-take distinction — survive in electronic rulebooks.


<a id="term-opportunity-cost"></a>

**Opportunity Cost** — *机会成本*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

The cost on the unexecuted remainder of an order, marked at the terminal price; zero only if the order fills completely.


<a id="term-order-book-imbalance"></a>

**Order book imbalance** — *订单簿不平衡*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

The ratio of visible bid-side to ask-side liquidity at the best quotes, widely used as a short-horizon predictor of price and flow.

In Cebiroğlu and Horst (2011) it is the single channel through which a display decision affects the book, so the optimal display size trades time priority against the imbalance it creates.


<a id="term-order-flow-imbalance"></a>

**Order flow imbalance** — *订单流失衡*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The net of buys and sells at the best quotes; over short intervals it, rather than volume, drives the price change.

Cont-Kukanov-Stoikov (2014) find a linear relation between order flow imbalance and price change, with a slope inversely proportional to market depth.


<a id="term-order-protection-rule"></a>

**order protection rule** — *订单保护规则*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

Reg NMS Rule 611 — centres must try to avoid executions inferior to protected quotations; only immediately accessible quotes are protected.

The "immediately accessible" condition refuses protection to slow, manual quotations and forced floor markets to automate. It is not a prohibition — a market need only make a good-faith effort, and an order can instead be sent as an intermarket sweep order (ISO).


<a id="gloss-p"></a>

## P

<a id="term-paper-portfolio"></a>

**Paper Portfolio** — *纸上组合*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

A hypothetical holding of the positions an investor decided to hold, taken at the decision-time midquote with no costs.


<a id="term-participation-rate"></a>

**Participation rate** — *参与率*  <span class='bd-gloss-alpha'>First seen: Chapter 13</span>

The share of market volume during the execution window that the trader's own order accounts for; the variable a POV strategy fixes.

Zarinelli et al. write it as $\eta=Q/(V(t_e)-V(t_s))$, distinct from the duration $F$ and the daily fraction $\pi=\eta F$.


<a id="term-payment-for-order-flow"></a>

**Payment for order flow** — *订单流付费*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

A payment from a wholesaler to a broker in exchange for routing customer orders to it, permitted by the SEC but subject to disclosure.

Ernst and Spatt (2022) estimate about 40 cents per 100 shares in options against about 20 cents in equities, and the same nominal investment generates ten times as much PFOF in options.


<a id="term-percentage-of-volume"></a>

**Percentage of Volume** — *参与率*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

A strategy trading a constant fraction of market volume, so the horizon is implied; its optimal rate has a closed form in Guéant's model.


<a id="term-permanent-impact"></a>

**Permanent impact** — *永久冲击*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The part of a trade's price movement that reflects released information and stays in the price; it is not recoverable by waiting.

Identified with lambda times net order flow; it is a public-information event, not a rent paid to a liquidity provider.


<a id="term-perpetual-futures"></a>

**perpetual futures** — *永续合约*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

A futures contract with no expiry, anchored to spot by periodic funding payments; crypto's main venue, introduced by BitMEX in 2016.

In 2022 the median total daily perpetual-futures volume across exchanges was USD 101.9 billion, about two to three times total spot volume. A perpetual leg typically has a wider quoted spread but more depth and more balanced flow than spot.


<a id="term-picking-off-risk"></a>

**Picking-off risk** — *被抢单风险*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

The option value fast traders hold against a resting displayed order, which they can hit before it is cancelled.

Buti and Rindi (2009) show it is a distinct reason to hide: once fast traders are modelled, hidden orders become optimal at both decision nodes, placed at the second level of the book.


<a id="term-policy-distillation"></a>

**Policy distillation** — *策略蒸馏*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

Training a student policy to reproduce a teacher's action distribution, so the student keeps its behaviour without its extra inputs.

Fang et al. distil a teacher that sees perfect future information into a student that sees only current observables, removing future-information leakage at deployment.


<a id="term-policy-gradient"></a>

**Policy gradient** — *策略梯度*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

A family of methods that optimise the policy directly by estimating the gradient of expected reward with respect to the policy parameters.

Unlike value-based methods it handles continuous actions naturally, which is why it is used for end-to-end execution where the action is a trade size.


<a id="term-pre-commitment"></a>

**Pre-commitment** — *预承诺*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

Solving a mean-variance objective evaluated at the initial time, then committing to that policy even though it is no longer optimal later.

It is the standard repair for the time-inconsistency of the mean-variance criterion; Forsyth (2011) solves it numerically as a non-linear HJB equation and notes it is optimal only in the averaged sense over many identical trades.


<a id="term-predatory-trading"></a>

**Predatory trading** — *掠夺性交易*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

Front-running a trader whose liquidation need is observable, selling alongside them and buying back once they have pushed the price down.

Brunnermeier and Pedersen (2005) show the resulting price overshoot makes the distressed trader's excess cost equal to the predator's profit, and argue against strict disclosure of positions.


<a id="term-predictable-loss"></a>

**Predictable loss** — *可预测损失*  <span class='bd-gloss-alpha'>First seen: Chapter 11</span>

The unhedgeable loss of a concentrated-liquidity liquidity provider, computed from the pool's curvature and rising as the range narrows.

It is the concentrated-liquidity analogue of loss-versus-rebalancing and the symmetric counterpart of the convexity cost an AMM taker pays; fee income trades off against it.


<a id="term-price-discovery"></a>

**Price discovery** — *价格发现*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The process by which bids, asks and trades converge on a new value by aggregating heterogeneous beliefs; the outcome is unknown in advance.

Hasbrouck stresses that trading is required for it and that the fact and direction of a trade enter the public information set even without news.


<a id="term-price-improvement"></a>

**Price improvement** — *价格改善*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The amount by which an execution price beats the near-side quote, as in a dark trade or against a hidden order.

It is why the effective spread can be narrower than the posted spread; under maker-taker pricing the quoted spread is not the traded cost at all.


<a id="term-price-manipulation"></a>

**Price manipulation** — *价格操纵*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

A round trip that starts from zero inventory, trades back and forth, and ends at zero inventory with a positive expected profit.

Its absence is the standard no-arbitrage condition in limit-order-book models. Gatheral's theorem shows linear permanent impact is the only manipulation-free constant-coefficient specification; Guéant recovers non-linearity by making the microscopic impact depend on volume already executed.


<a id="term-principal-agent-problem"></a>

**Principal–agent problem** — *委托代理问题*  <span class='bd-gloss-alpha'>First seen: Chapter 12</span>

The misalignment between the broker who executes a trade and the client who bears its cost, set by the benchmark and the fee schedule.

Benchmark attribution decides who bears the trend component of a move, and a maker-taker fee split changes the broker's routing incentive; reading the two together as one agency problem is this chapter's judgement.


<a id="term-protected-quotation"></a>

**Protected quotation** — *受保护报价*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

A quote that is automated, shown by an automated trading center and is a venue family's best bid or offer; only these Rule 611 shields.

Rule 600(b)(57) sets the three elements. A quotation that fails the automation tests of Rule 600(b)(3) is a manual quotation and receives no protection.


<a id="term-proximal-policy-optimization"></a>

**Proximal policy optimization** — *近端策略优化*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

A policy-gradient method that clips each update so the new policy cannot move far from the old, giving stable training.

Lin and Beling use it end-to-end on raw level-2 data with a sparse reward against TWAP; their agents converge in under 200,000 steps.


<a id="gloss-q"></a>

## Q

<a id="term-q-learning"></a>

**Q-learning** — *Q 学习*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

A model-free algorithm that learns the expected value or cost of each action in each state by updating estimates from transitions.

Applied to execution it is often run by backward induction, exploiting the approximate independence of private and market variables so that tabular state spaces stay small.


<a id="term-quadratic-cost-model"></a>

**Quadratic cost model** — *二次成本模型*  <span class='bd-gloss-alpha'>First seen: Chapter 5</span>

The name for the linear-impact model, because trading n units in one interval costs epsilon*|n|+(eta/tau)*n^2, quadratic in the slice size.


<a id="term-queue-position"></a>

**Queue position** — *队列位置*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

An order's rank in the FIFO queue at its price. It sets both the probability of being filled and the adverse selection borne when filled.

Moallemi and Yuan (2016) value it as $V(q,\delta)=\alpha(q)\delta-\beta(q)$: fill probability times liquidity premium minus adverse-selection cost, all functions of queue position.


<a id="gloss-r"></a>

## R

<a id="term-realised-spread"></a>

**Realised spread** — *实现价差*  <span class='bd-gloss-alpha'>First seen: Chapter 13</span>

Twice the difference between the execution price and the quote midpoint measured a fixed interval after the order, usually five minutes.

Its difference from the effective spread approximates the order's own price impact; SEC Rule 605 requires market centres to publish both monthly.


<a id="term-realized-spread"></a>

**Realized spread** — *实现价差*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

Twice the gap between the execution price and the quote midpoint five minutes later; the provider's compensation once the price has moved.

Effective minus realized approximates the order's price impact. O'Hara (2015) notes it is time-frame-dependent and often negative in fast electronic markets.


<a id="term-reg-nms"></a>

**Reg NMS** — *全国市场系统条例*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

The SEC's Regulation National Market System (2005), which links the separate US equity trading centres into a virtually consolidated market.

It has four components — the Order Protection Rule (611), the Access Rule (610) with a USD 0.003 per-share access-fee cap, the Sub-Penny Rule (612), and the Market Data Rules. It was proposed February 2004, adopted June 2005 and phased in over 2006.


<a id="term-reservation-price"></a>

**Reservation price** — *保留价格*  <span class='bd-gloss-alpha'>First seen: Chapter 7</span>

The mid-price adjusted for the maker's inventory, at which the maker is indifferent about quoting; also called the indifference price.

Avellaneda–Stoikov give it in closed form as r(s,q,t)=s-q*gamma*sigma^2*(T-t); a long inventory quotes below the mid and a short inventory above it, and the adjustment vanishes as the horizon approaches.


<a id="term-resilience"></a>

**Resilience** — *复原速度*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

How fast new limit orders replenish the gap a trade tore in the book, i.e. the decay rate of the ask price to its steady state.

Obizhaeva–Wang write it as rho in the ask-price deviation term. It, not the static impact coefficient, governs the optimal schedule, and rho to 0 gives pure block trading while rho to infinity recovers the continuous Almgren–Chriss flow.


<a id="term-risk-aversion-parameter"></a>

**Risk-aversion parameter** — *风险厌恶参数*  <span class='bd-gloss-alpha'>First seen: Chapter 5</span>

The multiplier lambda in the objective E(x)+lambda*V(x), measuring how heavily cost variance is penalised relative to expected cost.

It is the Lagrange multiplier of the constrained problem min E s.t. V<=V*, the curvature of a smooth concave utility function, and it fixes where on the efficient frontier the trader sits. Lambda=0 is the risk-neutral special case.


<a id="term-roll-s-implicit-spread"></a>

**Roll's implicit spread** — *罗尔隐含价差*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

A quote-free estimator of the effective bid-ask spread from the first-order serial covariance of price changes, s = 2 sqrt(-cov).

It works because trading costs induce negative serial dependence; Roll (1984) reports 0.298 percent daily on CRSP 1963-1982, falling with firm size.


<a id="gloss-s"></a>

## S

<a id="term-sandwich-attack"></a>

**Sandwich attack** — *三明治攻击*  <span class='bd-gloss-alpha'>First seen: Chapter 11</span>

An MEV strategy that places trades before and after a victim's on-chain trade to worsen its price and profit from the move.

For n user trades the maximum price impact from sandwiching relative to the average is O(log n); sandwiched trades show a pronounced upward shift in routing sub-optimality. Batch trading at a single clearing price eliminates it.


<a id="term-settlement-latency"></a>

**Settlement latency** — *结算延迟*  <span class='bd-gloss-alpha'>First seen: Chapter 11</span>

The waiting time until a blockchain confirms a transfer, during which a cross-exchange arbitrageur is exposed to price risk.

It gives a no-trade band that widens with latency and volatility, so cross-venue routing is a control problem over settlement time, not only a fee comparison. Holding inventory on the destination venue removes the latency but substitutes exchange default risk.


<a id="term-smart-order-routing"></a>

**Smart order routing** — *智能订单路由*  <span class='bd-gloss-alpha'>First seen: Chapter 9</span>

Automated consolidation of several venues' quotes and optimisation of how a parent order splits across them in price, fee and fill odds.

In Foucault and Menkveld (2008) the share of brokers who are smart routers determines whether an entrant venue can survive at all, so routing technology is a condition of venue entry.


<a id="term-specialist"></a>

**specialist** — *专家经纪人*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

The NYSE member who ran one stock's limit-order book, owing narrow quotes and public priority, knowing the book and getting first look.

The specialist system is the template for every later market-making debate. His privileges are the ancestor of the modern high-frequency market maker's informational and speed advantage; his obligations the ancestor of affirmative and negative trading obligations.


<a id="term-split-order"></a>

**Split order** — *拆单*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

A large order broken into smaller pieces to conceal intent; Stenfors and Susai define it as same-price orders sent within 0.1 seconds.

About 17.42% of EBS EUR/USD limit orders meet this rule. Splitting is detected and, for a given size, triggers more liquidity withdrawal than equivalent human-sized orders.


<a id="term-square-root-law"></a>

**Square-root law** — *平方根律*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The empirical rule that the average price change of a metaorder scales as the square root of its size relative to volume, times volatility.

Written Delta = Y sigma (Q/V)^(1/2) with Y of order one; the exponent is more generally in 0.4-0.7, and Zarinelli et al. (2015) find a logarithmic form fits better.


<a id="term-stochastic-control"></a>

**Stochastic control** — *随机控制*  <span class='bd-gloss-alpha'>First seen: Chapter 14</span>

The paradigm that casts execution as a controlled state process plus an objective, and solves the resulting dynamic program or HJB equation.

Its lineage runs from Bertsimas-Lo (1998) and Almgren-Chriss (2000) through Obizhaeva-Wang, Gueant and the Forsyth HJB work. Relaxing its assumptions - linear impact, martingale price, mean-variance objective - defines most of the post-2000 execution literature.


<a id="gloss-t"></a>

## T

<a id="term-target-close"></a>

**Target Close** — *目标收盘*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

Execution benchmarked on the closing price, with a fixed end and an unknown start; the mirror image of implementation shortfall.


<a id="term-temporary-impact"></a>

**Temporary impact** — *暂时冲击*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

The part of a trade's price movement caused by exhausting resting liquidity; it decays as the book refills, so slowing down reduces it.

Carried by a decaying kernel G(l) in the propagator model; with sign correlation ~ l^-gamma the kernel must decay as l^-beta with beta = (1-gamma)/2.


<a id="term-tiered-maker-taker-fee"></a>

**Tiered maker-taker fee** — *分层做市—吃单费率*  <span class='bd-gloss-alpha'>First seen: Chapter 11</span>

An exchange fee charged as a fraction of traded notional, tiered by realised 30-day volume and differing between posting and taking.

Binance spot runs from 0.1000% maker/taker at retail to 0.0110%/0.0230% at VIP 9, maker below taker at every tier and zero negotiable for market makers. Because the tier depends on volume already traded, the marginal fee is history-dependent.


<a id="term-time-consistency"></a>

**Time consistency** — *时间一致性*  <span class='bd-gloss-alpha'>First seen: Chapter 14</span>

The property that the policy chosen today stays optimal later, so it can run as a feedback rule; mean-variance execution lacks it.

Mean-variance execution is a pre-commitment policy; Forsyth et al. (2012) replace it with a mean-quadratic-variation criterion that is time-consistent, and recover the Almgren-Chriss schedule as the arithmetic-Brownian-motion optimum.


<a id="term-time-inconsistency"></a>

**Time inconsistency** — *时间不一致*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

A criterion whose optimal policy at one date is no longer optimal later under unchanged conditions, so dynamic programming does not apply.

The mean-variance criterion is time-inconsistent because its variance term contains a squared expectation. Mean-quadratic-variation (Forsyth et al. 2012) and expected-utility objectives (Schied–Schöneborn 2009) are time-consistent alternatives.


<a id="term-trade-through"></a>

**trade-through** — *成交穿透*  <span class='bd-gloss-alpha'>First seen: Chapter 2</span>

An execution at a price inferior to a bid or offer standing elsewhere, so that a better-priced resting order is bypassed.

On the CME it was a trading infraction (Rule 528). In Reg NMS, Rule 611 requires trading centres to make a good-faith effort to avoid trade-throughs of protected quotations only.


<a id="term-trading-trajectory"></a>

**Trading trajectory** — *交易轨迹*  <span class='bd-gloss-alpha'>First seen: Chapter 5</span>

The sequence of holdings x_0,...,x_N from the initial block X down to zero at the horizon, equivalently described by the trade list n_k.


<a id="term-trading-enhanced-risk"></a>

**Trading-enhanced risk** — *交易放大的风险*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

The part of execution-cost variance that grows with trade size itself, because large trades are more exposed to uncertain liquidity.

Almgren (2003) adds a random component h(V)*xi to temporary impact, which puts the term sum V^2 h(V)^2 into the variance of terminal wealth and slows the optimal schedule.


<a id="term-transaction-cost-analysis"></a>

**Transaction Cost Analysis** — *交易成本分析*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

The measurement loop around execution, covering pre-trade forecasting, post-trade attribution, benchmark choice and significance testing.


<a id="term-transient-price-impact"></a>

**Transient price impact** — *瞬态冲击*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

Price impact that is strongest immediately after a trade and then decays, rather than persisting as a permanent shift of the price.

It replaces the Almgren–Chriss split into permanent plus instantaneous impact. Lorenz–Schied show that under transient impact the optimal strategy depends on the derivative of the drift, making it far more fragile than under Almgren–Chriss.


<a id="gloss-u"></a>

## U

<a id="term-utilitarian-trader"></a>

**Utilitarian trader** — *功利交易者*  <span class='bd-gloss-alpha'>First seen: Chapter 3</span>

A trader who trades to solve a non-trading problem and accepts the costs; his willingness to pay funds all profit-motivated traders.

Harris's counterpart is the profit-motivated trader, split into informed traders, parasitic traders (order anticipators, bluffers), and liquidity suppliers.


<a id="gloss-v"></a>

## V

<a id="term-volume-effect-process"></a>

**Volume effect process** — *成交量效应过程*  <span class='bd-gloss-alpha'>First seen: Chapter 6</span>

The part of a trade that is still visible in the limit order book and decays as the book recovers, driving the current ask price.

Predoiu–Shaikhet–Shreve model it as E_t = X_t minus the integral of the recovery rate h(E_s); the ask price is the shadow price plus psi(E_t), with psi the inverse of the book's cumulative depth.


<a id="term-volume-weighted-average-price"></a>

**Volume-Weighted Average Price** — *成交量加权平均价*  <span class='bd-gloss-alpha'>First seen: Chapter 4</span>

The average price weighted by volume traded at each price over a period; an execution benchmark that can be influenced.


<a id="term-vpin"></a>

**VPIN** — *成交量同步的知情交易概率*  <span class='bd-gloss-alpha'>First seen: Chapter 8</span>

A real-time, volume-time measure of order-flow toxicity computed as average absolute buy-sell imbalance per volume bucket.

Defined by Easley, López de Prado and O'Hara (2012) as $\text{VPIN}=\sum_\tau|V_\tau^S-V_\tau^B|/(nV)$, with $V$ one-fiftieth of daily volume and $n=50$ buckets in the baseline.


<a id="gloss-z"></a>

## Z

<a id="term-zero-shot-transfer"></a>

**Zero-shot transfer** — *零样本迁移*  <span class='bd-gloss-alpha'>First seen: Chapter 10</span>

Applying a policy to assets or markets it was never trained on, with no further learning, and measuring the cost gap it suffers.

Tran and Yu train once on 954 stocks and test unchanged on unseen stocks, crypto and a second exchange, finding a transfer gap of within 0.015 spreads per share that does not reach unseen spread regimes.
