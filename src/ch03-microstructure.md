# Market microstructure: where execution cost comes from

Place an order and something has already been paid before any opinion about
value is formed. This chapter answers why. Execution cost is not a fee
attached to trading; it is the price of the services a trade consumes and of
the information a trade reveals. Four mechanisms produce it: the spread, the
impact the order has on the price, the risk that the price moves while the
order waits, and the explicit charges of venues and brokers. The equilibrium
models below show that the first two are objects of equilibrium — they depend
on who else is trading — not constants a broker invents. Harris's is the
operational definition: **liquidity** is the ability to trade when you want to
trade, and Hasbrouck decomposes it into **immediacy**, **tightness**, **depth**
and **resiliency** — speed, a cheap round trip, size near the quote, price
effects that die out. Harris also fixes the accounting: trading is zero-sum
before costs and negative-sum after them, so a manager whose analysis is merely
uninformative loses on average simply by trading.

## Four sources of execution cost

**Bid–ask spread.** The best sell price exceeds the best buy price; a trader
who needs immediacy pays the difference to whoever is willing to wait on the
other side. Wyart, Bouchaud, Farmer and Lillo (2008) call it "the cost of an
instantaneous round-trip of one share", and their survey of the literature
splits the spread into three costs borne by a liquidity provider: order
processing, adverse selection, and inventory risk. All three produce it; none
is a fee.

**Market impact.** A large order consumes the liquidity resting at the best
prices and then walks up (or down) the book. Part of the resulting price
movement is informational and stays in the price; part is the mechanical
exhaustion of liquidity and fades. This is the object of Sections 3 and 4.
It is the dominant cost of institutional orders and the one every model in
Chapters 5–6 is built to control.

**Timing risk.** An order cannot be filled at the price seen when the decision
was made; between decision and completion the price diffuses. Ho and Stoll
(1981) trace this to two independent uncertainties the dealer bears: return
uncertainty on his inventory and transaction uncertainty about when the next
order arrives, so he does not know how long he must hold a position. For the
same reason Almgren and Chriss write the variance of execution cost as
$V(x) = \sigma^2 \sum_k \tau x_k^2$: whatever is left unexecuted is exposed.
Timing risk is the cost of not trading, and it trades off directly against
impact.

**Explicit fees.** Exchange access fees, rebates, broker commissions, taxes
and clearing. These are the only component that is a genuine transfer with a
posted price. In the models below they are a constant added to the impact
term — Almgren and Chriss carry them as $\epsilon$ in
$h(n_k/\tau) = \epsilon\,\mathrm{sgn}(n_k) + (\eta/\tau)n_k$ — and they are
the smallest component for large orders.

The distinction that matters for execution is not the list but the addressee:
the spread and the mechanical part of impact pay liquidity providers; the
permanent part of impact pays whoever knows more; timing risk is paid to
nobody and is pure loss.

Harris splits the participants by motive rather than by information.
**Utilitarian** traders trade to solve a non-trading problem — moving cash
through time, shedding risk, converting currency — and they accept transaction
costs to do so; that willingness to pay is the source of every profit-motivated
trader's revenue. The profit-motivated side splits further: informed traders
(value and news traders) trade on fundamental value, parasitic traders on
predictions of other traders' orders, and **liquidity suppliers** — dealers, who
connect buyers and sellers arriving at different times, and arbitrageurs, who
connect them across markets. Whether an order takes or makes is a trade-off, the
**make/take choice**: taking costs the spread and is certain, posting is cheaper
but may never execute. Books rank the competition by price, then time, with
displayed orders ahead of hidden ones, and Harris's three needs for speed,
**taking**, **making** and **canceling**, make liquidity supply a race in which
only comparative speed has value.

## Inventory: the dealer's problem

Ho and Stoll (1981) model a single risk-averse dealer who posts a buying fee
$b$ and a selling fee $a$ (so bid $=p-b$, ask $=p+a$ at his own valuation
$p$) and maximizes expected utility of terminal wealth $E[U(W_T)]$. Orders
arrive as Poisson processes with intensities that fall as the dealer raises
his fee, so a wider quote trades less often. The problem is a dynamic
program whose recurrence is a Bellman equation with two jump terms, one for a
sale at intensity $\lambda_a$ and one for a purchase at intensity $\lambda_b$:

$$\max_{a,b}\ \Big\{\, \lambda_a\big(J(F+pQ+aQ,\ I-pQ,\ Y) - J(F,I,Y)\big) + \lambda_b\big(J(F-pQ+bQ,\ I+pQ,\ Y)-J(F,I,Y)\big)\Big\} = 0 .$$

The paper describes its pricing strategy through the spread $s=a+b$ and an
adjustment variable $d=a-b$. Its two structural results are that the spread
is largely invariant to inventory, and that inventory moves the *midpoint*:
setting $d\ne0$ shifts both quotes in the same direction, so a dealer long of
stock lowers both bid and ask. The spread itself is written as the risk-neutral
revenue-maximizing spread $\rho$ — larger the more inelastic the demand for
immediacy — plus a sale risk premium and a purchase risk premium. The first-order
risk adjustment is independent of inventory and grows with three things: the
coefficient of absolute risk aversion $-U''/U'$, the transaction size $Q$, and
the instantaneous variance of the stock. It is *total* variance, not
systematic variance, because the dealer is undiversified with respect to the
quote. That is the inventory theory of the spread: the cost of immediacy is
the compensation for holding risk the customer does not want to hold.

*(Eq. 19, 24 and 25 of Ho–Stoll are multi-line display equations that did not
survive OCR of the scanned original; they are described in words above rather
than transcribed.)*

Roll (1984) turns this into a measurement. If the fundamental price is a
random walk and trades bounce between bid and ask, the first-order serial
covariance of observed price changes is exactly minus a quarter of the squared
effective spread, with variance $s^2/2$ and autocorrelation $-1/2$:

$$\operatorname{Cov}(\Delta p_t, \Delta p_{t-1}) = -\tfrac14 s^2, \qquad s = 2\sqrt{-\operatorname{Cov}(\Delta p_t,\Delta p_{t-1})}.$$

Using percentage returns and a factor of 200 to express the answer in percent,
$s_{jt} = 200\sqrt{-\hat c_{jt}}$. On CRSP data for 1963–1982 the implicit
spread across all stocks averaged 0.298 percent from daily returns and 1.74
percent from weekly returns, and was strongly negatively related to firm size.
The estimator is the ancestor of every method that recovers execution cost
from the tape, and its failure mode — a positive covariance, which makes it
undefined — is exactly what New York Stock Exchange specialist rents look
like to Wyart et al.

Hasbrouck's cost framework recovers two further spreads from execution data.
With $m$ the quote midpoint at the trade and $m_5$ the midpoint five minutes
later, the **effective cost** of a marketable buy is $p-m$ and the **realized
cost** is $p-m_5$; the effective spread is twice the former, and when $p$ beats
the near-side quote the difference is **price improvement**. The gap between the
two measures how far the midpoint travelled after the trade — an approximate
measure of the order's price impact, shared with every other trade in the
interval — and so separates the liquidity provider's compensation (the realized
spread) from the informed order's information. It is the empirical form of the
permanent/temporary split below, and it carries two qualifications from the
sources themselves. O'Hara (2015) notes that it is time-frame-dependent: in a
fast electronic market realized spreads are often *negative*, five minutes is a
"lifetime", and permanent and transitory are themselves suspect because nothing
fixes the horizon that defines transitory. Harris adds that under maker-taker
pricing the quoted spread is no longer the traded cost: a taker's true spread is
the quote plus twice the access fee, a maker's the quote minus twice the rebate.

Amihud and Mendelson (1986) place the spread in equilibrium asset pricing:
with investors of differing holding periods, market-observed expected return
should be an increasing and concave function of the relative spread. Illiquidity
is therefore not merely a cost of trading; it is priced, and investors who hold
longer can afford to bear more of it. *(This paper was paywalled and was not
read in full; only its published abstract is available.)*

## Information asymmetry: the spread as a Bayesian inference

Ho–Stoll's dealer loses to no one on average. Glosten and Milgrom (1985) show
that a spread exists even when the specialist is risk neutral, competitive, and
forced to earn exactly zero expected profit. A customer who agrees to trade at
the specialist's ask may be doing so *because he knows something*. The
specialist cannot tell insiders from liquidity traders, so he quotes prices that
break even conditional on the trade that actually occurs:

$$A_t = E[V \mid S_t,\ \text{customer buys}], \qquad B_t = E[V \mid S_t,\ \text{customer sells}].$$

**Proposition 1** then gives $A \ge E[V] \ge B$, strictly whenever adverse
selection is possible. The proof is a conditional-expectation argument —
$A = E[V\mid C]$ for the buying event $C$, and $E[V\mid F]$ is a coarser
expectation than $E[V\mid F,C]$ — and its content is that the spread is a
pure information rent. The specialist is not charging for a service; he is
protecting himself against a customer he cannot classify.

Three further results are what make the model usable. **Proposition 2**: the
transaction prices $P_k = E[V\mid S_k]$ are a martingale, so prices are
semi-strong efficient even though a spread exists. **Proposition 3**: if buy
and sell arrival probabilities are bounded away from zero, the root-mean-square
spread over the first $k$ trades vanishes on the order of $1/\sqrt{k}$,

$$\sqrt{\tfrac1k E[(A_k-B_k)^2]} \le \sqrt{\operatorname{Var}(V)/(\alpha\beta k)} .$$

**Proposition 5**: the spread widens when insiders' information is finer, when
the informed/uninformed arrival ratio rises, or when the dispersion of the
liquidity motive falls. That last clause is Akerlof's lemons result in an
order book: squeeze out enough liquidity traders and the market can close.

Glosten–Milgrom leave trading intensity exogenous. Kyle (1985) makes it
strategic. One risk-neutral insider observes the liquidation value
$v \sim N(\mu_0,\Sigma_0)$; noise traders submit $u \sim N(0,\sigma_u^2)$
independent of $v$; competitive risk-neutral market makers see only the
aggregate order flow $x+u$ and set $p = E[v \mid x+u]$. The insider maximizes
expected profit knowing his order moves the price. In the single-auction
equilibrium both strategies are linear (Theorem 1):

$$X(v) = \beta(v-\mu_0), \qquad P(x+u) = \mu_0 + \lambda(x+u),$$

$$\beta = \Big(\frac{\sigma_u^2}{\Sigma_0}\Big)^{1/2}, \qquad \lambda = \frac12\Big(\frac{\Sigma_0}{\sigma_u^2}\Big)^{1/2},$$

obtained from the insider's first-order condition $1/\beta = 2\lambda$ and the
projection theorem $\lambda = \beta\Sigma_0/(\beta^2\Sigma_0+\sigma_u^2)$. The
name **Kyle's lambda** is $\lambda$: the slope of price on order flow. Its
reciprocal $1/\lambda$ is market **depth** — the order flow needed to move the
price by one unit — and it is proportional to the ratio of noise trading to
unresolved private information. After one auction only half the private
information is in the price, $\Sigma_1 = \tfrac12\Sigma_0$; the insider's
maximized profit is $\Sigma_0/(4\lambda) = \tfrac12\sqrt{\Sigma_0\sigma_u^2}$.

The sequential version (Theorem 2) makes the same statement trade by trade,
$p_n - p_{n-1} = \lambda_n(\Delta x_n + \Delta u_n)$, and in the continuous
limit (Theorem 3) it collapses to a constant that is worth remembering:

$$\lambda(t) = \Big(\frac{\Sigma_0}{\sigma_u^2}\Big)^{1/2}, \qquad \Sigma(t) = (1-t)\,\Sigma_0 .$$

Depth is constant, prices are Brownian motion with constant volatility, and all
private information is in the price by the close of trading. Kyle's lambda is
the canonical linear price-impact coefficient, and it is the reason the
permanent-impact term $g(\cdot)$ in Chapter 5 has the form it does.

## Permanent and temporary impact

The two mechanisms above give two different impacts, and their separation is
the foundation of every execution model that follows. A trade that reveals
information moves the price to a new level and that level is real: the
information has been released and cannot be recalled. A trade that consumes
liquidity moves the price because the book was thin, and the book refills.
The first is **permanent impact** — a public-information event, identified with
$\lambda$ times the net order flow. The second is **temporary impact** — a
rent to whoever supplied liquidity, which decays.

Guéant's lectures give this split its general form, and the form says where the
modelling freedom lies. Let $v_t$ be the trading rate and $V_t$ the market
volume, so the **participation rate** is $\rho = v_t/V_t$. The midprice obeys

$$dS_t = \sigma\,dW_t - k\,v_t\,dt,$$

in which the $k$ term is the permanent impact, while the cash paid for
execution is $V_t\,L(v_t/V_t)\,dt$, with $L$ the **instantaneous impact
function**: strictly convex, even, increasing on $\mathbb R_+$, with $L(0)=0$,
asymptotically super-linear, and in practice $L(\rho) = \eta|\rho|^{1+\varphi} + \psi|\rho|$.
Terminal wealth over a liquidation of $q_0$ shares is

$$X_T = X_0 + q_0 S_0 - \tfrac{k}{2}q_0^2 + \sigma\int_0^T q_t\,dW_t - \int_0^T V_t\,L(v_t/V_t)\,dt .$$

The permanent-impact cost $\tfrac{k}{2}q_0^2$ is independent of the strategy —
the same whether the order is worked slowly or crossed at once — so $k$ is a
change of variables and the freedom lies in the shape of $L$ and in the risk
term. This is the formal version of the claim that only the temporary part can
be optimized away.

Bouchaud (2010) makes the empirical point sharply. If impact were permanent
and linear, prices would inherit the autocorrelation of the trade signs, and
because $\epsilon_n$ decays only slowly — over at least several days, thousands
of trades — prices would trend, contradicting their near-martingale behaviour.
Impact must therefore be transient, carried by a decaying kernel $G(\ell)$:

$$p_T = p_{-\infty} + \lambda\sum_{n=-\infty}^{N-1} G(N-n)\,\epsilon_n\, v_n^{\psi}.$$

With fixed trade sizes, if the sign correlation $C(\ell)\sim \ell^{-\gamma}$
(taking $\gamma\approx0.5$ for stocks), then $G(\ell)$ must decay as
$\ell^{-\beta}$ with $\beta = (1-\gamma)/2$ for price changes to stay white.
Gatheral's later condition $\beta+\psi\ge1$ is what rules out price-manipulation
strategies in this framework.

What does the impact look like as a function of size? The **square-root law**.
Tóth, Lempérière, Deremble, de Lataillade, Kockelkoren and Bouchaud (2011)
state it as the average relative price change between the first and last trade
of a metaorder of size $Q$:

$$\Delta(Q) = Y\sigma\sqrt{\frac{Q}{V}},$$

where $\sigma$ is daily volatility, $V$ daily volume, and $Y = O(1)$; more
generally $\Delta(Q)\propto Q^\delta$ with $\delta$ in the range 0.4–0.7. Their
reading of the data (Almgren et al. report $\delta\approx0.6$; Moro et al.
report $\delta\approx0.5$ on Madrid and $0.7$ on London) is that the law holds
approximately across equities, futures and FX, and across epochs. Their
mechanism is a latent order book whose supply/demand profile is V-shaped and
vanishes at the current price — liquidity is critical, so the linear-response
argument breaks down and a square root emerges.

The law is not the last word. Zarinelli, Treccani, Farmer and Lillo (2015),
using several million US equity metaorders from 2007–2009, find that the
square-root form fits well across only about two orders of magnitude in size,
with best-fit $Y = 0.15 \pm 0.01$ and $\delta = 0.47 \pm 0.01$ and a weighted
root-mean-square error of 6.70. A logarithmic form $a\log_{10}(1+b\pi)$ in the
daily participation rate $\pi$ fits across almost five orders of magnitude, with
$\hat a = 0.028 \pm 0.001$, $\hat b = 465 \pm 33$, and an error of 2.80 —
"quite dramatically better". They also introduce an impact *surface* in the
duration and participation rate of the metaorder, and find that on average the
impact relaxes to about **two-thirds of its peak**. Cont, Kukanov and Stoikov
(2014) supply a complementary decomposition: over short intervals the price
change is driven mainly by order flow *imbalance* at the best quotes, with a
slope inversely proportional to market depth, and a scaling argument turns that
linear relation into the observed square root.

Only the transient picture explains why impact is not permanent while order
flow is persistent. Bouchaud, Farmer and Lillo (2009) review the evidence:
order-sign autocorrelation is a long-memory process that decays as a power law
with exponent below one, revealed liquidity is tiny (the volume at the best
quotes is roughly $(w/u^*)$-smaller than the volume resting one tick away, and
about a thousand times smaller than daily turnover), so a large order must be
split over hours, days or weeks. Their aggregate-impact calculation gives

$$R(Q,N) = N\,\frac{s\Gamma_0}{I(1-\beta)}\left(\frac{Q}{N^{1-\gamma/2}}\right),$$

so impact is strongly concave for small metaorders and becomes linear for
large ones, and the spread itself reverts with a power-law exponent of 0.4–0.5.
This is the "resilience" of the market, and it is the reason temporary impact
is recoverable.

## Market microstructure invariance

Comparing execution costs across assets is otherwise hopeless: a share of a
USD 5 stock and a share of a USD 500 stock are not the same quantity of risk.
Kyle and Obizhaeva (2016) propose that the comparison becomes simple in
*business time* — time measured by the arrival of bets, where a bet is a
portfolio decision, not an order or a print. Their two hypotheses are
**invariance of bets** (the distribution of the dollar risk a bet transfers per
unit of business time is the same across assets and times) and **invariance of
transaction costs** (the dollar cost of executing a bet is the same function of
its size measured in those units). Writing bet activity as
$\bar W_{jt} = \sigma_{jt}P_{jt}\bar V_{jt}$ and bet size as
$\tilde I_{jt} = P_{jt}\tilde Q_{jt}\sigma_{jt}\gamma_{jt}^{-1/2}$, invariance of
bets yields $\bar W_{jt} = \gamma_{jt}^{3/2}E|\tilde I|$ and hence

$$\gamma_{jt} = \bar W_{jt}^{2/3}\,\iota^2, \qquad E|\tilde Q_{jt}| = \bar W_{jt}^{1/3}\frac{1}{P_{jt}\bar\sigma_{jt}}\iota^{-2}.$$

Number of bets scales as the two-thirds power of activity and average bet size
as the one-third power. Invariance of transaction costs gives an illiquidity
index that is the cube root of the ratio of returns variance to dollar volume,

$$\frac{1}{L_{jt}} = \iota^2\bar C_B\Big(\frac{\sigma_{jt}^2}{P_{jt}V_{jt}}\Big)^{1/3},$$

and a percentage spread proportional to volatility in business time,
$s_{jt}/P_{jt} = \kappa_0\sigma_{jt}\bar W_{jt}^{-1/3}$; holding volatility
fixed, an eightfold increase in activity halves the percentage spread. The
square root is the only impact function for which invariance predicts that
impact depends on the bet's size relative to bet volume and on nothing else:
$\bar C_{jt}(Q) = \bar\sigma_{jt}\kappa_I[|Q|/\bar V_{jt}]^{1/2}$. Kyle and
Obizhaeva test this on 400,000+ portfolio transition orders and find bet sizes
consistent with the first hypothesis (log-variance 2.53) and transaction costs
economically close to the second, though the tests usually reject them formally.

## Order flow as a signal

Everything above says the same thing from different angles: order flow carries
information, and that is why it costs. In Glosten–Milgrom the spread *is* the
adverse-selection rent; in Kyle the price change *is* the market's Bayesian
update. The size of that rent is measurable. Wyart et al. (2008) show that
imposing marginal profitability of both market-taking and liquidity-providing
strategies implies a linear relation between the spread and the instantaneous
impact of market orders, and then that the spread is proportional to volatility
*per trade* — empirically across Paris stocks with $R^2$ above 0.9. Their
conclusion is that the main determinant of the spread is adverse selection,
and that most of the volatility comes from trade impact. The implication for
execution is immediate: a strategy whose order flow is predictable pays less
than one whose flow is a surprise, because only the surprise moves the price.

Hasbrouck calls the process by which bids, asks and trades converge on a new
value **price discovery**, and the word is chosen because the outcome is
unknown: everyone may agree the news is good and no one knows what it is worth,
so the market can only aggregate heterogeneous beliefs. What makes the
aggregation expensive is the size of order impact relative to the order: a
marketable buy of 1,000 shares in a firm with ten million shares outstanding can
move the price by a cent and so revalue the firm by five times the order's own
worth. The book is not simply refreshed at the old prices because the fact of
the trade, and its direction, have entered the public information set. O'Hara
(2015) sharpens this for the electronic market: there the basic unit of market
information is not the trade but the underlying order. An algorithm turns one
parent order into dozens of child orders, so a parent buy appears on the tape as
many small sells, and speed itself becomes a kind of information — the trader
who sees and acts on a price first is, briefly, an informed trader.

This is also the reason a well-informed trader's cost structure differs from
everyone else's. To an uninformed execution algorithm, its own impact is a cost
paid to liquidity providers to be minimized. To an informed trader, the
permanent part of the impact is not a cost at all; it is the price moving *toward*
the value he is buying, and it is realized on the inventory the trade
accumulates. The two traders face the same lambda and should behave
differently in front of it. Chapter 8 returns to this when it treats information
leakage, and Chapter 2's market-maker takes the other side of it.

## The cost map

Reading the mechanisms together gives a simple ledger of who pays whom.

| Mechanism | Paid by | Received by | Recovers? |
|---|---|---|---|
| Spread | Whoever demands immediacy | Whoever supplies it (processing, adverse selection, inventory) | Paid on entry, returned on exit only as a quote |
| Temporary impact | The aggressor | Liquidity providers who absorbed the order | Yes — decays as $G(\ell)$, fully or partly |
| Permanent impact | The aggressor | Holders of the opposite position — the informed | No — the information is in the price |
| Timing risk | The unexecuted remainder | Nobody (pure risk, not a transfer) | No |
| Explicit fees | The trader | Venue, broker, state | No |

The row that reorganizes execution theory is permanent versus temporary. Both
lines look identical in a cost report written after the trade, and both are
impact. But the temporary part is a rent an algorithm negotiates against by
scheduling, whereas the permanent part is the market discovering what the order
meant. Only the first can be optimized away by trading more slowly.

## Sources

- Ho, T. and Stoll, H. (1981). *Optimal Dealer Pricing under Transactions and
  Return Uncertainty*, Journal of Financial Economics 9(1), 47–73.
  [Wharton working paper](https://rodneywhitecenter.wharton.upenn.edu/wp-content/uploads/2014/03/7927.pdf)
- Roll, R. (1984). *A Simple Implicit Measure of the Effective Bid-Ask Spread
  in an Efficient Market*, Journal of Finance 39(4), 1127–1139.
  [pdf](https://www.bauer.uh.edu/rsusmel/phd/roll1984.pdf)
- Glosten, L. and Milgrom, P. (1985). *Bid, Ask and Transaction Prices in a
  Specialist Market with Heterogeneously Informed Traders*, Journal of
  Financial Economics 14(1), 71–100 (working-paper version, Northwestern
  CMSEMS Discussion Paper 570, 1983).
  [pdf](https://www.econstor.eu/bitstream/10419/220930/1/cmsems-dp0570.pdf)
- Kyle, A. S. (1985). *Continuous Auctions and Insider Trading*, Econometrica
  53(6), 1315–1336.
  [pdf](https://people.duke.edu/~qc2/BA532/1985%20EMA%20Kyle.pdf)
- Amihud, Y. and Mendelson, H. (1986). *Asset Pricing and the Bid-Ask Spread*,
  Journal of Financial Economics 17(2), 223–249 (not obtained in full).
- Madhavan, A. (2000). *Market Microstructure: A Survey*, Journal of Financial
  Markets 3(3), 205–258.
- Wyart, M., Bouchaud, J.-P., Farmer, J. D. and Lillo, F. (2008). *Relation
  between Bid-Ask Spread, Impact and Volatility in Order-Driven Markets*.
- Bouchaud, J.-P., Farmer, J. D. and Lillo, F. (2009). *How Markets Slowly
  Digest Changes in Supply and Demand*, in Handbook of Financial Markets.
  [arXiv:0809.0822](https://arxiv.org/abs/0809.0822)
- Bouchaud, J.-P. (2010). *Price Impact*, Encyclopedia of Quantitative Finance.
  [arXiv:0903.2428](https://arxiv.org/abs/0903.2428)
- Tóth, B., Lempérière, Y., Deremble, C., de Lataillade, J., Kockelkoren, J.
  and Bouchaud, J.-P. (2011). *Anomalous Price Impact and the Critical Nature
  of Liquidity in Financial Markets*.
  [arXiv:1105.1694](https://arxiv.org/abs/1105.1694)
- Cont, R., Kukanov, A. and Stoikov, S. (2014). *The Price Impact of Order
  Book Events*.
  [arXiv:1011.6402](https://arxiv.org/abs/1011.6402)
- Zarinelli, E., Treccani, M., Farmer, J. D. and Lillo, F. (2015). *Beyond the
  Square Root: Evidence for Logarithmic Dependence of Market Impact on Size
  and Execution Kinetics*.
  [arXiv:1412.2152](https://arxiv.org/abs/1412.2152)
- Kyle, A. S. and Obizhaeva, A. A. (2016). *Market Microstructure Invariance:
  Empirical Hypotheses*, Econometrica 84(4), 1345–1404.
- Hasbrouck, J. (2025). *Securities Trading: Principles and Procedures*, v14c,
  NYU Stern teaching text (successor volume to *Empirical Market
  Microstructure*).
  [author's page](https://pages.stern.nyu.edu/~jhasbrou/)
- O'Hara, M. (2015). *High Frequency Market Microstructure*, Journal of
  Financial Economics 116(2), 257–270.
- Harris, L. (2015). *Trading and Electronic Markets: What Investment
  Professionals Need to Know*, CFA Institute Research Foundation.
  [pdf](https://www.cfainstitute.org/-/media/documents/book/rf-publication/2015/rf-v2015-n4-1-pdf.pdf)
- Guéant, O. (2016). *The Mathematics of Optimal Execution*, CFM–Imperial
  Distinguished Lectures.
  [pdf](https://www.ogueant.com/doc/imperial_slides.pdf)

Note on sources not obtained: the bodies of O'Hara's *Market Microstructure
Theory* (1995), Hasbrouck's *Empirical Market Microstructure* (2007) and
Harris's *Trading and Exchanges* (2003), and the printed text of Guéant's *The
Financial Mathematics of Market Liquidity* (2016), were not obtained, and
nothing in this chapter comes from them. It rests instead on releases by the
authors and rights holders: Hasbrouck's NYU Stern teaching text *Securities
Trading: Principles and Procedures* (v14c, 2025), the successor volume to his
book, for the liquidity attributes, order-book priority, cost decomposition and
private-information analysis; O'Hara's 2015 survey for the high-frequency
reassessment of the spread decomposition and the order-as-information-unit
argument; Harris's *Trading and Electronic Markets* (CFA Institute Research
Foundation, 2015), a free full volume by the book's author, for the trader
taxonomy, the zero-sum accounting, the three needs for speed and the maker-taker
caveat; and Guéant's CFM–Imperial lectures (2016), which cover his book's parts
I–II, for the general permanent/temporary impact framework. Still missing:
O'Hara's nine chapters, Hasbrouck's 2007 econometric chapters 3–13, Harris's
2003 chapters 3–29, and Guéant's printed body.

## Key points

- Execution cost has four sources — the spread, market impact, timing risk and
  explicit fees — and only the last is a posted transfer; the first three are
  equilibrium objects that depend on who else is trading.
- Ho–Stoll show that inventory risk puts a spread into the dealer's quotes:
  the spread is essentially inventory-independent and splits into a risk-neutral
  revenue-maximizing term plus risk premia that grow with risk aversion,
  trade size and total volatility, while inventory shifts the midpoint.
- Roll's estimator, $s = 2\sqrt{-\operatorname{Cov}(\Delta p_t,\Delta p_{t-1})}$,
  recovers the effective spread from prices alone because trading costs induce
  negative serial dependence; it averaged 0.298 percent daily on CRSP
  1963–1982 and falls with firm size.
- Glosten–Milgrom prove that adverse selection alone creates a spread:
  zero-profit quotes $A = E[V\mid\text{buy}]$ and $B = E[V\mid\text{sell}]$
  satisfy $A \ge E[V] \ge B$, prices remain a martingale, and the spread
  vanishes as $1/\sqrt{k}$ over the first $k$ trades.
- Kyle's lambda, $\lambda = \frac12(\Sigma_0/\sigma_u^2)^{1/2}$ in the single
  auction and $(\Sigma_0/\sigma_u^2)^{1/2}$ in the continuous limit, is the
  slope of price on order flow; its reciprocal is market depth, and it is the
  equilibrium microfoundation of permanent impact.
- Permanent and temporary impact must be separated: the permanent part is the
  release of information and never returns, while the temporary part is a rent
  to liquidity providers that decays; impact therefore has a shape
  $\Delta \propto \sigma\sqrt{Q/V}$ (the square-root law) with $\delta$ between
  0.4 and 0.7, and Zarinelli et al. find a logarithmic form fits their data
  better than the square root.
- Kyle–Obizhaeva's invariance says bet size and transaction costs are constant
  across assets once measured per unit of business time, giving
  $\gamma \propto \bar W^{2/3}$ and an illiquidity index
  $\propto (\sigma^2/PV)^{1/3}$ — the tool for cross-market comparison.
- Order flow is a signal: the spread is adverse selection paid to information
  holders, so a predictable order pays less than a surprising one, and an
  informed trader's permanent impact is not a cost but the price moving toward
  value.
