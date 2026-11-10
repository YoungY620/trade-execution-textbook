# Order routing and market fragmentation

Chapter 8 chose an order type and a venue. Here the same instrument is quoted on
many venues at once, the best price on any one of them is not the best execution
available across them, and a rulebook both forces and constrains the choice.

## The fact of fragmentation

Reg NMS defines a **trading center** to include exchanges and associations operating
an SRO trading facility, alternative trading systems, exchange and OTC market
makers, and "any other broker or dealer that executes orders internally by trading
as principal or crossing orders as agent" (SEC 2005, Rule 600(b)(78)). All four trade
the same stock at once. NYSE and Nasdaq together held approximately 95% of U.S.
equity volume in 2000 and less than 30% by 2016, while the number of U.S. market
centers nearly tripled and fragmentation measures more than doubled (Haslag and
Ringgenberg 2023).

For September 2009 the SEC reports registered exchanges at approximately 63.8% of
NMS share volume, none above 19.4%; five active ECNs at 10.8%; approximately 32 dark
pools at 7.9%; and internalisation across more than 200 dealers at 17.5%. Displayed
centers were 74.6% and undisplayed 25.4% (SEC 2010, Figure 6). NYSE's share of volume
in its own listed stocks fell from approximately 79.1% in January 2005 to 25.1% in
October 2009, while volume rose 181% in shares and 662% in trades over the two
comparable January–October periods. By the mid-2010s the venue count had settled at
16 national exchanges, more than thirty ATSs and numerous broker-dealers (Mishra and
Zhao 2021); exchanges were approximately 62% of consolidated volume with none above
15%, ATSs mainly dark pools approximately 15%, and internalisers the residual 23%
(Comerton-Forde, Grégoire and Zhong 2017).

The SEC staff review distinguishes **visible fragmentation**, dispersal among lit
venues, from **dark fragmentation**, the lit–dark split; their empirical signatures
differ (SEC 2013).

## The Reg NMS constraint set

### Protected quotations and the NBBO

A **trade-through** is the purchase or sale of an NMS stock during regular trading
hours at a price lower than a **protected bid** or higher than a protected offer
(Rule 600(b)(77)). A quotation is protected only if it is an **automated quotation**
displayed by an **automated trading center** and is the best bid or offer of an
exchange, Nasdaq, or the association other than Nasdaq (Rule 600(b)(57)). Automated
means the venue accepts an immediate-or-cancel order, executes it immediately up to
full size, cancels any remainder without routing it elsewhere, responds to the
sender, and updates the quotation on any material change (Rule 600(b)(3)); any human
intervention after the order arrives disqualifies it, and it becomes a **manual
quotation**. Only one accessible best bid and offer per venue family is protected,
not the whole book. Protection therefore runs only to quotations that are *immediately
and automatically accessible*, which refuses protection to slow, manual markets and
forces them to automate or lose priority.

### The Order Protection Rule (Rule 611)

Rule 611 requires every trading center to maintain written policies and procedures
"reasonably designed to prevent the execution of trades at prices inferior to
protected quotations displayed by other trading centers," subject to exceptions
(SEC 2005). Its central exception is the **intermarket sweep order**: a limit order
marked as such and sent simultaneously with other sweep orders routed to execute
against all better-priced protected quotations up to their displayed size may be
executed immediately by the receiving venue without regard to better-priced quotes
elsewhere (Rule 600(b)(30)), on the premise that the router has already tried to
access everything better. Rule 611 is the legal foundation of smart order routing: a mandate to scan every
protected quotation before executing. It binds on *prices*, not net-of-fee prices, so
a venue offering a rebate at a worse price cannot be preferred on the strength of the
rebate. It answered a measured problem: an SEC staff study found 1 of every 40 trades
(2.5%) in both NYSE and Nasdaq stocks executing at a price inferior to the best
displayed price — approximately 98,000 trades per day in Nasdaq stocks alone — and
the Commission put implicit transaction costs at USD 30 billion to more than
USD 100 billion per year (SEC 2005).

### The Access Rule (Rule 610)

Rule 610 requires fair and non-discriminatory access to quotations, including indirect
access through members, subscribers or customers; caps the fee for accessing protected
quotations at **USD 0.003 per share** (0.3% of the quotation price below USD 1.00);
and requires SROs to prohibit a pattern or practice of displaying quotations that
**lock or cross** other automated quotations. The cap is tied to the Order Protection
Rule: without it, an outlier venue could exploit the obligation to protect displayed
quotations by charging exorbitant access fees to those forced to trade with it. The
Commission's aim is that routers can treat displayed prices as "true prices" within a
limited range — on a USD 10.00 protected offer the total cost to buy is at most
USD 10.003, so three-tenths of a cent is the entire fee-based wedge the router may
see. The lock/cross prohibition is asymmetric: a trading center may lock or cross the
*manual* quotation of another venue, so the rule encodes the speed disparity it
created and establishes that the first protected quotation at a price is entitled to
an execution at that price.

### The Sub-Penny Rule (Rule 612) and the side effects

Rule 612 sets a uniform minimum increment of one cent for stocks at or above
USD 1.00, prohibiting the acceptance, ranking or display of orders, quotations or
indications of interest in a smaller increment, to protect the displayed quotes of
traditional liquidity suppliers. Its routing consequence is that a rebate cannot be
passed through as a better price: USD 0.0030 of rebate cannot become a USD 9.9997
quote. Three side effects follow: protection restricted to automated quotations
rewards speed and made automation a condition of being quoted; the lock/cross
prohibition created a persistent operational problem, since rapid updates can leave a
venue locked; and because the penny grid blocks price adjustment, fee models rather
than prices became the axis of venue competition.

## Smart order routing: Foucault–Menkveld

Foucault and Menkveld (2008) ask whether splitting one limit order market into two
competing books helps or hurts liquidity, and what routing automation does to the
answer, extending Parlour and Seppi (2003). In period 1 risk-neutral limit-order
traders post depth on a price grid in an incumbent book $I$ and/or an entrant book
$E$. In period 2 a broker arrives with a market order and is either a **smart
router**, with probability $\theta$, who consolidates both books and splits
optimally, or a non-smart router, with probability $1-\theta$, who can trade only in
the incumbent. Price and time priority hold within each book but **not across** books,
so a trader can leapfrog the incumbent's queue by posting at the same price in the
entrant book, competing away infra-marginal rents.

The entrant is viable only if the smart-router share exceeds a threshold
$\theta^{*}(c_E,c_I,\sigma_1)$; below it the incumbent dominates, above it the markets
coexist, so routing technology is *necessary* for venue entry (Proposition 1). Even at
$\theta=1$ with different fees both markets can coexist, because a limit-order trader
trades execution probability off against the fee, so routing is not infinitely
fee-elastic (Proposition 2). Cumulative depth in the incumbent falls after entry, but
**consolidated depth is always larger** because time priority is enforced only locally
(Proposition 3).

The May 2004 entry of the LSE's EuroSETS against Euronext's NSC in 22 Dutch stocks
raised consolidated depth through the fourth tick by 46.3% and then 100.8% for the most
active stocks, with consolidated spreads smaller or unchanged. The estimated
smart-router share was small — 27%, then 18% — and EuroSETS's relative spread was
significantly smaller in stocks with a high smart-router share.
Routing automation is thus what makes competing venues simultaneously *viable* and
*benevolent*; low adoption, or tie-breaking favouring the incumbent, leaves the
entrant's better prices untouched. On the determinants side, Mishra and Zhao (2021)
find routing responding to market structure design, to cost against execution quality
and adverse selection (weighted differently by investor type, with informed traders
splitting orders to camouflage), to trader type, to market conditions, and to
regulation — concluding only that routing is complex and influenced by many factors,
which leaves the objective function unstated.

## How fees enter the routing decision

Exchange pricing determines what an identical quoted price means, so it is an argument
of the routing decision rather than a rounding error on it.

Under **maker–taker** pricing market and marketable limit orders pay a taker fee and
resting limit orders receive a maker payment. On Nasdaq's schedule as Hasbrouck (2025)
records it, an executed *visible* limit order received USD 0.0029 per share and a
*hidden* one USD 0.0015, against a USD 0.0030 liquidity removal fee paid by the taker;
Nasdaq kept USD 0.0001.

**Inverted** pricing reverses the sign: BATS-Y charges USD 0.0030 for providing
liquidity and pays USD 0.0020 for removing it. The trade-off is timing. A trader
lifting a USD 10.00 offer sends to BATS-Y; a trader posting a sell limit order chooses
between BATS-X, which pays a rebate but has 10,000 shares ahead of him, and BATS-Y,
which charges him but puts him at the front of a book takers prefer. A sell order at
USD 9.9997 to net out the fee is barred by Rule 612.

Here the access-fee cap and the penny grid meet. Comerton-Forde, Grégoire and Zhong
(2017) explain why inverted venues are nevertheless valuable: discrete ticks prevent
prices from neutralising rebates, so a venue can use its fee to create a **finer
pricing grid inside the spread**, offering sub-penny improvement relative to the NBBO.
Their shock is exogenous — the 2016 Tick Size Pilot widened the tick from USD 0.01 to
USD 0.05, and one pilot group additionally faced a **trade-at** rule forcing lit
trading rather than sub-penny dark execution — and it raised the market share of
inverted venues from **15% to 28%** for trade-at stocks. Higher inverted share improved
pricing efficiency, increased liquidity and decreased volatility, strongest for
tick-constrained stocks; increasing sub-penny *dark* trading did the opposite.

The cheapest execution path is therefore not the best price, and the wedge accrues to
venues and brokers rather than traders. O'Donoghue (2015) models a broker that charges
a commission and endogenously routes either to a dealer or to an exchange, with the
exchange keeping part of the taker fee and passing the rest to the broker as a maker
rebate. Holding the exchange's retained profit constant, as taker fee and maker rebate
rise together, the quoted spread **narrows** while total trading cost **rises**,
 **Rule 605**
makes the conflict visible, requiring market centers to publish monthly, by stock,
trade size and pricing relative to the same-side quote, order and share counts,
execution and cancellation rates, and effective and realized spreads; **Rule 606**
requires brokers to report what orders they received and their relationships with
market centers.

## Internalization and payment for order flow

Retail marketable orders largely never reach an exchange. An SEC Concept Release
review of Rule 606 disclosures for eight broker-dealers with significant retail
accounts found that "nearly 100% of their customer market orders are routed to OTC
market makers" (quoted in Hasbrouck 2025). The wholesaler sells directly to the
customer at the national best offer or buys at the national best bid, and pays the
broker for the order. The flow is valuable because a dealer loses to informed traders
and profits from uninformed ones, and retail traders as a group are less informed;
payment is the price of that segmentation.

The conflict bites hardest where the amounts are largest. From Rule 606 reports Ernst
and Spatt (2022) estimate the typical payment for routing a 100-share *options* trade
at 40 cents against around 20 cents for a 100-share *equity* trade. Because the
average retail stock trade is in a USD 25 stock and the average retail option trade in
a USD 5 option, a nominal USD 1,000 generates a 40-share equity order worth 8 cents in
PFOF and a 200-share options order worth 80 cents — **10 times as much** PFOF for the
same nominal investment. With zero commissions an options trade gives a market maker
100% more revenue than an equity trade. Separately, sub-penny price improvement
accrues to the customer rather than the broker: over January 2019 to October 2021 it
delivered between USD 20 and USD 30 million per month, roughly 10% of market maker
revenue on retail trades goes to providing it, and about half of the total occurs when
stocks are at the one-penny minimum spread. On the options side, exploiting variation
in Designated Market Maker assignments, PFOF-paying DMMs are causally associated with
wider spreads and less price improvement.

The SEC permits the practice but requires disclosure. The forward branch is the proposed
**Order Competition Rule** (Proposed Rule 615, SEC 2023), which would prohibit a
"restricted competition trading center" — any trading center other than an open
competition trading center or a national securities exchange — from internally
executing certain individual-investor orders at a price unless they are first exposed
to competition at that price in a qualified auction operated by an "open competition
trading center." It treats routing itself as a market-design variable. Hasbrouck
records that the rule drew mixed comments and was withdrawn in 2025; the SEC also
adopted tick-size and access-fee amendments in September 2024 (a USD 0.005 tick for
some stocks, an access-fee cap cut to USD 0.001 per share) that were partly stayed
pending judicial review.

## Fragmentation and market quality

Whether fragmentation helps or hurts depends on the stock and on the kind of
fragmentation. O'Hara and Ye (2011) is the canonical test that it is not inherently
harmful: measuring fragmentation by Trade Reporting Facility volumes, it finds more
fragmented stocks have lower transaction costs and faster execution, with higher
short-term volatility but more efficient prices. **This paper could not be obtained for
this chapter** — it is subscription-only — and the summary rests on the SEC staff
review and later papers citing it, which flag that the proxy mixes lit and dark volume
because in the sample period, the first six months of 2008, two lit venues executed
approximately 50% of off-exchange volume (SEC 2013).

Zhu (2014) supplies the mechanism for the dark case, and it cuts against the regulatory
intuition. Informed and liquidity traders choose between a displayed exchange and a
dark pool that free-rides on exchange prices. Matching in the dark pool depends on
counterparty availability, so orders on the heavier side fail to execute — and informed
orders are positively correlated with each other, so they cluster on the heavy side and
face lower execution probabilities in the dark, while liquidity orders are less
correlated and fill more easily. That differential execution risk pushes relatively
more informed traders to the exchange and more uninformed traders into the dark pool,
so under natural conditions the self-selection lowers the noisiness of exchange supply
and demand and *improves* price discovery on the exchange. The improvement is neither
free nor universal: better price discovery need not coincide with higher liquidity or
welfare, since more informative orders worsen adverse selection on the exchange and
produce wider spreads. Zhu contrasts his result with Ye (2011), who finds the opposite
under exogenous liquidity-trader venue choice.

Haslag and Ringgenberg (2023) reconcile the literatures by showing the average conceals
a bifurcation. Across 2003–2016, with fragmentation measured as one minus the
Herfindahl index, fragmentation is associated with smaller and more frequent trades,
reduced spreads and better price efficiency for **large stocks** — the competition
channel — but for **small stocks** with less trading, larger trades, increased spreads
and worse price efficiency, the adverse-selection channel in which liquidity providers
facing more fragmented and thus more toxic flow widen spreads. They state the
small-stock magnitude as a one-standard-deviation increase in fragmentation being
associated with a 2% to 4% effect. 

| Benefit of fragmentation | Source |
| --- | --- |
| Lower transaction costs, faster execution, more efficient prices | O'Hara and Ye (2011) |
| Higher consolidated depth after venue entry; spreads narrow or hold | Foucault and Menkveld (2008) |
| Reduced spreads, better price efficiency for large stocks | Haslag and Ringgenberg (2023) |
| Dark self-selection can improve price discovery | Zhu (2014) |
| A finer effective pricing grid raises liquidity, lowers volatility | Comerton-Forde, Grégoire and Zhong (2017) |

| Cost of fragmentation | Source |
| --- | --- |
| Small stocks: less trading, larger trades, wider spreads, worse efficiency | Haslag and Ringgenberg (2023); SEC 2013 |
| Dark fragmentation raises costs, degrades price discovery | SEC 2013, citing ASIC (2013), Hatheway, Kwan and Zheng (2013), Comerton-Forde and Putniņš (2012) and others |
| Displayed depth may be overstated by duplicated orders | van Kervel (2012), in SEC 2013 |
| Larger price moves under high quote fragmentation in stress | Madhavan (2012), in SEC 2013 |
| Latency arbitrage is a measurable tax on liquidity provision | Aquilina, Budish and O'Neill (2022) |

## Speed and the latency arms race

Budish, Cramton and Shim (2015) frame the arms race as a defect of continuous trading
rather than of technology. Spread Networks' New York–Chicago fibre, built at an
estimated cost of USD 300 million, cut round-trip latency from 16 ms to 13 ms;
microwave links then reached 10, 9 and 8.5 ms, and the cable was obsolete. Their
argument is that market correlations break down at millisecond horizons, creating
purely technical arbitrage available to whoever is fastest, that the resulting race is
a prisoner's dilemma built into the design and paid for by investors through wider
spreads and thinner markets, and that discrete-time uniform-price **frequent batch
auctions** — for example every 1 second — convert speed competition into price
competition. In their ES–SPY data technical arbitrage opportunities number about 800
per day on average, with latency-arbitrage profits of USD 79 thousand per day from NYSE
data alone (99th-percentile day USD 554 thousand), or USD 306 thousand per day —
roughly USD 75 million per year — scaled to all exchanges.

Aquilina, Budish and O'Neill (2022) measure the race from message data rather than book
data, which matters because messages contain the losers: the book shows only who won.
In UK equities the average FTSE 100 symbol has **537 latency-arbitrage races per day**
— about one per minute per symbol — and the modal race lasts **5–10 microseconds**.
About 22% of FTSE 100 volume is in races. Participation is concentrated: the top 6 firms
win 82% of races and lose 87%, taking about 80% of the liquidity taken in races while
providing about 42% of the liquidity that gets taken. Each race is worth only a bit more
than half a tick, about GBP 2, but price impact from racing is about 31% of all price
impact and about 33% of the effective spread; the latency-arbitrage tax is 0.42 basis
points on total volume and 0.53 basis points on non-race volume against an effective
spread of just over 3 basis points; and designs that eliminate latency arbitrage would
reduce the market's cost of liquidity by **17%**, with annual sums at stake on the order
of USD 5 billion per year across global equity markets. The link to routing is direct:
some fraction of the spread an execution algorithm pays reflects races it cannot win on
latency, which is why the remedy is market design rather than a faster router. The
counterweight is that Menkveld (2016) identifies "speed to connect fragmented markets"
as a productive channel of high-frequency trading, since those firms economise on the
fixed costs of fast market access and link venues.

## Cross-venue routing as an allocation problem

Take the venue set $\mathcal{V}$ for one symbol. For venue $v$ the router observes, or
must estimate: a displayed bid and ask $(b_v,a_v)$ and size at each; hidden or reserve
depth, knowable only through fill history; an effective fee $f_v$, positive when the
router pays on the relevant side and negative when the venue rebates; a latency
$\ell_v$; and an execution probability $\pi_v$ for a resting child order at $v$ before
the deadline.

Three of these interact in ways the literature supports. The **net price** compared
across venues is the quoted price adjusted by the fee, so on a buy venue $v$'s offer
costs $a_v + f_v$ to take and $a_v + f_v'$ to make, where $f_v'$ has the opposite sign.
The **Reg NMS constraint** binds on $a_v$, not on $a_v + f_v$: Rule 611 forbids
execution at a price inferior to a protected quotation, so the router cannot prefer a
rebating venue at a worse price and must satisfy or sweep every better protected price
first. And **fill probability** is venue-specific and not free: posting to capture a
rebate behind a long queue has a lower $\pi_v$ than posting behind a shorter one, which
is Hasbrouck's point that the rebating venue is not necessarily the right place for a
limit order. **Latency** enters twice — as the cost of reaching a venue whose quote may
have moved, and as the probability that a stale quote at $v$ is picked off by someone
faster.

A defensible rule treats taking and posting separately, because the constraints differ.
The **taking** decision is a sweep: sort venues by protected price, walk the ladder from
the best, execute at each up to displayed (and, where accessible, reserve) size, correct
each fill by its fee, and stop when marginal net price exceeds the constraint — the
intermarket sweep order being the compliance mechanism that lets receiving venues
execute immediately. The **posting** decision is an expected-value comparison, not a
spread comparison:

$$ \pi_v\,(h_v + r_v) - (1-\pi_v)\,\lambda - \ell_v\,\sigma \;>\; \pi_{v'}(h_{v'} + r_{v'}) - (1-\pi_{v'})\,\lambda - \ell_{v'}\,\sigma $$

for any two candidates, where $h_v$ is the half-spread captured, $r_v$ the effective
rebate net of the venue's adverse-selection penalty as in Chapter 8, $\lambda$ the cost
of not filling by the deadline, and $\sigma$ the per-unit rate of price movement over
the latency gap. The components are each supported — Cont and Kukanov (2017) for the
$\pi$ against $h+r$ trade-off, Comerton-Forde, Grégoire and Zhong (2017) for the fee
creating the sub-penny grid, Foucault and Menkveld (2008) for the cross-venue
competition that makes $r_v$ differ, Hasbrouck (2025) for the fee signs — while the
assembled inequality is **this chapter's construction**, not a result stated in any one
source.

On when splitting pays, the material gives conditional answers rather than a threshold.
Splitting raises the number of connections and the fixed cost per child order, so it
pays when the size to be executed exceeds what one venue's displayed depth at the touch
absorbs at an acceptable price — the equity analogue of the crypto gas-fee problem in
Chapter 11. It pays more for large stocks than small ones, because for small stocks the
adverse-selection cost of fragmentation dominates the fee and price gain (Haslag and
Ringgenberg 2023) and because displayed depth is thin: evaluating Nasdaq stocks, SEC
staff measured average displayed depth at the NBBO of 1,833 shares with a median of 581
(SEC 2005). It pays less when displayed depth is duplicated and will partly evaporate
(van Kervel 2012), and less when venues' quotes are correlated in stress, since added
venues then add correlation rather than diversification (Madhavan 2012). Finally the
value of the routing decision is bounded by how many candidate venues the router can
reach and how smart the routing is: Foucault and Menkveld's estimated smart-router
shares of 27% and 18% show that the consolidated best price can be systematically left
un-hit even when it is displayed. That reading, and the assumption that a trader can
measure $\pi_v$ and $\lambda$ rather than only the spread, is **the chapter's
judgement**.

## Routing in crypto markets

The crypto venue set is fragmented in a stronger sense: there is no consolidated tape,
no protected-quotation rule and no NBBO, so cross-venue price gaps are large and
persistent rather than arbitraged away, and the binding constraints are capital
mobility and settlement latency rather than a fee cap. Chapter 11 sets out the evidence
— the two-layer CEX cost stack of a proportional venue fee plus a fixed per-transaction
gas charge, the size threshold above which a decentralized venue becomes
cost-competitive, and the fact that on-chain routing is convex without fixed costs and
mixed-integer with them.

## Sources

- U.S. Securities and Exchange Commission (2005). *Regulation NMS*, Exchange Act Release No. 34-51808, 70 FR 37496. [sec.gov](https://www.sec.gov/rules/final/34-51808.pdf)
- U.S. Securities and Exchange Commission (2010). *Concept Release on Equity Market Structure*, Release No. 34-61358. [sec.gov](https://www.sec.gov/rules/concept/2010/34-61358.pdf)
- Staff of the SEC Division of Trading and Markets (2013). *Equity Market Structure Literature Review, Part I: Market Fragmentation*.
- U.S. Securities and Exchange Commission (2023). *Order Competition Rule*, Proposed Rule 615, Release No. 34-96495, 88 FR 128.
- Foucault, T., Menkveld, A. J. (2008). *Competition for Order Flow and Smart Order Routing Systems.* Journal of Finance 63(1), 119–158.
- O'Hara, M., Ye, M. (2011). *Is Market Fragmentation Harming Market Quality?* Journal of Financial Economics 100(3), 459–474. (Not obtained; summarised from the SEC staff review and citing papers.)
- Zhu, H. (2014). *Do Dark Pools Harm Price Discovery?* Review of Financial Studies 27(3), 747–789.
- Budish, E., Cramton, P., Shim, J. (2015). *The High-Frequency Trading Arms Race: Frequent Batch Auctions as a Market Design Response.* Quarterly Journal of Economics 130(4), 1547–1621.
- O'Donoghue, S. M. (2015). *The Effect of Maker-Taker Fees on Investor Order Choice and Execution Quality in U.S. Stock Markets.* Working paper, Indiana University.
- Menkveld, A. J. (2016). *The Economics of High-Frequency Trading: Taking Stock.* Annual Review of Financial Economics 8, 1–24.
- Comerton-Forde, C., Grégoire, V., Zhong, Z. (2017). *Inverted Fee Venues and Market Quality.* SSRN 2939012.
- Mishra, S., Zhao, L. (2021). *Order Routing Decisions for a Fragmented Market: A Review.* Journal of Risk and Financial Management 14(11), 556.
- Aquilina, M., Budish, E., O'Neill, P. (2022). *Quantifying the High-Frequency Trading "Arms Race".* Quarterly Journal of Economics 137(1), 493–564.
- Ernst, T., Spatt, C. S. (2022). *Payment for Order Flow and Asset Choice.* NBER Working Paper No. 29883.
- Haslag, P., Ringgenberg, M. C. (2023). *The Demise of the NYSE and Nasdaq: Market Quality in the Age of Market Fragmentation.* Journal of Financial and Quantitative Analysis 58(7), 2753–2782.
- Hasbrouck, J. (2025). *Securities Trading: Principles and Procedures*, Version 14c, Chapters 16 and 19. (Course text; the commercial text itself was not obtained.)

## Key points

- Fragmentation is large and measurable: NYSE and Nasdaq fell from approximately 95% of U.S. equity volume in 2000 to under 30% by 2016, and September 2009 data show 63.8% of volume at registered exchanges, 10.8% at ECNs, 7.9% at roughly 32 dark pools and 17.5% at more than 200 internalising dealers.
- Reg NMS is the constraint set of routing: Rule 611 forbids executing at a price inferior to a protected quotation, Rule 610 caps the access fee at USD 0.003 per share and bans locking or crossing automated quotations, and Rule 612 sets a one-cent minimum increment, with protection extending only to quotations that are immediately and automatically accessible.
- Foucault and Menkveld show routing automation is what makes competing venues viable at all — an entrant survives only if the smart-router share exceeds a threshold — and that consolidated depth rises after entry even as incumbent depth falls.
- Fees are a first-order routing variable: a Nasdaq-style schedule pays USD 0.0029 per visible share and charges USD 0.0030, an inverted venue reverses the signs, and the penny grid means a rebate cannot be expressed as a better price, so inverted venues compete by creating a finer grid inside the spread.
- The cheapest execution path is not the best price: O'Donoghue finds maker-taker pricing can narrow the quoted spread while raising the investor's total cost, with venues and brokers capturing the difference.
- Internalisation and PFOF divert most retail flow off-exchange — about 40 cents per 100 shares in options against 20 cents in equities — and the withdrawn Order Competition Rule would have forced retail marketable orders into public auctions first.
- Fragmentation's net effect on execution cost is conditional: beneficial for large stocks, harmful for small ones where a one-standard-deviation increase is associated with a 2% to 4% worsening; and cross-venue latency is at once a 0.42–0.53 basis point tax, about a third of the effective spread, and a documented source of lower transaction costs.
- A router's taking decision is a fee-corrected sweep of protected prices; its posting decision compares fill probability against half-spread, rebate, non-fill cost and latency, and splitting across venues pays in size, in large stocks, and where displayed depth is not duplicated.
