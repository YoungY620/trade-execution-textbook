# Market making and the stochastic-control view

Market making is the mirror image of execution. An executor walks up to the
book and pays the spread for immediacy; a market maker stands in the book,
posts both sides, and is paid the spread for standing there. Both hold an inventory they do not want and would like to trade only against
uninformative counters. Written as
stochastic control they are one problem with the controls swapped: the executor
chooses a trading rate and pays, the market maker chooses a pair of quote
distances and collects.

## The control problem

The canonical state at time $t$ is the triple (inventory $q$, cash $X$,
reference or mid-price $S$). The market maker controls the two quote distances
from the mid, the ask distance $\delta^a=p^a-S$ and the bid distance
$\delta^b=S-p^b$. The mid-price is a local martingale, $dS_u=\sigma\,dW_u$ with
$S_t=s$, so there is no predictable drift to trade on; we relax this later.
Inventory and cash move only when a market order arrives,

$$q_t=N^b_t-N^a_t,\qquad dX_t=(S_t+\delta^a_t)\,dN^a_t-(S_t-\delta^b_t)\,dN^b_t,$$

where $N^a$ (respectively $N^b$) counts the market orders that lift the ask and
hit the bid. The key modelling step is the arrival intensity: a quote further
from the mid is filled less often, so we take decreasing functions
$\lambda^a(\delta^a)$ and $\lambda^b(\delta^b)$. The objective is the expected
utility of terminal mark-to-market wealth,

$$u(s,x,q,t)=\max_{\delta^a,\delta^b}\mathbb E_t\!\left[-\exp\!\left(-\gamma(X_T+q_TS_T)\right)\right],$$

with $\gamma>0$ the absolute risk aversion. Dynamic programming turns this into
a Hamilton–Jacobi–Bellman (HJB) equation whose local part is
$u_t+\tfrac12\sigma^2u_{ss}$ and whose non-local part maximises the two jump
terms $\lambda^b(\delta^b)[u(s,x-s+\delta^b,q+1,t)-u]$ and its ask mirror. The
structure — a price diffusion, Poisson fills, a maximisation over quote
distances, a terminal utility — is the template for everything below.

## Avellaneda–Stoikov: reservation price and optimal spread

Avellaneda and Stoikov close the model with exponentially decaying arrival
intensities, a form distilled from the power-law order-size and impact
statistics of the econophysics literature:

$$\lambda^a(\delta^a)=Ae^{-k\delta^a},\qquad \lambda^b(\delta^b)=Ae^{-k\delta^b}.$$

Here $A$ is the intensity at zero distance, in fills per unit time, and $k$ is
the decay rate of fill intensity per unit of price, so that $k\delta$ is
dimensionless. With exponential utility the ansatz
$u=-\exp(-\gamma x)\exp(-\gamma\theta(s,q,t))$ strips out the cash variable. The
agent's subjective valuation of the stock is then the **reservation price**
(also called indifference price), the mid adjusted for the inventory held:

$$r(s,q,t)=s-q\gamma\sigma^2(T-t).$$

The correction $-q\gamma\sigma^2(T-t)$ is the inventory skew: a long inventory
($q>0$) values the stock below the mid and quotes down to sell it, a short
inventory quotes up to buy it back, and the term vanishes as $t\to T$ because
near the horizon any inventory can be marked out at the mid. Reading the
frozen-inventory prices separately gives the bid and ask reservation prices
$r^b(s,q,t)=s+\tfrac{(-1-2q)\gamma\sigma^2(T-t)}{2}$ and
$r^a(s,q,t)=s+\tfrac{(1-2q)\gamma\sigma^2(T-t)}{2}$, whose average is $r$.

The optimal quote distances solve implicit first-order conditions
$s-r^b(s,q,t)=\tfrac1\gamma\ln\!\left(1-\tfrac{\lambda^b(\delta^b)}{\gamma\,(\partial\lambda^b/\partial\delta)(\delta^b)}\right)$
and symmetrically for the ask. For exponential intensities the total spread has
the closed form, centred on $r$:

$$\delta^a+\delta^b=\gamma\sigma^2(T-t)+\frac{2}{\gamma}\ln\!\left(1+\frac{\gamma}{k}\right).$$

Two features matter. The spread splits into a risk term $\gamma\sigma^2(T-t)$,
growing with volatility, risk aversion and time left to carry the position, and
a liquidity term $\tfrac2\gamma\ln(1+\gamma/k)$, growing with $\gamma$ and $k$. And the spread is *independent of the inventory*: with
exponential intensities all inventory information sits in the skew $r-s$, not
the width. That is an artefact of the exponential form, not a general law, and
later models break it. 
## Guéant–Lehalle–Fernandez-Tapia: an inventory bound and a spectral solution

Avellaneda–Stoikov is an approximation: the closed forms come from a
first-order expansion in $q$, and the admissibility of the quotes was open.
Guéant, Lehalle and Fernandez-Tapia replace the expansion by a hard inventory
bound $q\in\{-Q,\dots,Q\}$ — no bid at $q=Q$, no ask at $q=-Q$ — and solve
exactly. With $\alpha=\tfrac12 k\gamma\sigma^2$ and
$\eta=A\left(1+\tfrac{\gamma}{k}\right)^{-(1+k/\gamma)}$, the HJB collapses to
a linear tridiagonal ODE system for the levels $v_q(t)$,

$$\dot v_q(t)=\alpha q^2 v_q(t)-\eta\left(v_{q-1}(t)+v_{q+1}(t)\right),$$

absorbing at the two boundaries and $v_q(T)=1$; the value function is
$-\exp(-\gamma(x+qs))\,v_q(t)^{-\gamma/k}$. The optimal quotes are

$$\delta^{b*}(t,q)=\frac1k\ln\!\left(\frac{v_q(t)}{v_{q+1}(t)}\right)+\frac1\gamma\ln\!\left(1+\frac{\gamma}{k}\right),\qquad
\delta^{a*}(t,q)=\frac1k\ln\!\left(\frac{v_q(t)}{v_{q-1}(t)}\right)+\frac1\gamma\ln\!\left(1+\frac{\gamma}{k}\right),$$

with spread
$\psi^*(t,q)=-\tfrac1k\ln\!\left(\tfrac{v_{q+1}(t)v_{q-1}(t)}{v_q(t)^2}\right)+\tfrac2\gamma\ln\!\left(1+\tfrac{\gamma}{k}\right)$.
The $v_q$ are the levels of a birth–death problem, and the quotes are almost
independent of $t$ far from the horizon — at odds with the time-increasing
spread the expansion suggests. Letting $T\to\infty$, the asymptotic quotes are
$\delta_\infty^{b*}(q)=\tfrac1\gamma\ln(1+\tfrac\gamma k)+\tfrac1k\ln(f^0_q/f^0_{q+1})$,
where $f^0$ is the eigenvector for the smallest eigenvalue of the tridiagonal
matrix. Replacing the finite grid by $L^2(\mathbb R)$ gives a Gaussian
$f^0(x)\propto\exp(-\tfrac12\sqrt{\alpha/\eta}\,x^2)$ and the celebrated
approximation, affine in inventory,

$$\delta_\infty^{b*}(q)\simeq\frac1\gamma\ln\!\left(1+\frac{\gamma}{k}\right)+\frac{2q+1}{2}\sqrt{\frac{\sigma^2\gamma}{2kA}\left(1+\frac{\gamma}{k}\right)^{1+k/\gamma}},$$

with the mirror image on the ask side and
$\psi_\infty^*(q)\simeq\tfrac2\gamma\ln(1+\tfrac\gamma k)+\sqrt{\tfrac{\sigma^2\gamma}{2kA}(1+\tfrac\gamma k)^{1+k/\gamma}}$.
The inventory sensitivity of the quotes is the square-root factor
$\sqrt{\sigma^2\gamma/(2kA)}$: quotes widen with volatility and risk aversion and
tighten with fill intensity. The effect of $\gamma$ is ambiguous,
because a more risk-averse maker both widens the spread against price risk and
narrows it against the risk of never being filled.

## Guéant–Royer: general intensities, an inventory penalty, many assets

Guéant replaces the hard bound by a terminal inventory penalty
$\ell(|q_T|)$, nondecreasing and convex, and lets the intensities
$\Lambda^b,\Lambda^a$ be arbitrary decreasing functions (a mild condition on
$\Lambda\Lambda''/(\Lambda')^2$ ensures the per-trade problem $\delta\Lambda(\delta)$
has a unique maximiser). Two objectives are shown to be the same family:

$$\text{(Model A)}\quad \sup\mathbb E\!\left[-\exp\!\left(-\gamma(X_T+q_TS_T-\ell(|q_T|))\right)\right],$$

$$\text{(Model B)}\quad \sup\mathbb E\!\left[X_T+q_TS_T-\ell(|q_T|)-\tfrac12\gamma\sigma^2\!\int_0^T q_t^2\,dt\right].$$

Model A is CARA utility of terminal wealth; Model B is the Cartea–Jaimungal-type
criterion, penalising inventory *continuously* through the running term. Both
reduce to a single ODE for $\theta(t,q)$,

$$0=-\partial_t\theta(t,q)+\tfrac12\gamma\sigma^2q^2-\mathbf 1_{q<Q}H^b_\xi\!\left(\tfrac{\theta(t,q)-\theta(t,q+\Delta)}{\Delta}\right)-\mathbf 1_{q>-Q}H^a_\xi\!\left(\tfrac{\theta(t,q)-\theta(t,q-\Delta)}{\Delta}\right),$$

with $\theta(T,q)=-\ell(|q|)$, where
$H^b_\xi(p)=\sup_\delta\tfrac{\Lambda^b(\delta)}{\xi}(1-e^{-\xi\Delta(\delta-p)})$,
$\Delta$ is the trade size, and $\xi=\gamma$ for Model A against $\xi=0$ for
Model B. The parameter $\xi$ is exactly the distinction between aversion to
*price risk* (Model A, where risk enters only through the utility curvature) and
to *non-execution risk* (Model B, where $\gamma$ penalises inventory held over
time). With exponential intensities the change of variable
$v_q(t)=\exp(\tfrac k\Delta\theta(t,q))$ linearises the system, recovering the
Guéant–Lehalle–Fernandez-Tapia reduction; expanding $H_\xi$ to second order and
passing to the continuum gives

$$\delta^{b*}_{\text{approx}}(q)=\frac1{\xi\Delta}\ln\!\left(1+\frac{\xi\Delta}{k}\right)+\frac{2q+\Delta}{2}\sqrt{\frac{\gamma\sigma^2}{2\Delta kA}\left(1+\frac{\xi\Delta}{k}\right)^{1+k/(\xi\Delta)}},$$

which for $\xi=\gamma,\Delta=1$ is the Guéant–Lehalle–Fernandez-Tapia formula
and for $\xi=0$ covers Model B. Section 5 extends the construction to a maker in
charge of $d$ correlated assets with covariance matrix
$\Sigma=(\rho^{i,j}\sigma^i\sigma^j)$: the single-asset matrix becomes a matrix
Riccati-type system, and in the ergodic limit the quadratic coefficient
stabilises at $\tfrac12\sqrt{\gamma/2}\,\Gamma$ with
$\Gamma=D^{-1/2}(D^{-1/2}\Sigma D^{-1/2})^{1/2}D^{-1/2}$, $D$ collecting the
second derivatives $H^{i,\prime\prime}_\xi(0)$. A maker short one bond and long a highly correlated one should not skew both
quotes, because the portfolio is nearly flat.

## Guilbaud–Pham: limit and market orders in one problem

Everything so far is OTC-style: unit trades at Poisson intensities. Guilbaud and
Pham build a genuine limit-order-book model. Time is a tick clock whose
intensity is estimated from data, the spread is a continuous-time Markov chain
on the tick grid, and the agent may improve the best quote by one tick to gain
execution priority — quoting better is filled more often. Crucially the agent
also has a *second* control: it may send market orders. Because market orders
are instantaneous and can be sent at arbitrary stopping times, they are modelled
as **impulse control**, not as a rate. The dynamic programming equation is then
an **HJB quasi-variational inequality (HJBQVI)**,

$$\min\left\{-\frac{\partial v}{\partial t}-\sup_{(q,\ell)\in\mathcal Q(s)\times[0,\bar\ell]^2}\mathcal L^{q,\ell}v+\gamma g\;,\; v-\mathcal Mv\right\}=0,$$

where $\mathcal L^{q,\ell}$ generates the limit-order dynamics (price, spread
chain, two fill jumps) and the impulse operator
$\mathcal Mv=\sup_{e\in[-\bar e,\bar e]}v(t,x-c(e,p,s),y+e,p)$ is the value of
*taking* liquidity, at cost $c$ equal to the half-spread plus a fee. The
inequality says: either the state keeps running under the optimal limit-order
policy, or the agent jumps by sending a market order. Ordinary HJB cannot carry
this, because the two controls act on different time sets — limit orders apply
continuously, market orders at isolated instants — and the minimisation is what
makes the impulse channel competing rather than additive. This is the model of
the cluster closest to real book mechanics, and the cleanest statement that
market making and execution are one problem: the same agent both posts and
takes.

## Cont–Stoikov–Talreja: the microstructure beneath the intensities

Where do the fill intensities come from? Cont, Stoikov and Talreja model the
book as a queueing system on $n$ price levels with unit order size. Six event types drive it: limit buy and sell
arrivals, market buy and sell orders, and cancellations of resting orders, each
an independent Poisson process whose rate depends only on the distance to the
best quotes. The process is a continuous-time Markov chain; the admissible
(pyramidal, non-crossed) states are absorbing, and if the minimum market-order
rate is positive the chain is ergodic with an explicit stationary distribution.
The pay-off is tractability: first-passage techniques for birth–death processes,
through two-sided Laplace transforms, give **closed-form conditional
probabilities** — that the mid-price's next move is up, that an order at the bid
executes before the ask quote moves, and that both a buy and a sell execute
before the price moves. "Probability of being filled" is thus not an assumed
$Ae^{-k\delta}$ but a computable function of the book state, the microfoundation
on which quoting and order-placement policies rest.

## Cartea–Jaimungal–Ricci: buy low, sell high

The models above price inventory risk but contain no view on direction. Cartea,
Jaimungal and Ricci add one through self- and mutually-exciting order flow: the
mid-price carries a predictable short-term alpha $\alpha_t$ and the arrival
intensities of buy and sell market orders follow coupled Hawkes-like dynamics,
so trades and the book shape influence each other. The maker maximises terminal
wealth minus a running inventory penalty $\varphi\int q^2$. The optimal quote is
a base distance plus two corrections,

$$\delta^{\pm*}=\delta^\pm_0+B(\delta^\pm_0;\kappa^\pm)\left\{\pm\mathcal S^\pm_\lambda\,\mathbb E\!\left[\int_t^T\alpha_u\,du\right]+\varphi\left[\pm\mathcal S^\pm_\lambda b_\varphi+(1\mp2q)(T-t)\right]\right\},$$

where $\delta^\pm_0$ is the risk-neutral quote. The two roles are now explicit:
the alpha term *simultaneously* corrects adverse selection and positions the
quote directionally — with a positive expected drift the sell quote is pushed
deeper and the buy quote pulled closer — while the penalty term provides
inventory mean reversion. Market making and speculative holding are separated,
and the maker chooses how much of each by trading spread capture against the
pick-off risk the direction exposes it to.

## Signals and the bridge to execution

Two papers make the link to Chapter 6 concrete. Lehalle and Neuman study
optimal execution with a Markovian signal $I$ (alpha) and transient market
impact. The minimiser is characterised by a first-order condition: $X^*$ is
optimal iff there is a constant $\lambda$ with
$\int_0^t\mathbb E_\iota[I_s]ds+\int_0^tG(|t-s|)dX^*_s-2\varphi\int_0^tX^*_s ds=\lambda$
for all $t$. For an Ornstein–Uhlenbeck signal $dI_t=-\gamma I_t\,dt+\sigma\,dW_t$
and exponentially decaying impact, the strategy is closed-form and *linear in
both the initial inventory and the signal*. The notable qualitative result is
that it need not be monotone: a signal can make an agent buy while liquidating,
precisely the condition under which price manipulation becomes possible. Cartea
and Wang make the market-making counterpart explicit: with alpha driven by
market orders, the maker cancels or pushes away the risk-side limit order as
$|\alpha|$ grows, and beyond a threshold switches to sending *market* orders in
the direction of the signal. As its tolerance to inventory risk rises, the
strategy takes more speculative market orders and does more limit-order round
trips, and there is a range of tolerance over which expected profit rises while
its volatility barely moves. Execution and market making thus run on the same
signal-driven machinery, only the sign of the activity differs.

## One problem, two faces

Read together, the cluster is one statement: market making and execution are the
same stochastic control problem seen from opposite sides. Drissi's Oxford
lecture notes make this the organising principle of the field — optimal trading
problems "share a common structure": identify the decision problem, summarise
the environment in a few state variables, frame it as an optimisation, and solve it. The state is inventory, cash and price in both;
the controls differ only in sign; and the trade-off is identical — inventory
risk, adverse selection and impact. An aggressive order is a maker's decision to
take rather than post, exactly the impulse channel of Guilbaud–Pham; a resting
limit order is an executor's decision to post rather than take. Guéant's remark
that many market-making models "can be regarded as generalizations of optimal
execution models" points the same way.

## Adverse selection: the bridge to Chapter 3

Chapter 3's Glosten–Milgrom argument says that a spread can exist for purely
informational reasons: if some counterparties know the value, the quotes that
break even conditional on the trade must straddle it, so
$A=E[V\mid\text{buy}]\ge E[V]\ge B=E[V\mid\text{sell}]$, and the spread widens
when informed arrivals are more frequent or better informed. The market-making
literature imports this in two ways. Quantitatively, adding adverse selection to
the Guéant–Lehalle–Fernandez-Tapia model through a price impact
$dS_t=\sigma\,dW_t+\xi\,dN^a_t-\xi\,dN^b_t$ rescales the off-diagonal fill
coupling by $e^{-k\xi/2}$ and injects a constant $\xi$ into the spread, so the
quote is a base plus an adverse-selection surcharge. Procedurally, Lehalle and
Mounjid ask *where and when to place one limit order* to minimise the risk of
being picked off. Order-book imbalance predicts the next price move, and the
agent compares the expected execution price not with the current mid but with
the **microprice**, the imbalance-conditional long-run price
$P_{+\infty}(t)=\lim_{\delta t\to+\infty}\mathbb E(P_{t+\delta t}\mid P_t,\mathrm{Imb}_t)$.
The value of exploiting the imbalance is eroded by latency: modelling a slower
agent by a latency factor $\tau$ that limits how often the order can be revised,
the latency cost $\mathrm{Latency}_{U_0,f}(\tau)=V_{U_0,f}-V_{U_0,f,\tau}$ grows
quickly in $\tau$, and it is optimal to cancel and re-post more actively near
the horizon. The adverse-selection component of the spread is thus not constant:
it is the expected loss to better-informed flow, and it is what makes latency
worth paying for.

## Sources

- Avellaneda, M. and Stoikov, S. (2008). *High-frequency trading in a limit
  order book*. Quantitative Finance 8(3), 217–224.
  https://www.math.nyu.edu/~avellane/HighFrequencyTrading.pdf
- Guéant, O., Lehalle, C.-A. and Fernandez-Tapia, J. (2013). *Dealing with the
  inventory risk: a solution to the market making problem*. Mathematics and
  Financial Economics 7(4), 477–507. https://arxiv.org/abs/1105.3115
- Guéant, O. (2017). *Optimal market making*. Applied Mathematical Finance
  24(2), 112–154. https://arxiv.org/abs/1605.01862
- Guilbaud, F. and Pham, H. (2013). *Optimal high frequency trading with limit
  and market orders*. Quantitative Finance. https://arxiv.org/abs/1106.5040
- Cont, R., Stoikov, S. and Talreja, R. (2010). *A stochastic model for order
  book dynamics*. Operations Research 58(3), 549–563.
  http://rama.cont.perso.math.cnrs.fr/pdf/CST2010.pdf
- Cartea, Á., Jaimungal, S. and Ricci, J. (2014). *Buy low, sell high: a high
  frequency trading perspective*. SIAM Journal on Financial Mathematics 5(1),
  415–444.
- Lehalle, C.-A. and Neuman, E. (2019). *Incorporating signals into optimal
  trading*. Finance and Stochastics 23(2), 275–311.
  https://arxiv.org/abs/1704.00847
- Cartea, Á. and Wang, Y. (2020). *Market making with alpha signals*.
  International Journal of Theoretical and Applied Finance 23(3), 2050016.
- Lehalle, C.-A. and Mounjid, O. (2016). *Limit order strategic placement with
  adverse selection risk and the role of latency*.
  https://arxiv.org/abs/1610.00261
- Bergault, P., Evangelista, D., Guéant, O. and Vieira, D. (2021). *Closed-form
  approximations in multi-asset market making*. Applied Mathematical Finance.
  https://arxiv.org/abs/1810.04383
- Drissi, F. (2024). *Lecture notes on market microstructure and algorithmic
  trading*, University of Oxford.

## Key points

- The market-making problem is a stochastic control problem with inventory,
  cash and mid-price as state, the two quote distances as controls, and CARA or
  mean–variance utility of terminal mark-to-market wealth as objective.
- Avellaneda–Stoikov's reservation price $r(s,q,t)=s-q\gamma\sigma^2(T-t)$
  shifts quotes against the inventory and decays to the mid as the horizon
  nears; their optimal spread
  $\delta^a+\delta^b=\gamma\sigma^2(T-t)+\tfrac2\gamma\ln(1+\gamma/k)$ splits
  into a risk term and a liquidity term and, with exponential intensities, does
  not depend on the inventory.
- Guéant–Lehalle–Fernandez-Tapia replace the inventory expansion by a hard
  bound and reduce the HJB to a linear tridiagonal ODE whose spectral solution
  yields the asymptotic quotes and the affine-in-inventory approximation with
  sensitivity $\sqrt{\sigma^2\gamma/(2kA)}$.
- Guéant–Royer generalise the intensities and show that a terminal CARA
  criterion and a running quadratic inventory penalty are one family of ODEs,
  that $\xi$ separates price-risk from non-execution-risk aversion, and that the
  construction extends to many correlated assets.
- Once the agent can also send market orders, the dynamic programming equation
  becomes an HJB quasi-variational inequality: limit orders are a regular
  control and market orders an impulse control, so one value function covers
  posting and taking.
- Cont–Stoikov–Talreja give the microscopic foundation, modelling the book as a
  birth–death queueing network and computing fill probabilities and price-move
  probabilities in closed form from the book state.
- Cartea–Jaimungal–Ricci and Cartea–Wang show how an alpha signal enters the
  optimal quote: it corrects adverse selection and positions directionally, and
  a large enough signal turns the maker into a taker.
- Market making and execution are the same control problem viewed from opposite
  sides; the adverse-selection surcharge in the maker's spread is the same
  quantity as the informational cost in Chapter 3's Glosten–Milgrom model.
