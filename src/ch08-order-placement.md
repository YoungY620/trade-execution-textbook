# Order books, order types and stealth

Every execution schedule reaches a decision it does not make for the trader: which
order type, where to post, how much to display, and where. These choices set the cost
and decide how much of the intention becomes readable by others.

## Order types and their cost structure

A market order buys immediacy: it executes at the best opposite quote, paying half
the spread plus the venue fee, and beyond the touch it moves the price. In Cont and
Kukanov (2017), $h$ is **one-half of the bid-ask spread** at time 0, $f$ is the
market-order fee, and a market order of size $M$ costs $(h+f)M$ relative to the
mid-quote before impact.

A limit order trades immediacy for price improvement, earning $h$ plus the **effective
rebate** $r_k=r_k^{e}+AS_k$, which nets the exchange's rebate against $AS_k$, an
exchange-specific adverse-selection penalty measured as average returns after a
limit-order fill. Two risks offset the saving: the order may not fill by the deadline,
and it may fill because the price was about to move against the resting side. Cont and
Kukanov price these as per-share penalties $\lambda_u$ for falling short of the target
$S$ and $\lambda_o$ for exceeding it.

An **iceberg order** displays a peak smaller than its total, refreshing a new peak
from the hidden remainder as each peak executes; a **hidden order** displays nothing.
Both forfeit time priority to displayed orders at the same price — on Xetra, a
displayed order outranks hidden orders regardless of submission time. The gain is
that the whole size is not standing on the book to be read and picked off.

Conditional and time-weighted orders (arrival, participation, VWAP/TWAP targets) are
scheduling devices that fix the slice the placement problem routes, and Cartea and
Jaimungal (2016) show that the schedule should itself respond to expected net order
flow.

## The limit-versus-market split

Cont and Kukanov (2017) fix a slice $S$ to buy within $[0,T]$, with the schedule
given, and choose $X=(M,L_1,\dots,L_K)$: one market order and one limit order per
venue, posted at the National Best Bid, joining queues of size $Q_k$. Let
$\xi_k=C_k+D_k$ be the outflow from the front of queue $k$ — cancellations plus
marketable orders reaching the venue — so the limit order at $k$ fills
$\min(\max(\xi_k-Q_k,0),L_k)$ shares. Their cost function charges the spread and fee
on the market order, credits the effective rebate on limit fills, and adds $\theta$ per
share of linear market impact, with $\lambda_u,\lambda_o$ the per-share penalties for
under- and overfilling $S$. Under the three restrictions $\min_k r_k+h>0$,
$\lambda_o>h+\max_k r_k$ and $\lambda_u>h+f$, the objective is convex, bounded below,
and has a global minimiser: no single order is oversized, but the *sum* may
deliberately exceed $S$.

With one venue the split is closed-form (their Proposition 3). For
$\underline{\lambda_u}<\lambda_u<\overline{\lambda_u}$,

$$L^{*}=F^{-1}\!\left(\frac{2h+f+r}{\lambda_u+h+r+\theta}\right)-Q,\qquad M^{*}=S-L^{*},$$

where $F$ is the CDF of the queue outflow $\xi$ and $F^{-1}$ its left-inverse. The
quantile level is a ratio of marginal costs to savings: raising $M$ by one share costs
$h+f$ and, since $M+L=S$, forfeits $h+r$ of saving that share would have earned
passively — the numerator $2h+f+r$; against it, a limit order that does not fill forces
a costly catch-up of $\lambda_u+\theta$ — the denominator. At the thresholds the
corners take over: all-limit $(0,S)$ below $\underline{\lambda_u}$, all-market
$(S,0)$ above $\overline{\lambda_u}$. A stronger short-term view raises $\lambda_u$,
shrinks $L^{*}$ and crosses more.

Three features matter for hiding. The optimum depends on the **full distribution**
$F$, not its mean, since fills come from the tail $\Pr(\xi>Q+L)$. Limit-order
capacity is **bounded**: as $S$ grows, $L^{*}$ stops growing and the extra quantity
goes to market orders, since filling a large limit order would need an implausibly
large outflow. And across venues the optimum **equates shortfall probabilities**
(Proposition 4), posting more total limit size than the target needs — deliberate
overbooking that grows with the number of venues because fills are imperfectly
correlated. That explains "phantom liquidity" as risk diversification: the visible
book overstates execution intent, without anyone spoofing.

## What a queue position is worth

Moallemi and Yuan (2016) value being early in the queue. Under price-time priority a
resting order's expected value decomposes as $V_t=\alpha_t(\delta_t-AS_t)$, where
$\alpha_t=\Pr(\text{FILL}\mid\mathcal F_t)$ is the fill probability, $\delta_t$ the
liquidity premium (posted price minus latent efficient price, a half-spread when the
latent price is the mid), and $AS_t=\mathbb{E}[P_{\tau^*}-P_t\mid\text{fill}]$ the
adverse-selection cost. With Poisson trades, price jumps and cancellations and state
$(\delta,q)$ for queue position $q$, the value function is quasi-linear in the
premium:

$$V(q,\delta)=\alpha(q)\delta-\beta(q).$$

The fill probability $\alpha(q)$ falls with queue position because the queue drains
in order, and the adverse-selection cost $\beta(q)/\alpha(q)$ rises with $q$, because
an order at the back of a long queue is more likely to be hit by a large, informed
trade. With two-sided exponential trade sizes,
$\alpha(q)=p_J^++\frac{\mu(1-p_J^+)}{\mu+2\gamma}e^{-bq}$,
$b=(\gamma+\zeta)\theta/(\mu/2+\gamma)$, so value decays exponentially in position. On
BAC, the front of the queue was worth about 0.26 ticks more than the average position
on 8/9/2013 and 0.21 on 8/20/2013 — comparable to the whole spread.

## How much to show

Display trades time priority against exposure. Cebiroğlu and Horst (2011) let an
iceberg trader choose a display size $\Delta\in[0,N]$ affecting the book only through
the top-of-book imbalance $I(\Delta)$, increasing in $\Delta$. The marginal effect of
showing one more share splits into two terms:

$$\frac{d}{d\Delta}W\coloneqq M_{\mathrm{Market}}-M_{\mathrm{Priority}},$$

where $M_{\mathrm{Priority}}$, the gain from time priority, is always a cost
reduction, while $M_{\mathrm{Market}}$, the exposure impact on flows and prices, can
cut either way. If $W$ is strictly convex with an interior optimum,
$M_{\mathrm{Market}}(\Delta^{*})=M_{\mathrm{Priority}}(\Delta^{*})$: show until the
market's reaction to your visibility offsets the queue position you gain. Where
exposure is rewarded, full display $\Delta^{*}=N$ is optimal; for large orders the
sign flips, and beyond a size $N_d$, $\Delta^{*}=0$ when price drifts away from
exposure. Calibrated on NASDAQ data, exposure impact runs mainly through competing
("front-running") limit order flow and the quoted opposite price, not market order
flow, and icebergs can cut execution cost by up to 60% versus plain limit orders — but
only for large orders with the imbalance skewed toward the trader's own side.

Hiding is a detection game, and Frey and Sandås (2009) score it. On Xetra an iceberg
is detectable: after a peak executes, a same-priced peak reappears at the back of the
queue, and peak size plus the number of executed peaks forecast the hidden volume.
Icebergs are about 9% of submitted non-marketable orders and 16% of executed shares,
and are 12–20 times larger than ordinary limit orders. Detected icebergs *attract*
market orders rather than repelling them, and the greater the fraction already
executed, the *smaller* the price impact — consistent with the hidden liquidity being
uninformed.

Theory gives the same "show less when you are big" conclusion. Buti and Rindi (2009)
let each trader choose price, quantity *and* exposure and find hidden orders are
equilibrium strategies: early on, traders use them to prevent undercutting and choose
the largest displayed size that stops it; once fast traders and **picking-off risk**
are added, hidden orders are optimal at both nodes, placed at the second level of the
book, hiding the largest possible amount. Hidden-order use increases with order size
and with relative depth on the opposite side, and decreases with time-to-shock. Moinas
(2010) reaches a parallel conclusion comparing opaque and transparent books: informed
and large uninformed suppliers both hide, because displaying a large order drives away
the uninformed counterparty a transparent book would have attracted, so pre-trade
opacity raises liquidity and welfare.

Modern data complicates this. Chakrabarty, Hendershott, Nawn and Pascual (2019) find
high-frequency traders use hidden limit orders heavily but *not* as theory predicts:
their hidden orders are small, placed aggressively near the best quotes, and carry
the *lowest* information content of any trader-type/order-type combination. Neither
the free-option theory nor the information-revelation theory explains this, so the
authors validate two other conjectures — that HFTs use the non-display flag to
undercut standing quotes without announcing themselves, and to position ahead of
short-term volatility peaks. Roughly 30% of the Nasdaq book near the best quotes is
hidden on low-volatility days and 50% on high.

## Two shades of opacity

Hidden orders and dark venues are different products. Degryse, Tombeur and Wuyts
(2015), on millisecond data for Dutch large-caps, find hidden-order volume share is
higher on high-volume days while dark-venue share is *lower* (more volume means more
immediacy demand, so lit venues win and hidden execution probability rises); volatility
moves neither but cuts block trading; and hidden-order trading decreases in visible
depth, because visible depth has priority, while dark trading is unaffected. The two
are **substitutes**, but dark is the better substitute for hidden than the reverse.

Zhu (2014) asks what a dark venue does to price discovery. Informed orders are
directionally aligned, so they cluster on the heavy side and face a *lower* fill
probability in a dark pool than uncorrelated liquidity orders — a winner's curse of
fills. There is a unique volatility threshold $\bar\sigma$: below it informed traders
stay on the exchange with a shifted mix, above it they avoid the dark pool entirely.
Adding a dark pool strictly reduces the exchange participation rate of liquidity
traders and the total mass of informed traders and raises the exchange spread, yet by
concentrating price-relevant information on the exchange it *improves* price
discovery — the opposite of the regulatory intuition, resting on liquidity traders
choosing venues endogenously.

Foucault, Kadan and Kandel (2005) read the book as a market for liquidity: patient
traders post and impatient traders cross, sorted by a waiting cost, and with
heterogeneous patience there is a cutoff spread $s_c\in(j_p,K)$ above which all
traders post. The expected wait rises with the spread a limit order creates,
$T^{*}(j)>1/\pi$, because a wider spread is lower priority.

## Leakage and predatory trading

An exposed order is not merely visible; it is exploitable. Brunnermeier and Pedersen
(2005) let some traders face distress and a liquidation deadline $T$ while predators
face none, with price $p(t)=v+\lambda\int_0^t\sum_i a^i(u)\,du$ plus temporary impact.
Their Proposition 1 gives the single-predator equilibrium: each distressed trader
sells at constant speed $A/I$ for $\tau=x(t_0)/(A/I)$ periods, while the predator
sells as fast as possible without temporary impact for the same $\tau$ and then buys
back. The price **overshoots**, and the distressed trader's excess liquidation cost
equals the predator's profit. The market is least liquid exactly when liquidity is
most needed, so disclosure of positions advertises where the price impact will be.

Order flow's information content is measurable in real time. Easley, López de Prado
and O'Hara (2012) define **VPIN**, the volume-synchronized probability of informed
trading, splitting the day into equal-volume buckets of size $V$ indexed
$\tau=1,\dots,n$, with $V_\tau^{B},V_\tau^{S}$ the buy and sell volume in bucket $\tau$
under bulk volume classification:

$$\text{VPIN}=\frac{\alpha\mu}{\alpha\mu+2\varepsilon}\;\approx\;\frac{\sum_{\tau=1}^{n}|V_\tau^{S}-V_\tau^{B}|}{n\,V},$$

updated in volume time by dropping the oldest bucket, with $V$ one-fiftieth of average
daily volume and $n=50$ in the baseline. On E-mini S&P 500 futures, CDF(VPIN) crossed
0.9 at least two hours before the May 6, 2010 flash crash. VPIN tells a resting trader
the flow has turned toxic.

Hiding strategies leave footprints. Stenfors and Susai (2021) study 1,419,630 limit
orders in EUR/USD on EBS and define a **split order** as one whose price matches
another same-direction order submitted within 0.1 seconds with no other order or
cancellation in between — a threshold no human can meet. About 17.42% of orders are
split. Split orders are more aggressive than non-split ones, so splitting also improves
price rather than only hiding size; but for a given size they trigger *more* liquidity
withdrawal and are perceived as more market-moving. In the short run, splitting
backfires on concealment. Algorithms react first to opposite-side (free-option) risk;
once humans react, same-side non-execution risk dominates for the largest orders.

## What a trader holding private information should do

The literature supports concrete practices, and is silent on others.

- **Choose the split from the outage quantile.** Post
  $L^{*}=F^{-1}\!\big(\frac{2h+f+r}{\lambda_u+h+r+\theta}\big)-Q$ and cross the rest:
  raising $\lambda_u$ shifts size to market orders, a bigger queue $Q$ shrinks the
  limit slice, and across venues shortfall probabilities should be equalised.
- **Post at the touch for its queue value.** Moallemi–Yuan price the front of the
  queue at 0.21–0.26 ticks on a large-tick stock, near a whole spread, so placement is
  a queue-priority decision.
- **Scale displayed size down as the order grows.** Cebiroğlu–Horst and
  Buti–Rindi/Moinas both say hide more when large, with the largest gains (up to 60%)
  for big orders whose imbalance favours the trader.
- **Assume hiding is detectable.** Frey–Sandås find icebergs by their refresh
  pattern, after which they attract market orders; Chakrabarty et al. show the
  non-display flag is itself gamed.
- **Do not treat splitting as free concealment.** Stenfors–Susai measure split orders
  as *more* market-moving than equivalent human orders. Splitting plausibly helps only
  with randomised timing, sizes and venue choice — but the joint optimisation of
  participation rate, randomisation and sub-order granularity against a detection model
  is **not directly answered by the literature surveyed here**.
- **Use dark and hidden liquidity as substitutes, not duplicates.** Degryse et al.
  find they respond to different conditions and that dark is the better substitute for
  hidden, so route to whichever fits the current regime.
- **Model the predator, not just the book.** Brunnermeier–Pedersen say an observable
  liquidation need is front-run, so conceal direction and size and avoid a forced
  footprint; the size of that benefit against urgency is **not directly answered by the
  literature surveyed here**.
- **Watch flow toxicity.** VPIN is computable in volume time and led the flash crash
  by at least two hours, so quote placement and display size can be conditioned on it.

Open questions the literature does not settle: how the optimal iceberg display size
interacts with a concurrently available dark venue, and whether the 60% iceberg saving
survives netting against the time-priority and adverse-selection costs Frey–Sandås
measure behind an iceberg. Esser and Mönch (2007), Cartea and Jaimungal (2015) and
Yang and Zhu (2020) were not obtained for this chapter and are not relied on.

## Sources

- Cont, R., Kukanov, A. (2017). *Optimal order placement in limit order markets.* Quantitative Finance 17(1), 21–39. [arXiv:1210.1625](https://arxiv.org/abs/1210.1625)
- Moallemi, C. C., Yuan, K. (2016). *A model for queue position valuation in a limit order book.* Columbia Business School.
- Cebiroğlu, G., Horst, U. (2011). *Optimal display of iceberg orders.* SFB 649 Discussion Paper 2011-057.
- Frey, S., Sandås, P. (2009). *The impact of iceberg orders in limit order books.* CFR Working Paper 09-06.
- Buti, S., Rindi, B. (2009). *Hidden orders and optimal submission strategies in a dynamic limit order market.*
- Moinas, S. (2010). *Hidden limit orders and liquidity in order driven markets.*
- Chakrabarty, B., Hendershott, T., Nawn, S., Pascual, R. (2019). *Order exposure in high frequency markets.*
- Degryse, H., Tombeur, G., Wuyts, G. (2015). *Two shades of opacity: hidden orders versus dark trading.*
- Foucault, T., Kadan, O., Kandel, E. (2005). *Limit order book as a market for liquidity.* Review of Financial Studies 18(4), 1171–1217.
- Brunnermeier, M. K., Pedersen, L. H. (2005). *Predatory trading.* Journal of Finance 60(4), 1825–1863.
- Easley, D., López de Prado, M. M., O'Hara, M. (2012). *Flow toxicity and liquidity in a high-frequency world.* Review of Financial Studies 25(5), 1457–1493.
- Zhu, H. (2014). *Do dark pools harm price discovery?* Review of Financial Studies 27(3), 747–789.
- Stenfors, A., Susai, M. (2021). *Stealth trading in FX markets.* PBSEconFin 2021-02.

## Key points

- A market order pays half-spread plus fee plus impact for immediacy; a limit order earns half-spread plus effective rebate but bears non-fill and adverse-selection risk; icebergs and hidden orders give up time priority to conceal size.
- Cont–Kukanov's optimal single-venue split, $L^{*}=F^{-1}\!\big(\frac{2h+f+r}{\lambda_u+h+r+\theta}\big)-Q$, $M^{*}=S-L^{*}$, is a quantile of the queue-outflow distribution, so the optimum depends on the whole distribution, and limit-order capacity is bounded.
- Across venues the optimum equalises shortfall probabilities and deliberately overbooks, explaining "phantom liquidity" as risk diversification rather than manipulation.
- Queue position has measurable value — a resting order is worth fill probability times premium minus adverse-selection cost — and the front of the queue was worth 0.21–0.26 ticks more than an average position on BAC.
- The optimal display size equates the exposure impact of being seen with the priority benefit of being visible; detected icebergs attract market orders, and the more of an iceberg already executed the smaller its price impact.
- Hiding is not free: split orders are detected and perceived as more market-moving than equivalent human orders, and HFT hidden orders are small, near-touch and informationally empty rather than informed.
- Dark venues and hidden orders are substitutes responding differently to volume, depth and spread; adding a dark pool can raise the exchange spread while still improving price discovery.
- An observable liquidation need invites predatory front-running and price overshoot, so concealing direction and size is rational, and order-flow toxicity is measurable in real time through VPIN.
