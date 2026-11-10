# A short history: from the floor to the algorithm

Every execution model in this book answers a market structure that already existed. The
mean-variance schedule of Chapter 5, the order-placement logic of Chapter 8 and the routing problem
of Chapter 9 were not designed in the abstract; they responded to a specific institutional question,
and that question has changed more than once. Each structural change did not merely add a technology:
it moved execution cost from one party to another and redefined what "the benchmark" means.

## Before the screen

Many of today's markets began as *floor markets* — a central place where traders met face to face. The
New York Stock Exchange (NYSE) first operated in the Tontine Coffee House; the American Stock Exchange
grew out of the New York Curb Market outside. The floor peaked in the second half of the twentieth
century, then gave way to screen trading: the London Stock Exchange closed its floor in 1992 and the
Chicago Mercantile Exchange (CME) closed most of its pits in 2015.

The floor's mechanics matter because the rules they produced survive in adapted form. On the CME,
trading was confined to the exchange's designated area (Rule 520) and had to occur by *open outcry* in
the contract's pit (Rule 521). Two features followed. Quotes were *passive*: "Bidding 3.85 for three"
declared availability only, and to produce a trade someone had to act, shouting "Sell it!" to hit a bid
or "Buy it!" to lift an offer. And there was *no time priority at a price*: in the crowded pit a seller
could hit any trader bidding the same price.

Two other practices became permanent. A *trade-through* is an execution at a price worse than a
standing quote elsewhere: if one trader bids 100, another bids 101, and a third sells to the first at
100, the second is traded through. On the CME this was an infraction (Rule 528) remedied by breaking
the trade or making the seller fill the aggrieved bidder. A *cross* — pairing one customer's buy
against another's sell at a fair mid-price — required a ritual: the broker bid and offered by open
outcry three times at the same price before matching (Rule 533), so that a trade never appears in the
record with no bid or offer behind it. Trades disconnected from preceding visible quotes were called
"dark", a term that survives. The floor ran on reputation: rules were silent on how long a quote stayed
valid, so enforcement rested on a convention — a quote was good "as long as the breath is warm".

Above the crowd sat the *specialist*. At the NYSE (one per stock by the 1960s) he was an independent
trader who managed the single limit-order book in each stock, kept on paper or chalkboard. His balance
of obligations and privileges is the template for every market-making debate since. The obligations:
post a reasonably narrow bid and offer; avoid destabilising trades, staying neutral rather than taking
public orders; *yield to customer orders at the same price* (public priority); cross simultaneous
public buy and sell orders, generally at the midpoint; and bridge large price gaps to keep price
continuity. The privileges mirrored these: as agent for the book he knew its contents, and as agent for
incoming market orders he had a first-mover advantage in deciding whether to take the other side. US
equity tick sizes stayed at one-eighth of a dollar, USD 0.125, until about 2000.

Large orders were the hardest problem, and the floor handled them by negotiation rather than display: a
broker holding both a customer buy and a customer sell could cross them (subject to Rule 533), and a
*point-in-time crossing network* later automated exactly this. What the sources used here do **not**
document is the "upstairs" block market, where a broker telephones dealers to work a large order before
it reaches the floor; no claim about it is made here.

## The move to screens

The first screen markets tried to imitate the floor. In the late 1980s the Chicago Board of Trade and
Apple built *Aurora*, showing buyers and sellers as circles and squares in an on-screen "trading ring";
it was never implemented. LIFFE built *Automated Pit Trading*, copying the "warm breath" convention
with a ten-second order-refresh limit. The future lay elsewhere: the CME's *GLOBEX*, an electronic
limit-order market developed at the same time, became the model, and the screen-based limit order book
now governs most trading in equities, options and futures.

Electronification also required seeing one market from many places. *Market information systems* — for
NYSE- and AMEX-listed stocks the Consolidated Trade System (CTS) and Consolidated Quote System (CQS),
run by the Consolidated Tape Association, with analogues for Nasdaq stocks — broadcast trade reports
and each centre's best bid and offer, from which the National Best Bid and Offer (NBBO) is computed; the
SEC designates most such operators as Securities Information Processors (SIPs). *Access systems* carried
orders the other way, and *routing systems* chose where an order should go. New venues exploited that
connectivity: by the 1990s Instinet was close to a modern all-electronic limit-order book, Island
attracted retail traders, and Optimark, Wunsch Auction Systems and Delta Options tried other designs.
The SEC's response took until 1997, when it adopted *Regulation ATS*, letting these venues operate
without becoming exchanges. (One source differs: O'Hara's 2015 survey places Reg ATS in 2000; the 1997
adoption date is the one in the SEC's rulemaking history as relayed by Hasbrouck.)

The systemic risk of electronification became visible in **October 1987**, in a market "break" with a
sudden decline across virtually all stocks. **The material read here mentions this only in passing, and
nothing below goes beyond it.** It records that market-wide circuit breakers were first instituted in
response to that break (not triggered until 27 October 1997, when the consensus was that they had fired
prematurely); that the Federal Reserve encouraged banks to extend credit to securities firms; and that
options and portfolio-insurance hedges require *dynamic* hedging and therefore demand speed. The rise of
program trading as such is not documented in the material used here.

## Algorithmic execution takes shape

Three changes turned execution into an optimisation problem.

The first was **decimalisation**. US equities quoted in eighths (USD 0.125); Congress's Common Cents
Pricing Act of 1997 set the penny as the increment, exchanges moved first to 1/16ths, and penny pricing
was completed in 2001. Tick size is, in Hasbrouck's phrase, the price of time priority: with a
20,000-share queue at USD 10.00, bidding USD 10.01 jumps the line for one cent per share. Decimalisation
shrank the grid over which price competition happens.

The second was **market access**. Direct market access (DMA) and sponsored access let proprietary and
buy-side traders send orders straight to exchange systems. Lehalle's lectures give an industry figure:
"in most brokerage firms, Algorithmic trading executes more than 80% of the orders (in value)", with
algo users paying fees of 2 to 8 basis points.

The third was the **first generation of execution algorithms**, each defined by a benchmark. TWAP is
the naive equal-split schedule. VWAP, whose use as an execution benchmark traces to Berkowitz, Logue
and Noser (1988), slices the order to track the day's volume curve; Konishi formalises the result that
the optimal static VWAP-liquidation curve equals the relative market-volume curve when volatility and
volume are uncorrelated. POV (percentage of volume) fixes a demand/supply impact, and Guéant (2014)
gives the constant-participation optimal control. Implementation shortfall (IS) benchmarks against the
*decision price* and is Chapter 4's subject; Perold's 1988 paper named the gap between the "paper
portfolio" and the implemented one, and Mittal (2005) argues IS, not VWAP, is the correct objective,
because VWAP is an influenceable moving target, while VWAP and POV remain tools for reducing IS provided
risk is also controlled.

The two founding models of optimal-execution theory arrive at the end of this period, taking IS as
given: Bertsimas and Lo (1998) minimise expected cost over a schedule, and Almgren and Chriss (December
2000 preprint; *Journal of Risk*, 2001) add a variance penalty and characterise the efficient frontier
— Chapter 5's subject. Menkveld documents the payoff: US equity effective spreads fell from roughly the
mid-teens to low single digits of basis points between 2001 and 2011, with retail commissions collapsing
toward zero.

## Fragmentation and high frequency

**Regulation NMS** — proposed February 2004, adopted June 2005, phased in over 2006 — implemented a
vision set out three decades earlier, when the Securities Acts Amendments of 1975 directed the SEC to
facilitate a "national market system" electronically linking the separate markets. It has four
components: the **Order Protection Rule** (Rule 611), requiring trading centres to establish written
policies "reasonably designed to prevent" executions at prices inferior to *protected* quotations
displayed elsewhere; the **Access Rule** (Rule 610), requiring fair, non-discriminatory access, capping
access fees at USD 0.003 per share, and prohibiting a pattern of displaying quotations that lock or
cross other automated quotations; the **Sub-Penny Rule** (Rule 612), a one-cent minimum increment for
stocks at or above USD 1.00; and the **Market Data Rules**.

The definitional move in Rule 611 is *protected*: a quotation is protected only if immediately and
automatically accessible. That refuses protection to slow, manual quotations, which is why the rule
forced floor markets — notably the NYSE — to automate quickly; Hasbrouck says it "made a distinction
between 'fast' and 'slow' markets". The rule is not a prohibition: a market must make a good-faith
effort to avoid trade-throughs, and a trader can instead use an **intermarket sweep order** (ISO),
notifying each market of the sweep and relieving it of the duty to check for trade-throughs itself.

Adoption rested on measured harm: an SEC staff study found roughly **1 in 40 trades** in NYSE and
Nasdaq stocks executed at prices inferior to the best displayed quotation — about **98,000
trade-throughs per day in Nasdaq stocks alone** — and hidden costs from price impact and liquidity
search were estimated at more than **USD 30 billion per year**, in a range cited up to USD 100 billion,
against more than 84 million individual US investors and a listed universe worth over USD 14 trillion.

The result was fragmentation beyond the 1975 vision. The SEC's 2010 Concept Release on Equity Market
Structure, using October 2009 data, found registered exchanges collectively executing about **63.8%** of
share volume with no single exchange above **19.4%**; the five active ECNs about **10.8%**; displayed
centres about **74.6%** and undisplayed centres (dark pools and internalisers) about **25.4%**. The
canonical demonstration: **the NYSE executed about 79.1% of consolidated volume in its listed stocks in
January 2005, versus 25.1% in October 2009.** Volume exploded — 181% more shares and 662% more trades
in January–October 2009 than in the comparable 2005 period — and the release put high-frequency trading
at more than 50% of US equity volume. Whether this helped is size-dependent: O'Hara and Ye (2011) found
more fragmented stocks had lower transaction costs and faster execution, while Haslag and Ringgenberg
(2023) show the average conceals the opposite, with fragmentation improving outcomes for *large* stocks
and worsening them for *small* stocks by roughly 2% to 4% per standard deviation. The SEC's own 2013
staff review reaches the same conditional conclusion.

High-frequency trading is the counterpart of this structure. The clean causal evidence is Hendershott,
Jones and Menkveld (2011), who exploit the NYSE's **staggered introduction of "autoquote" in 2003** —
automatic dissemination of the inside quote on any relevant book change — as an instrument across 943
common stocks. For large stocks, algorithmic trading narrows quoted and effective spreads, and the
narrowing comes from a sharp decline in *adverse selection*: it increases price discovery that occurs
without trading, so quotes become more informative. Their measure was already thought to account for as
much as 73% of US volume in 2009.

The failure mode appeared on **6 May 2010**. In the Flash Crash a mutual-fund complex sent a sell
program of **75,000 E-mini S&P 500 futures contracts (about USD 4.1 billion)**, executed by an algorithm
set to sell at **9% of the previous minute's volume without regard to price or time**. Between 2:32 and
2:45 p.m. Central Time it sold about **35,000 contracts (about USD 1.9 billion)**. By 2:45:28, buy-side
resting orders in the E-mini had fallen below **1,050 contracts — less than 1%** of the buy-side depth
at the open — before the CME's five-second Stop Logic pause. More than **300 securities, including many
ETFs, traded 60% or more away** from pre-crash levels, and regulators broke over 20,000 trades.
Kirilenko, Kyle, Samadi and Tuzun's trade-by-trade study reaches the central result: the trading pattern
of the most active HFT intermediaries **did not change** when prices fell, and their combined net
inventory never exceeded **6,000 E-mini contracts**, an order of magnitude smaller than the
75,000-contract sell program. Depth vanished rather than being replenished. Harris names the seller
(Waddell & Reed's Asset Strategy Fund, USD 27 billion), notes that the algorithm's own volume-following
rule raised volume and so made it sell faster, and puts the crude cost at about **USD 100 million**.

The Flash Crash exposed a race long building. Budish, Cramton and Shim (2015) document that Spread
Networks' **USD 300 million** New York–Chicago fibre cut round-trip latency from **16 ms to 13 ms**, soon
beaten by microwave links at 8.5 ms. Millisecond direct-feed data show market correlations near 1 at
human time scales (E-mini versus SPY) breaking down at high frequency — median return correlation
0.1016 at 10 milliseconds and 0.0080 at 1 millisecond — creating purely *technical* arbitrage for
whoever is fastest, on average about **800 opportunities per day**, worth **USD 79k per day** on average
(99th-percentile day USD 554k) and scaling to about **USD 306k per day, roughly USD 75 million per
year** across correlated equity pairs. Their proposed fix is *frequent batch auctions*: uniform-price
sealed-bid double auctions at discrete intervals, e.g. every 1 second, making a millisecond advantage
worthless.

Aquilina, Budish and O'Neill (2022) measured the same race directly with exchange *message* data that
include losing as well as winning attempts. In UK equities (FTSE 100/250, autumn 2015), races average
**537 per day for a FTSE 100 symbol — about one per minute**; the modal race lasts **5 to 10
microseconds**; and they account for about **22% of trading volume**. Participation is concentrated: the
top three firms win 55% and lose 66% of races, the top six win 82% and lose 87%, and the top six take
about 80% of the liquidity taken in races while providing about 42% of it. Price impact from races is
about **31% of all price impact and 33% of the effective spread**; the latency tax is about 0.42 basis
points of volume, and eliminating latency arbitrage would cut the market's cost of liquidity by **17%**,
with about GBP 60 million at stake annually in the UK and on the order of **USD 5 billion globally**.
The US practical response was delay: RBC's THOR router and the IEX exchange, approved by the SEC in
2016, insert a 350-microsecond delay on inbound and outbound messages so that no participant can act on
one venue's execution before its order reaches another; the SEC accepted the delay as "de minimis".
Menkveld supplies the balanced reading: HFT firms and the new venues had a *symbiotic* relationship that
drove the migration to electronic trading and delivered lower costs and more volume, and speed used to
*connect* fragmented markets is a form of liquidity provision, not only an arms race.

## The modern era and a different starting point

Since 2010 the theory of execution has moved in the two directions developed later: the static
closed-form schedule of Almgren–Chriss gave way to *adaptive and stochastic control* treatments —
limit-versus-market-order choice, dark pools, participation constraints, utility-based objectives — and
then to *data-driven and learning* approaches including reinforcement learning (Chapters 6 and 10).

Cryptocurrency markets are, in a real sense, a different starting point: a place where the equity
institutional history did not happen and the design questions are open again. Three differences matter
for execution, and Chapter 11 develops them. First, **continuous operation without a consolidated
market**: Binance and other spot venues trade 24/7, while the CME's bitcoin futures adhere to
traditional hours, and there is no single opening or closing auction and no consolidated tape. Barbon
and Ranaldo's dataset alone covers about 70% of centralised-exchange volume and roughly 60% of
decentralised-exchange volume, and Makarov and Schoar (2020) document large, *recurrent* arbitrage
deviations between exchanges that persist because arbitrage capital cannot move freely. Merging these
sources, it is **this chapter's judgement** that the market-wide halts the US equity market uses to
interrupt a collapse have no obvious crypto analogue, because there is no single market to halt and no
consolidated tape to trigger it; no single source states this.

Second, **venue quality dispersion**. A centralised exchange's effective cost is half-spread plus
exchange fee; a decentralised exchange's is quoted half-spread plus protocol fee plus gas. Because the
gas fee is a *fixed* cost per transaction, its share of total cost falls as trade size grows: Barbon and
Ranaldo find small and medium trades cheaper on centralised exchanges and decentralised exchanges
competitive for larger trades, making crypto venue choice explicitly size-dependent. Third,
**derivatives, not spot, are the primary execution venue**: crypto perpetual futures were introduced by
BitMEX in 2016, and He, Manela, Ross and von Wachter report that in 2022 the median total daily
perpetual-futures volume across exchanges was **USD 101.9 billion, about two to three times** total spot
volume. The perpetual has no expiry but pays a *funding rate* — a clamped function of the futures–spot
premium — so borrowing cost and funding timing enter the execution problem directly: a router faces a
worse quoted spread but better depth and more balanced flow on the perpetual leg.

## A timeline

| Year | Event or institution | Meaning for execution | Source |
|---|---|---|---|
| 20th c. | Floor markets at their zenith; NYSE specialist manages the book | Execution = negotiated, reputation-enforced, specialist-intermediated | Hasbrouck Ch. 3, 7 |
| until c. 2000 | US tick size = 1/8 dollar (USD 0.125) | Time priority priced at 12.5 cents per share | Hasbrouck Ch. 7, 20 |
| late 1980s | CBT's Aurora (unimplemented); LIFFE APT; CME GLOBEX | The limit order book, not the simulated pit, became the electronic market | Hasbrouck Ch. 3 |
| Oct. 1987 | Market "break"; circuit breakers instituted | *Mentioned only in passing* | Hasbrouck Ch. 3, 11 |
| 1992 / 2015 | LSE closes floor / CME closes most pits | End of the physical floor as primary mechanism | Hasbrouck Ch. 3 |
| 1990s | Instinet, Island, Optimark, Wunsch, Delta Options | New electronic venues fragment order flow | Hasbrouck Ch. 20 |
| 1997 | Reg ATS adopted; Common Cents Pricing Act | Electronic venues legalised; decimalisation legislated | Hasbrouck Ch. 20 |
| 1988 / 2005 | Perold (IS); Berkowitz–Logue–Noser (VWAP); Mittal (IS objective) | Benchmarks: decision price, then VWAP, then IS | Cluster B |
| 1998 / 2000 | Bertsimas–Lo; Almgren–Chriss | IS-minimisation becomes a scheduling problem | Cluster B |
| 2001 | Penny pricing completed in US equities | Finer price grid; spread compression | Hasbrouck Ch. 20 |
| 2003 | NYSE staggered autoquote introduction | Instrument for the AT/liquidity causal test | Hendershott et al. 2011 |
| 2004–2006 | Reg NMS proposed, adopted, phased in | Order protection, access-fee cap, sub-penny rule | SEC 2005, Release 34-51808 |
| 2005 → 2009 | NYSE share of its listed stocks: 79.1% → 25.1% | Fragmentation of the primary listing venue | SEC Concept Release 2010 |
| 6 May 2010 | Flash Crash | A large price-insensitive order met vanishing depth | Kirilenko et al. 2017; Harris 2015 |
| 2011 | US effective spreads at low single-digit bps | Execution-cost collapse of the 2000s | Menkveld 2016 |
| 2015 / 2022 | Budish et al.: batch auctions; Aquilina et al.: races measured | The latency race quantified and given a design fix | Budish 2015; Aquilina 2022 |
| 2016 | IEX approved as an exchange with 350-microsecond delay | Delay as a market-design instrument | Hasbrouck Ch. 21 |
| 2016 → 2022 | BitMEX introduces perpetuals; volume 2–3× spot | Derivatives become the primary crypto execution venue | He et al. |

## Three regularities

**Market-structure changes redistribute execution cost; they do not simply lower it.** The SEC's stated
objective for Reg NMS was to minimise long-term investors' transaction costs, and access fees, tick size
and order-protection rules are levers over who bears what. The evidence bears out redistribution:
Hendershott, Jones and Menkveld find algorithmic trading's spread narrowing comes from a decline in
*adverse selection*, a transfer from liquidity suppliers; Haslag and Ringgenberg find fragmentation
helps large caps and hurts small caps; and the latency race is a measurable tax on liquidity provision
(Budish, Cramton and Shim; Aquilina, Budish and O'Neill). The cost did not disappear; its incidence
moved.

**Benchmarks follow institutions.** The benchmark an algorithm matches tracks the opportunity the market
actually presents. Where the relevant price was the price at decision time, Perold's *decision-price* IS
was the benchmark; where the comparison was the day's average trade, Berkowitz, Logue and Noser proposed
VWAP; as venues multiplied and VWAP became gameable, Mittal argued IS was the objective and VWAP and POV
mere tools. In crypto, with no consolidated tape and perpetuals dominant, the "arrival price" must be
read from the leading venue rather than the venue being traded.

**Speed advantages are repriced, not abolished.** Every attempt to remove the advantage of being fast —
penning the pit's advantage in a formal rule, forcing fast and slow markets onto equal terms, letting a
market impose delay — has produced a new form of the same advantage elsewhere. The floor's membership
seat and informational edge became the specialist's book knowledge, then collocation, then the HFT
firm's latency and fee model, then the crypto searcher's relationship with a block builder. Budish,
Cramton and Shim and Menkveld give the reason to expect this: as long as speed is rewarded, capital is
spent on speed, and the design question is whether the reward buys anything socially useful. **This
chapter's judgement**, drawn from the sources' own framing rather than a single measurement, is that the
recurring pattern in which systemic risk appears where concentration and interconnection are highest —
1987, 2010, Knight Capital — is the durable lesson, and that each structure change is best read as a
redistribution of that risk rather than its elimination.

## Sources

- Hasbrouck, J. *Securities Trading: Principles and Procedures*, Version 14c (2025), NYU Stern — Ch. 3
  (Floor Markets), 4 (Limit Order Markets), 5 (Multiple Markets), 7 (Dealers), 8 (Dark Markets), 11
  (Circuit Breakers), 20 (Reg NMS), 21 (High Frequency Trading).
- U.S. Securities and Exchange Commission (2005). *Regulation NMS*, Exchange Act Release No. 34-51808,
  70 FR 37496.
- U.S. Securities and Exchange Commission (2010). *Concept Release on Equity Market Structure*, Release
  No. 34-61358, File No. S7-02-10.
- SEC Staff, Division of Trading and Markets (2013). *Equity Market Structure Literature Review, Part I:
  Market Fragmentation*; (2014) *Part II: High Frequency Trading*.
- Hendershott, T., Jones, C. M., and Menkveld, A. J. (2011). Does Algorithmic Trading Improve
  Liquidity? *Journal of Finance* 66(1), 1–33.
- Kirilenko, A., Kyle, A. S., Samadi, M., and Tuzun, T. (2017). The Flash Crash: High-Frequency Trading
  in an Electronic Market. *Journal of Finance* 72(3), 967–998.
- Budish, E., Cramton, P., and Shim, J. (2015). The High-Frequency Trading Arms Race: Frequent Batch
  Auctions as a Market Design Response. *Quarterly Journal of Economics* 130(4), 1547–1621.
- Aquilina, M., Budish, E., and O'Neill, P. (2022). Quantifying the High-Frequency Trading "Arms Race".
  *Quarterly Journal of Economics* 137(1), 493–564.
- Menkveld, A. J. (2016). The Economics of High-Frequency Trading: Taking Stock. *Annual Review of
  Financial Economics* 8, 1–24.
- Haslag, P., and Ringgenberg, M. C. (2023). The Demise of the NYSE and Nasdaq. *Journal of Financial
  and Quantitative Analysis* 58(7), 2753–2782.
- O'Hara, M. (2015). High frequency market microstructure. *Journal of Financial Economics* 116(2),
  257–270.
- Harris, L. (2015). *Trading and Electronic Markets: What Investment Professionals Need to Know*, CFA
  Institute Research Foundation — Ch. 5–6 and Appendix A (The Flash Crash).
- Lehalle, C.-A. (2015). *Market Microstructure in Practice*, IPAM/UCLA lecture series.
- He, S., Manela, A., Ross, O., and von Wachter, V. *Fundamentals of Perpetual Futures* (working paper).
- Barbon, A., and Ranaldo, A. *On the Quality of Cryptocurrency Markets: Centralized vs. Decentralized
  Exchanges* (working paper).
- Makarov, I., and Schoar, A. (2020). Trading and arbitrage in cryptocurrency markets. *Journal of
  Financial Economics* 135(2), 293–319.

## Key points

- Floor markets produced the rules electronic markets inherited in adapted form — open outcry, no
  cross-market time priority, trade-through remedies, and the specialist's obligations and privileges,
  whose book knowledge and first-mover advantage are the template for every market-maker debate since.
- Electronification chose the limit order book over a simulated pit and added consolidated market
  information, access and routing systems; the October 1987 break is documented only in passing in the
  sources used here, and program trading's rise is not documented at all.
- Decimalisation (completed 2001), direct market access and the first generation of algorithms (TWAP,
  VWAP, POV, implementation shortfall) turned execution into a benchmarked optimisation problem, with
  Perold's decision price and a variance penalty entering via Almgren–Chriss.
- Reg NMS (adopted 2005) restructured US equities through the order protection rule, a USD 0.003
  access-fee cap, the sub-penny rule and the market data rules; its "immediately accessible" definition
  of a protected quote forced manual markets to automate.
- Fragmentation followed: the NYSE's share of its own listed stocks fell from about 79.1% in January
  2005 to 25.1% in October 2009, and its effect is positive for large caps and negative for small caps.
- HFT is the modern liquidity supplier, with causal evidence that algorithmic trading narrows spreads by
  reducing adverse selection; the Flash Crash showed the failure mode when price-insensitive size meets
  vanishing depth, and the latency race is a measurable tax of about half a basis point.
- Crypto is a different starting point: continuous 24/7 trading without a consolidated tape,
  size-dependent venue choice between centralised and decentralised exchanges, and perpetual futures as
  the primary venue, with funding cost entering the execution problem directly.
- Three regularities hold: market-structure changes redistribute execution cost rather than lowering it;
  benchmarks follow institutions; and speed advantages are repriced rather than abolished.
