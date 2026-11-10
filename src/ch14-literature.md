# The literature: a full map and a reading path

Optimal execution is not one literature but eight that rarely cite each other at
the same time. A dealer's inventory model, a broker's implementation-shortfall
white paper, a paper on Hamilton–Jacobi equations, an order-book econometrics
study, a regulator's release and a reinforcement-learning paper all answer "how
to trade" — and each answers a different part of it. This chapter draws the map,
then gives three reading paths through it.

Everything below rests on material actually read; the extraction notes are in
`materials/notes/`. Where a work could not be obtained it is listed as such, and
nothing is asserted about its contents.

---

## Eight schools of thought

### 1. Microstructure foundations — where cost comes from

**The problem.** Why does an order cost anything before any explicit fee? Two
logically independent mechanisms produce the answer.

**Core method.** Equilibrium models of one asset in which an intermediary (a
dealer or market maker) posts quotes to two unobservable customer types.
*Inventory branch:* Ho and Stoll (1981) give a risk-averse dealer maximising
expected utility under return uncertainty and arrival uncertainty; the spread is
a risk-neutral revenue term plus risk premia, and inventory moves the midpoint,
not the spread. *Information branch:* Glosten and Milgrom (1985) show a spread
arises even when the specialist is risk neutral, competitive and earns zero
expected profit, because $A = E[V\mid\text{buy}]$ and $B = E[V\mid\text{sell}]$
bracket $E[V]$. *Strategic branch:* Kyle (1985) lets the insider plan and
internalise his own impact, yielding the linear coefficient $\lambda$ (Kyle's
lambda, reciprocal = depth). *Measurement:* Roll (1984) recovers the effective
spread from price autocovariance alone; Amihud and Mendelson (1986) show
illiquidity is priced.

**What it does not explain.** The models are one-asset, one-payoff and mostly
exogenous-flow (Kyle excepted). They say nothing about sequencing a particular
order, and they assume the fundamental is well defined even though the empirical
work they seeded finds most price movement comes from order flow, not news.

### 2. Transaction-cost analysis — the accounting

**The problem.** How is cost measured against a decision, and which benchmark is
right?

**Core method.** Define the implementation shortfall (IS) as the return gap
between a "paper" portfolio executed at the decision price and the real
portfolio, then decompose it into delay + execution (impact) + opportunity +
fees. Perold (1988) fixes the decision-price benchmark; Berkowitz, Logue and
Noser (1988) introduce VWAP; Leinweber (2002) measures IS on 13,651
institutional purchases and finds cost is *not* monotone in size, small orders
bearing a disproportionate share; Mittal (2005) argues IS is the objective
while VWAP/POV are tools, and defines opportunity cost as the dispersion of cost
versus arrival.

**What it does not explain.** It is descriptive — what you paid, not what you
should have done. It mixes a cost you control (scheduling) with one you do not
(the market's drift), and the benchmark is fragile when the decision price is
unobservable.

### 3. The stochastic-control school

**The problem.** Given a cost model, choose the schedule minimising expected cost
plus a risk penalty.

**Core method.** A controlled inventory/price process, a running objective, and
the resulting dynamic program or HJB equation, relaxed in stages. Bertsimas and
Lo (1998) set the dynamic-programming statement and prove equal-split optimal
under a linear *permanent*-impact random walk. Almgren and Chriss (2000) add
price risk, giving the hyperbolic-sine path, the half-life $1/\kappa$ and the
mean-variance frontier. Time-inconsistency is then attacked: Schied and
Schöneborn (2009) use arbitrary VNM utility, Gatheral–Schied (2011) a
time-averaged VaR, Forsyth (2011) a numerical HJB solution of the whole
frontier, Forsyth, Kennedy, Tse and Windcliff (2012) a time-consistent
mean–quadratic-variation criterion. Transient impact is endogenised: Obizhaeva
and Wang (2013) model book resilience and find the optimum is two discrete lumps
plus continuous trading; Predoiu, Shaikhet and Shreve (2011) and Alfonsi, Fruth
and Schied (2010) generalise the book shape; Guéant (2013) shows permanent
impact can be nonlinear and still arbitrage-free. Drift robustness occupies
Lorenz and Schied (2013), who show the transient-impact strategy depends on the
drift's *derivative* and may not exist, and Schied (2013), who shows that under
AC the drift enters integratively and averages out. Almgren (2003) handles
nonlinear impact and Guéant (2015) gives existence, a Hamiltonian
characterisation and block-trade pricing under general convex costs; Lehalle and
Neuman (2019) and Neuman and Voß (2022) fold in an alpha signal. Donnelly (2022)
maps exactly which assumption each paper relaxes.

**What it does not explain.** Impact parameters are inputs, not outputs;
liquidity is usually taken as given; and the objective is a weak discriminator —
Forsyth (2011) finds many schedules with nearly identical frontiers.

### 4. The market-making and quoting school

**The problem.** How should a two-sided quoter price when fills are uncertain and
inventory is risky?

**Core method.** Control the quote distance from a reference price with
exponentially decaying fill intensities $Ae^{-k\delta}$ and solve for the
reservation price and spread. Avellaneda and Stoikov (2008) give the reservation
price $r = s - q\gamma\sigma^2(T-t)$ and a spread splitting into a risk term and
a liquidity term; Guéant, Lehalle and Fernandez-Tapia (2013) add a hard inventory
bound, prove the open verification theorem, and give the GLFT closed-form spread
affine in inventory; Guéant (2017) generalises the intensity shape and extends to
correlated assets, for which Bergault, Evangelista, Guéant and Vieira (2021)
give Riccati-ODE proxies. Cartea, Jaimungal and Ricci (2014) and Cartea and Wang
(2020) add mutually-exciting flow and alpha signals; Guilbaud and Pham (2013)
model the real order book with a tick-time clock and mixed limit/market control;
Cont, Stoikov and Talreja (2010) supply the underlying queueing model.

**What it does not explain.** Fill probability is largely exogenous and quoting
is OTC-like, so queue position is not solved; and the models say nothing to a
liquidity taker — the mirror problem the execution school addresses.

### 5. The order-book and micro-execution school

**The problem.** Below the schedule, where should each child order sit — market
or limit, at which venue, at which queue position, displayed how much?

**Core method.** Reduce placement to an optimisation over a few order sizes given
a distribution of queue outflows, and treat queue position and display as
economic objects. Cont and Kukanov (2017) show the limit-versus-market split is a
convex program whose single-venue solution is a quantile of the outflow
distribution, and that deliberate overbooking across venues is optimal; Moallemi
and Yuan (2016) price queue position as fill probability times spread minus
adverse selection; Cebiroğlu and Horst (2011) characterise the optimal iceberg
display as priority impact offsetting exposure impact, while Frey and Sandås
(2009) show detected icebergs *attract* market orders; Buti–Rindi (2009) and
Moinas (2010) give the equilibrium reason to hide and its welfare effect; and
Brunnermeier and Pedersen (2005) show an exposed liquidation invites predatory
front-running. Foucault, Kadan and Kandel (2005) supply the equilibrium
limit-versus-market choice, with impatience as the sorting primitive; Easley,
López de Prado and O'Hara (2012) measure flow toxicity (VPIN) in real time; and
Abergel and Jedidi (2013) prove the book's diffusive limit.

**What it does not explain.** These are single-order or partial-equilibrium
models, and hiding is a contested detection game: HFTs use the non-display flag
for undercutting and volatility timing (Chakrabarty, Hendershott, Nawn and
Pascual, 2019), and FX order splitting is *more* market-moving, not less
(Stenfors and Susai, 2021).

### 6. The market-structure and routing school

**The problem.** How do fragmentation, fees, latency and regulation change where
an order should go?

**Core method.** A regulatory constraint set plus market-quality measurement.
Reg NMS (SEC, 2005) makes the protected-quote scan and the access-fee cap the
legal foundation of routing, and the SEC Concept Release (2010) quantifies the
fragmentation; Foucault and Menkveld (2008) prove routing automation is what
makes competing venues viable and benevolent; O'Hara and Ye (2011) find
fragmentation lowers costs on average, and Haslag and Ringgenberg (2023) show
the average hides worse outcomes for small caps. Fee models are first-order:
Comerton-Forde, Grégoire and Zhong (2017) show inverted venues improve quality;
O'Donoghue (2015) shows maker-taker can narrow the quoted spread while raising
total cost. Latency is a measured tax: Budish, Cramton and Shim (2015) propose
frequent batch auctions, and Aquilina, Budish and O'Neill (2022) price races at
roughly 0.5 basis points. Kirilenko, Kyle, Samadi and Tuzun (2017) is the
failure-mode case study; Mishra and Zhao (2021) map the routing literature.

**What it does not explain.** Routing theory is equity- and NMS-specific, and the
empirical results contradict each other across stock size, so "more venues is
better" is not a theorem. Payment for order flow (Ernst and Spatt, 2022) shows
the conflict is worse in options than in equities.

### 7. The learning and adaptation school

**The problem.** When the impact model is wrong or unknown, can a policy be
learned instead of derived?

**Core method.** Cast execution as a Markov decision process and learn the
policy. Nevmyvaka, Feng and Kearns (2006) make the first large-scale RL
application, with the action a price over the whole inventory; Hendricks and
Wilcox (2014) scale an AC schedule; Ning, Lin and Jaimungal (2018) apply Double
DQN; Lin and Beling (2020) use PPO on raw level-2 data with a
benchmark-relative reward; Karpe, Fang, Ma and Wang (2020) make placement a
first-class action, and Cheridito and Weiss (2025) give the richest joint
scheduling-and-placement action. Offline and imitation learning appear in Fang
et al. (2021), who distil a future-seeing oracle into an observable-state
student, and Li and Chen (2025), who fit a generative model to an ensemble of
classical experts. Cross-asset generalisation is the open end: Kyle and
Obizhaeva (2016) supply invariant units, and Tran and Yu (2026) find zero-shot
transfer real but modest and failing under regime shift. Macrì and Lillo (2024)
and Hambly, Xu and Yang (2023) frame the RL-versus-analytics question.

**What it does not explain.** Almost every result is simulation-trained; the
sim-to-real gap and out-of-distribution behaviour stay open, and Karpe et al.'s
agent collapses to TWAP in its own environment.

### 8. Crypto and AMM execution

**The problem.** Execution with no consolidated tape, a two-layer cost stack (a
CEX fee plus fixed gas), and perpetual futures anchored by funding.

**Core method.** Cross-venue measurement of dislocations plus convex optimisation
over the AMM trading function. Makarov and Schoar (2020) document persistent
cross-exchange deviations and Hautsch, Scheuch and Voigt (2018/2023) explain them
by settlement latency; Barbon and Ranaldo (2021/2024) formalise the CEX-versus-DEX
size threshold. Donier and Bonart (2015) confirm the square-root impact law on
Bitcoin and Cho, Norman and Nunes (2023) fit a transient, price-reverting
kernel. On perpetuals, He, Manela, Ross and von Wachter (2022/2026) give the
fee-aware funding-anchored arbitrage, Ackerer, Hugonnier and Jermann (2023) the
rigorous random-maturity pricing, Kim and Park (2025) the funding-rate design,
and Donnelly, Lin and Lorig (2026) the AC liquidation with funding. On AMMs,
Angeris, Agrawal, Evans, Chitra and Boyd (2021) make trades convex programs,
Angeris, Chitra, Evans and Boyd (2022) add fixed costs as a mixed-integer
program, and Diamandis, Resnick, Chitra and Angeris (2023) give the
dual-decomposition router; Wu, Sui, Thiery and Pai (2025) measure CEX–DEX
extracted value and Xi and Moallemi (2026) measure routing sub-optimality.

**What it does not explain.** Crypto measurement is fast-moving and
venue-specific, and the "60% fee-schedule saving" claim that circulates could not
be verified in any obtained source (see the honesty ledger).

---

## Inheritance and opposition

The schools are a genealogy, and their disagreements are as useful as their
agreements.

- **Glosten–Milgrom versus Ho–Stoll.** GM shows adverse selection is a
  *sufficient* cause of the spread, not merely a contributing one; the dealer
  models descend from Ho–Stoll.
- **Kyle versus the empirical impact literature.** Kyle's linear, permanent,
  constant-$\lambda$ impact is what the square-root-law work (Tóth et al., 2011;
  Bouchaud, 2010) contradicts — impact is concave and transient. Guéant (2013)
  reconciles them by making the microscopic permanent impact depend on volume
  already executed.
- **Bertsimas–Lo versus Almgren–Chriss.** BL minimise expected cost only and get
  equal-split; AC add a variance penalty and get the sinh path, showing naive is
  only the risk-neutral corner.
- **Almgren–Chriss versus Obizhaeva–Wang.** AC take impact as a static function;
  OW make it the dynamics of supply and demand, so the smooth AC schedule is the
  infinite-resilience special case.
- **Mean-variance versus time-consistency.** Schied–Schöneborn, Forsyth and
  Forsyth et al. all attack AC's pre-commitment problem; Forsyth shows the
  criterion barely separates schedules.
- **Static versus adaptive.** Brigo–Piat and Neuman–Voß show adaptivity pays only
  with an alpha signal; under CARA and no signal the deterministic AC path is
  optimal (Schied–Schöneborn, Corollary 4).
- **Continuous trading versus batch auctions.** Budish–Cramton–Shim call the
  race a design flaw; Menkveld (2016) counters that speed to connect fragmented
  venues is a genuine liquidity service. Both channels are real.
- **Model-based versus learned.** Macrì–Lillo find RL matches the analytic optimum
  when it exists and beats approximations only when impact is latent and price is
  in the state.

---

## The master bibliography

Every work appearing in the extraction notes for this book, with the chapter that
uses it. Status: **full** = open-access full text read; **partial** = only
official front matter, preview, sample chapters or author lecture notes
(commercial books); **none** = not obtained, reason in the "not obtained" table.

### School 1 — Microstructure foundations

| School | Author–year | Title | Key contribution | Chapter | Status |
|---|---|---|---|---|---|
| Foundations | Ho & Stoll (1981) | Optimal Dealer Pricing under Transactions and Return Uncertainty | Risk-averse dealer: spread = risk-neutral revenue term + risk premia; inventory moves the mid | 3 | full |
| Foundations | Roll (1984) | A Simple Implicit Measure of the Effective Bid–Ask Spread | Model-free, quote-free spread estimator from negative serial covariance | 3 | full |
| Foundations | Glosten & Milgrom (1985) | Bid, Ask and Transaction Prices in a Specialist Market | Adverse selection alone creates a spread; zero-profit quotes bracket $E[V]$ | 3 | full |
| Foundations | Kyle (1985) | Continuous Auctions and Insider Trading | Strategic insider gives linear permanent impact $\lambda$; $1/\lambda$ is depth | 3 | full |
| Foundations | Amihud & Mendelson (1986) | Asset Pricing and the Bid–Ask Spread | Spread is priced: expected return increases concavely with the spread | 3 | none (paywall) |
| Foundations | Wyart, Bouchaud, Farmer & Lillo (2008) | Relation between Bid–Ask Spread, Impact and Volatility | Spread linear in impact and proportional to volatility per trade | 3 | full |
| Foundations | Bouchaud, Farmer & Lillo (2009) | How Markets Slowly Digest Changes in Supply and Demand | Long-memory order flow, resilient liquidity, aggregate-impact formula | 3 | full |
| Foundations | Tóth, Lempérière, Deremble, de Lataillade, Kockelkoren & Bouchaud (2011) | Anomalous Price Impact and the Critical Nature of Liquidity | The square-root law $\Delta \propto \sigma\sqrt{Q/V}$ | 3 | full |
| Foundations | Cont, Kukanov & Stoikov (2014) | The Price Impact of Order Book Events | Short-horizon price change driven by best-quote order-flow imbalance | 3 | full |
| Foundations | Zarinelli, Treccani, Farmer & Lillo (2015) | Beyond the Square Root | Logarithmic impact fits better than square root across five decades | 3 | full |
| Foundations | Bouchaud (2010) | Price Impact (Encyclopedia of Quantitative Finance) | Concave, decaying impact; orders forecast prices rather than move them | 3, 6, 13 | full |
| Foundations | Madhavan (2000) | Market Microstructure: A Survey | Field survey; inventory and information views of the spread | 3 | full |
| Foundations | Biais, Glosten & Spatt (2005) | Market Microstructure: A Survey of Microfoundations… | Canonical survey of the field | 3 | full |
| Foundations | Bouchaud, Bonart, Donier & Gould (2018) | Trades, Quotes and Prices | Econophysics monograph of the propagator/square-root programme | 3, 5, 6, 7, 12 | partial (preview + TOC; no full draft exists) |
| Foundations | Bouchaud, Farmer & Lillo (2009) | (the 111-page arXiv review, the book's ancestor) | Author-released monograph-length theory of TQP Parts IV–VIII | 3, 6 | full |

### School 2 — Transaction-cost analysis

| School | Author–year | Title | Key contribution | Chapter | Status |
|---|---|---|---|---|---|
| TCA | Perold (1988) | The Implementation Shortfall: Paper vs. Reality | Defines IS against the decision price, paper versus real portfolio | 4 | none (paywall) |
| TCA | Berkowitz, Logue & Noser (1988) | The Total Cost of Transactions on the NYSE | Introduces VWAP as the execution-quality benchmark | 4 | none (paywall) |
| TCA | Perold & Sirri (2002) | Implementation Shortfall: A Practitioner's Guide | Practitioner guide to the IS framework | 4 | none (not located) |
| TCA | Kissell (2006) | The Expanded Implementation Shortfall | Four components: delay + execution + opportunity + fees | 4, 13 | none (paywall; book TOC + author whitepaper read) |
| TCA | Leinweber (2002) | Using Information from Trading in Trading and Portfolio Management | Large-sample IS measurement; cost not monotone in size | 4 | full |
| TCA | Mittal / ITG (2005) | Implementation Shortfall — One Objective, Many Algorithms | IS is the correct objective; opportunity cost = dispersion | 4 | full |
| TCA | Konishi & Makimoto (2001) / Konishi (2002) | Optimal slice of a block / VWAP trade | Optimal static VWAP-liquidation curve equals the relative-volume curve | 4 | none (paywall) |
| TCA | Kissell (2013/2020) | The Science of Algorithmic Trading… / Algorithmic Trading Methods | Commercial TCA mechanics; I-Star impact model | 4, 5, 13 | none (TOC + author whitepaper) |

### School 3 — Stochastic control and optimal execution

| School | Author–year | Title | Key contribution | Chapter | Status |
|---|---|---|---|---|---|
| Control | Bertsimas & Lo (1998) | Optimal Control of Execution Costs | Dynamic-programming statement; naive equal-split optimal under linear permanent impact | 4, 5 | full |
| Control | Almgren & Chriss (2000) | Optimal Execution of Portfolio Transactions | Mean-variance frontier, sinh trajectory, half-life, IS link | 4, 5 | full |
| Control | Labadie & Lehalle (2013) | Optimal Starting Times, Stopping Times and Risk Measures | Explicit Target-Close and IS recursions, start/stop times, PVol constraint | 4 | full |
| Control | Lehalle (2013) | Market Microstructure Knowledge for Intra-Day Trading | Bridge from AC schedule to microstructure; scheduling versus routing | 4, 13 | full |
| Control | Guéant & Royer (2014) | VWAP Execution and Guaranteed VWAP | Volume-tracking theorem; guaranteed-VWAP pricing | 4 | full |
| Control | Guéant (2014) | Execution and Block Trade Pricing with POV | Closed-form optimal participation rate; risk-liquidity premium | 4, 13 | full |
| Control | Frei & Westray (2015) | Optimal Execution of a VWAP Order | Closed-form VWAP tracking; TWAP-plus-adjustment split | 4, 6 | full |
| Control | Guéant, Lasry & Pu (2014) | Optimal Liquidation with Participation Constraints | Convex-duality method for hard participation caps | 4 | full |
| Control | Obizhaeva & Wang (2013) | Optimal Trading Strategy and Supply/Demand Dynamics | Optimal schedule = discrete lumps + constant interior rate under resilience | 6 | full |
| Control | Predoiu, Shaikhet & Shreve (2011) | Optimal Execution in a General One-Sided Limit-Order Book | Impact derived from arbitrary book shape; Type-A optimal strategy | 6 | full |
| Control | Alfonsi, Fruth & Schied (2010) | Optimal Execution in LOBs with General Shape Functions | Shape-independent optimal schedule; adaptive strategies | 6 | full |
| Control | Guéant (2013) | Permanent Market Impact can be Nonlinear | Volume-dependent permanent impact that is arbitrage-free | 6, 13 | full |
| Control | Guéant (2015) | Optimal Execution and Block Trade Pricing: a General Framework | Existence, Hamiltonian characterisation, block-trade pricing | 6, 13 | full |
| Control | Schied & Schöneborn (2009) | Risk Aversion and the Dynamics of Optimal Liquidation | Utility-based adaptive strategy; CARA ⇒ static is enough | 6 | full |
| Control | Lorenz & Schied (2013) | Drift Dependence under Transient Price Impact | Under transient impact the strategy depends on the drift's derivative | 6 | full |
| Control | Schied (2013) | Robust Strategies for Optimal Execution in the AC Framework | Time-consistent, closed-form, volatility-independent strategy | 6 | full |
| Control | Forsyth (2011) | A Hamilton–Jacobi–Bellman Approach to Optimal Trade Execution | Solves the whole frontier numerically; many near-optimal schedules | 6 | full |
| Control | Forsyth, Kennedy, Tse & Windcliff (2012) | Optimal Trade Execution: A Mean–Quadratic-Variation Approach | Time-consistent criterion; AC schedule is the ABM optimum | 6 | full |
| Control | Brigo & Piat (2016) | Static vs Adapted Optimal Execution Strategies | Quantifies the value of adaptivity to a signal over AC | 6 | full |
| Control | Lehalle & Neuman (2019) | Incorporating Signals into Optimal Trading | Unifies alpha signal and transient impact | 6, 7 | full |
| Control | Neuman & Voß (2022) | Optimal Signal-Adaptive Trading with Temporary and Transient Impact | Fully adaptive, time-consistent signal strategy | 6 | full |
| Control | Liu, Muhle-Karbe & Weber (2017) | Rebalancing with Linear and Quadratic Costs | Cost form determines lumpy vs continuous trading | 4 | full |
| Control | Donnelly (2022) | Optimal Execution: A Review | Map of which AC assumption each paper relaxes | 6 | full |
| Control | Almgren (2003) | Optimal Execution with Nonlinear Impact Functions | Power-law impact with a characteristic time | 6 | none (paywall; via Donnelly §4.4) |
| Control | Almgren (2009) | Optimal Trading with Stochastic Liquidity and Volatility | Plug-in heuristic for stochastic $\sigma_t, k_t$ | 6 | none (paywall; via Donnelly) |
| Control | Almgren & Lorenz (2007) | Adaptive Arrival Price | Aggressive/passive-in-the-money adaptive schedules | 6 | none (paywall) |
| Control | Gatheral & Schied (2011) | Optimal Trade Execution under GBM in the AC Framework | Time-averaged-VaR criterion; volatility-independent strategy | 6 | none (paywall; via Schied 2013) |
| Control | Gatheral (2010) | No-Dynamic-Arbitrage and Market Impact | Absence of arbitrage forces linear permanent impact | 6 | none (paywall; via Guéant 2013) |
| Control | Cartea & Jaimungal (2016) | A Closed-Form Execution Strategy to Target VWAP | Two closed-form VWAP-targeting strategies | 6 | none (404; abstract only) |
| Control | Alfonsi & Schied (2010) | Optimal Trade Execution and Absence of Price Manipulations | No price manipulation in LOB models | 6 | none (metadata only) |
| Control | Cheridito & Sepin (2014) | Optimal Trade Execution under Stochastic Volatility and Liquidity | Stochastic volatility and liquidity execution | 6 | none (wrapper only; via Donnelly) |

### School 4 — Market making and quoting

| School | Author–year | Title | Key contribution | Chapter | Status |
|---|---|---|---|---|---|
| Making | Avellaneda & Stoikov (2008) | High-Frequency Trading in a Limit Order Book | Reservation price $r = s - q\gamma\sigma^2(T-t)$; risk + liquidity spread | 7 | full |
| Making | Guéant, Lehalle & Fernandez-Tapia (2013) | Dealing with the Inventory Risk | Hard inventory bound; verification theorem; GLFT closed form | 7 | full |
| Making | Guéant (2017) | Optimal Market Making | General intensity shape; CARA = running-penalty; multi-asset | 7 | full |
| Making | Bergault, Evangelista, Guéant & Vieira (2021) | Closed-Form Approximations in Multi-Asset Market Making | Riccati-ODE proxies for correlated-asset quoting | 7 | full |
| Making | Cartea, Jaimungal & Ricci (2014) | Buy Low, Sell High: A High-Frequency Trading Perspective | Hawkes self/mutually-exciting flow; signal-driven quotes | 7 | full |
| Making | Cartea & Wang (2020) | Market Making with Alpha Signals | Jump mid-price; alpha flips limit and speculative market orders | 7 | full |
| Making | Guilbaud & Pham (2013) | Optimal High Frequency Trading with Limit and Market Orders | Real LOB: tick-time clock, price priority, mixed control | 7 | full |
| Making | Cont, Stoikov & Talreja (2010) | A Stochastic Model for Order Book Dynamics | Queueing model; closed-form fill and move probabilities | 7 | full |
| Making | Fodra & Labadie (2012) | HF Market Making with Inventory Constraints and Directional Bets | Non-martingale mid-price; risk aversion tunes the P&L distribution | 7 | full |
| Making | Drissi (2024) | Lecture Notes on Market Microstructure and Algorithmic Trading | Oxford course bridging AS/GLFT and Cartea–Jaimungal conventions | 7 | full |
| Making | Cartea, Jaimungal & Penalva (2015) | Algorithmic and High-Frequency Trading | Monograph: exponential-utility execution and market making | 5, 6, 7, 10 | partial (preview + excerpt) |

### School 5 — Order-book micro-execution, placement and stealth

| School | Author–year | Title | Key contribution | Chapter | Status |
|---|---|---|---|---|---|
| Micro | Cont & Kukanov (2017) | Optimal Order Placement in Limit Order Markets | Convex split = quantile of queue outflow; overbooking is optimal | 8 | full |
| Micro | Cartea & Jaimungal (2016) | Incorporating Order-Flow into Optimal Execution | AC schedule plus a weighted average of expected future net flow | 5, 8 | full |
| Micro | Abergel & Jedidi (2013) | A Mathematical Approach to Order Book Modeling | Ergodicity and a diffusive limit for the Markovian book | 8 | full |
| Micro | Moallemi & Yuan (2016) | A Model for Queue Position Valuation in a Limit Order Book | Queue value = fill probability × spread − adverse selection | 8 | full |
| Micro | Cebiroğlu & Horst (2011) | Optimal Display of Iceberg Orders | Optimal display offsets priority against exposure impact | 8 | full |
| Micro | Frey & Sandås (2009) | The Impact of Iceberg Orders in Limit Order Books | Detected icebergs attract market orders; hidden liquidity is uninformed | 8 | full |
| Micro | Buti & Rindi (2009) | Hidden Orders and Optimal Submission Strategies | Hidden orders are equilibrium strategies against picking-off risk | 8 | full |
| Micro | Moinas (2010) | Hidden Limit Orders and Liquidity in Order-Driven Markets | Opacity improves liquidity and welfare under asymmetry | 8 | full |
| Micro | Foucault, Kadan & Kandel (2005) | Limit Order Book as a Market for Liquidity | Impatience sorts traders into liquidity supply and demand | 8 | full |
| Micro | Brunnermeier & Pedersen (2005) | Predatory Trading | Exposed liquidation invites front-running; argues against disclosure | 8 | full |
| Micro | Zhu (2014) | Do Dark Pools Harm Price Discovery? | Dark pools need not harm price discovery | 8 | full |
| Micro | Easley, López de Prado & O'Hara (2012) | Flow Toxicity and Liquidity in a High-Frequency World | VPIN: real-time, volume-time measure of order-flow toxicity | 8 | full |
| Micro | Chakrabarty, Hendershott, Nawn & Pascual (2019) | Order Exposure in High-Frequency Markets | HFTs use the non-display flag for undercutting and volatility timing | 8 | full |
| Micro | Degryse, Tombeur & Wuyts (2015) | Two Shades of Opacity | Hidden-order and dark trading are imperfect substitutes | 8 | full |
| Micro | Stenfors & Susai (2021) | Stealth Trading in FX Markets | Order splitting is detectable and more market-moving | 8 | full |
| Micro | Esser & Mönch (2007) | The Navigation of an Iceberg | Optimal use of hidden orders | 8 | none (503/429) |
| Micro | Cartea & Jaimungal (2015) | Optimal Execution with Limit and Market Orders | Optimal limit/market mix (companion to the 2016 paper) | 5, 8 | none (paywall) |
| Micro | Yang & Zhu (2020) | Back-Running: Seeking and Hiding Fundamental Information | Order anticipation / back-running | 8 | none (403) |
| Micro | Degryse, de Jong & van Kervel (2015) | The Impact of Dark Trading and Visible Fragmentation | Dark trading and visible fragmentation effects | 8 | none (timeout) |
| Micro | Hasbrouck (2007) | Empirical Market Microstructure | Textbook; Roll model, VAR decomposition, information shares | 3, 8, 9 | partial (author NYU text + samples) |

### School 6 — Market structure and routing

| School | Author–year | Title | Key contribution | Chapter | Status |
|---|---|---|---|---|---|
| Routing | SEC (2005) | Regulation NMS (Final Rule) | Order Protection, Access, Sub-Penny rules; the SOR constraint set | 9 | full |
| Routing | SEC (2010) | Concept Release on Equity Market Structure | Official snapshot of fragmentation; displayed vs undisplayed | 9 | full |
| Routing | SEC Staff (2013) | Equity Market Structure Literature Review I: Fragmentation | Visible vs dark fragmentation; benefits taper for small caps | 9 | full |
| Routing | SEC Staff (2014) | Equity Market Structure Literature Review II: HFT | HFT as the de-facto liquidity supplier | 9 | full |
| Routing | Foucault & Menkveld (2008) | Competition for Order Flow and Smart Order Routing Systems | SOR adoption makes venues viable and lowers consolidated cost | 9 | full |
| Routing | Hendershott, Jones & Menkveld (2011) | Does Algorithmic Trading Improve Liquidity? | Autoquote experiment: AT narrows spreads via less adverse selection | 9 | full |
| Routing | Kirilenko, Kyle, Samadi & Tuzun (2017) | The Flash Crash: HFT in an Electronic Market | Failure mode: price-insensitive order met vanishing depth | 9, 12 | full |
| Routing | Budish, Cramton & Shim (2015) | The HFT Arms Race: Frequent Batch Auctions | Continuous LOB is a flawed design; batch auctions | 9 | full |
| Routing | Menkveld (2016) | The Economics of High-Frequency Trading: Taking Stock | Data-weighted review; speed to connect venues is a service | 9 | full |
| Routing | Aquilina, Budish & O'Neill (2022) | Quantifying the HFT "Arms Race" | Races ~1/min, 5–10 µs, ≈0.5 bp tax, 17% of liquidity cost | 9, 13 | full |
| Routing | Mishra & Zhao (2021) | Order Routing Decisions for a Fragmented Market: A Review | Map of SOR determinants; fee schedules by venue | 9 | full |
| Routing | Comerton-Forde, Grégoire & Zhong (2017) | Inverted Fee Venues and Market Quality | Fee model is a first-order routing variable | 9 | full |
| Routing | O'Donoghue (2015) | The Effect of Maker-Taker Fees on Order Choice | Maker-taker can narrow the spread while raising total cost | 9 | full |
| Routing | Ernst & Spatt (2022) | Payment for Order Flow and Asset Choice | PFOF small in equities, large in options | 9 | full |
| Routing | Haslag & Ringgenberg (2023) | The Demise of the NYSE and Nasdaq | Fragmentation helps large caps, hurts small caps | 9 | full |
| Routing | SEC (2022) | Order Competition Rule (Proposed) | Forces retail marketable orders into a public auction | 9 | full |
| Routing | O'Hara & Ye (2011) | Is Market Fragmentation Harming Market Quality? | Fragmentation lowers costs on average and speeds execution | 9 | none (paywall) |
| Routing | Johnson (2010) | Algorithmic Trading & DMA | DMA/execution-method taxonomy and infrastructure | 8, 13 | none (TOC only; site dead) |

### School 7 — Learning and adaptation

| School | Author–year | Title | Key contribution | Chapter | Status |
|---|---|---|---|---|---|
| Learning | Nevmyvaka, Feng & Kearns (2006) | Reinforcement Learning for Optimized Trade Execution | First large-scale RL; action is a price over the whole inventory | 10 | full |
| Learning | Azencott, Beri, Gadhyan, Joseph, Lehalle & Rowley (2013) | Realtime Market Microstructure Analysis: Online TCA | Closed-loop, cross-sectional TCA; anomaly detectors | 10 | full |
| Learning | Hendricks & Wilcox (2014) | A RL Extension to the Almgren–Chriss Framework | RL scales an AC schedule; median IS improves up to 10.3% | 10 | full |
| Learning | Kyle & Obizhaeva (2016) | Market Microstructure Invariance: Empirical Hypotheses | Invariant units in business time; square root is unique | 3, 10 | full |
| Learning | Ning, Lin & Jaimungal (2018/2021) | Double Deep Q-Learning for Optimal Execution | DQN execution beats TWAP on 7 of 9 names | 10 | full |
| Learning | Karpe, Fang, Ma & Wang (2020) | Multi-Agent RL in a Realistic LOB Simulation | First joint execution × placement action; collapses to TWAP | 10 | full |
| Learning | Lin & Beling (2020) | An End-to-End Optimal Trade Execution Framework Based on PPO | Raw level-2 state, sparse benchmark-relative reward | 10 | full |
| Learning | Fang et al. (2021) | Universal Trading for Order Execution with Oracle Policy Distillation | Imitation from a future-seeing oracle; one policy, many instruments | 10 | full |
| Learning | Pan, Zhang, Luo, He & Liu (2022) | Learn Continuously, Act Discretely (HALOP) | Hybrid continuous-scope + discrete-tick limit placement | 10 | full |
| Learning | Hambly, Xu & Yang (2023) | Recent Advances in RL in Finance | Survey: MDP formalism and six applications | 10 | full |
| Learning | Nagy, Calliess & Zohren (2023) | Asynchronous Deep Double Dueling Q-Learning | RL turns an alpha signal into per-order placements with a skip action | 7, 10 | full |
| Learning | Macrì & Lillo (2024) | RL for Optimal Execution when Liquidity is Time-Varying | Latent time-varying impact; RL beats AC only with price in the state | 10 | full |
| Learning | Micheli & Monod (2024) | Deep RL for Online Optimal Execution Strategies | DDPG with past-action history handles transient non-Markovian impact | 10 | full |
| Learning | Zhou, Chen & Xu (2024) | Two Kinds of Learning Algorithms for Continuous-Time VWAP Targeting | Explicit Gaussian optimal policy; ADP needs a known environment | 10 | full |
| Learning | Li & Chen (2025) | FlowOE: Imitation Learning with Flow Matching | Imitation over classical experts; beats them under concave impact | 10 | full |
| Learning | Olby, Bacalum, Baggott & Stillman (2025) | Right Place, Right Time: Market-Simulation-Based RL | Reactive-simulator RL reaches the AC frontier | 10 | full |
| Learning | Cheridito & Weiss (2025) | RL for Trade Execution with Market and Limit Orders | Richest joint state/action: simplex over fractions, levels, cancels | 10 | full |
| Learning | Tran & Yu (2026) | Does a Trading Policy Need to Know What It Trades? | Zero-shot transfer is modest and fails under regime shift | 10 | full |
| Learning | Kolm & Ritter (2019) | Dynamic Replication and Hedging: A Reinforcement Learning Approach | RL hedging under frictions | 10 | none (paywall) |

### School 8 — Crypto and AMM execution

| School | Author–year | Title | Key contribution | Chapter | Status |
|---|---|---|---|---|---|
| Crypto | Makarov & Schoar (2020) | Trading and Arbitrage in Cryptocurrency Markets | Persistent cross-exchange deviations; ~USD 2bn/day at the peak | 11 | full |
| Crypto | Hautsch, Scheuch & Voigt (2018/2023) | Building Trust Takes Time | Settlement latency explains limits to arbitrage | 11 | full |
| Crypto | Barbon & Ranaldo (2021/2024) | On the Quality of Cryptocurrency Markets | Two-layer cost stack; CEX cheaper small, DEX cheaper large | 11 | full |
| Crypto | Wu, Sui, Thiery & Pai (2025) | Measuring CEX-DEX Extracted Value | USD 233.8m by 19 searchers; top searchers take three-quarters | 11 | full |
| Crypto | Plazuelo Pascual et al. (2025) | Price Discovery in Cryptocurrency Markets | Which venue is the efficient-price reference | 11 | full |
| Crypto | Aleti & Mizrach (2021) | Bitcoin Spot and Futures Market Microstructure | BTC spot/CME spread and impact baseline | 11 | none (paywall; abstract only) |
| Crypto | Agentic Sciences (2023/2026) | Order Book Microstructure of Bitcoin: Spot vs Perpetual | Perp has 3.1× depth but a wider quoted spread than spot | 11 | full |
| Crypto | Easley, O'Hara, Yang & Zhang (2024) | Microstructure and Market Dynamics in Crypto Markets | Roll and VPIN predict crypto dynamics; stable through crypto winter | 11 | full |
| Crypto | Donier & Bonart (2015) | A Million Metaorder Analysis of Market Impact on the Bitcoin | Square-root law on four decades; impact decays | 11, 13 | full |
| Crypto | Cho, Norman & Nunes (2023) | PRIME: A Price-Reverting Impact Model | Transient, price-reverting BTC impact kernel | 11 | full |
| Crypto | He, Manela, Ross & von Wachter (2022/2026) | Fundamentals of Perpetual Futures | Fee-aware, funding-anchored arbitrage bound; Sharpe 3.35 to 11.65 | 11, 13 | full |
| Crypto | Ackerer, Hugonnier & Jermann (2023) | Perpetual Futures Pricing | Perpetual price = risk-neutral spot at a random time of mean $1/\kappa$ | 11 | full |
| Crypto | Kim & Park (2025) | Designing Funding Rates for Perpetual Futures | Path-dependent funding designs via backward SDEs | 11 | full |
| Crypto | Donnelly, Lin & Lorig (2026) | Optimal Liquidation of Perpetual Contracts | AC liquidation with funding; closed form and perturbation | 11 | full |
| Crypto | Angeris, Agrawal, Evans, Chitra & Boyd (2021) | Constant Function Market Makers: Multi-Asset Trades via Convex Optimization | CFMM trades are convex programs; duals are prices | 11 | full |
| Crypto | Angeris, Chitra, Evans & Boyd (2022) | Optimal Routing for Constant Function Market Makers | Routing convex without fixed costs, a MICP with them | 11 | full |
| Crypto | Diamandis, Resnick, Chitra & Angeris (2023) | An Efficient Algorithm for Optimal Routing Through CFMMs | Dual-decomposition router; handles v3 and multiple fee tiers | 11 | full |
| Crypto | Xi & Moallemi (2026) | Quantifying Sub-Optimality in Routing for AMs | DEX routing loses 2.02 bps/trade; mostly state freshness | 11 | full |
| Crypto | Zhang & Tessone (2025) | Measuring DEX Efficiency and the Effect of an Enhanced Routing Method | Standardised total arbitrage profit as a venue-quality measure | 11 | full |
| Crypto | Cartea, Drissi & Monga (2023/2025) | DeFi and Automated Market Making: Execution and Speculation | AMM analogue of AC; stochastic convexity cost; CEX–DEX co-optimisation | 11 | full |
| Crypto | Cartea, Drissi & Monga (2023/2024) | DeFi and AMM: Predictable Loss and Optimal Liquidity Provision | Predictable loss (CL LVR); optimal range width | 11 | full |
| Crypto | Lehar, Parlour & Zoican (2023/2024) | Fragmentation and Optimal Liquidity Supply on DEXs | High-fee pools take 58% of liquidity, 21% of volume | 11 | full |
| Crypto | Canidio & Fritsch (2023/2025) | Arbitrageurs' Profits, LVR and Sandwich Attacks | Batch trading (FM-AMM) removes LVR and sandwiches | 11 | full |
| Crypto | Kulkarni, Diamandis & Chitra (2022/2023) | Towards a Theory of MEV I: CFMMs | Constant price of anarchy; $O(\log n)$ sandwich damage | 11 | full |
| Crypto | Di Nosse & Lillo (2026) | Mitigating Adverse Selection in CL AMMs with Dynamic Fees | Dynamic fees compensate LVR more than reduce it | 11 | full |
| Crypto | Barone & Lillo (2026) | Trading in the Sunshine or in the Shade (Hyperliquid) | Announcing a TWAP lowers own impact ~9 bps and adverse selection ~5 bps | 11 | full |
| Crypto | Binance (2026) | Fees & Transactions Overview (Spot & Margin) | Full tiered maker/taker fee schedule | 11 | full (official doc) |

---

## Three reading paths

### Path A — building intuition (one to two weeks)

1. **Almgren–Chriss (2000), Sections 1–3.** The sinh path, the half-life and the
   frontier. *You should be able to explain why trading faster raises impact but
   lowers risk, and to draw the frontier.*
2. **Bertsimas–Lo (1998), Sections 2–3.** *Why is equal-split optimal only under
   linear permanent impact?*
3. **Kyle (1985) and Glosten–Milgrom (1985).** *Why does the spread exist with
   zero fees? What is Kyle's lambda and why is its reciprocal called depth?*
4. **Bouchaud (2010) and Tóth et al. (2011).** *What is the square-root law, and
   why must impact be transient rather than permanent?*
5. **Cont–Kukanov (2017).** *Why is the limit-versus-market split a quantile of
   the queue outflow?*

Skip AC and every later schedule is opaque. Skip Kyle/GM and you will misread
each "impact term" as a fee. Skip Bouchaud/Tóth and you will calibrate a linear
impact model the data reject. Skip Cont–Kukanov and placement looks like
engineering rather than optimisation.

### Path B — building a working execution system

**Theory.** AC (2000) for the schedule; Obizhaeva–Wang (2013) to know when the
schedule must contain discrete trades; Guéant (2015) for general convex costs,
participation caps and block-trade pricing; Donnelly (2022) for which assumption
each extension relaxes. For VWAP and POV benchmarks add Guéant–Royer (2014),
Frei–Westray (2015) and Guéant (2014).

**Calibration.** Bouchaud (2010), Tóth et al. (2011) and Zarinelli et al. (2015)
for the impact law — square root as default, the logarithmic form checked at
large participation; Kyle–Obizhaeva (2016) for cross-asset normalisation; Cont,
Kukanov and Stoikov (2014) for the imbalance-to-impact slope; Labadie–Lehalle
(2013) for explicit IS and Target-Close recursions with a PVol cap. In crypto add
Donier–Bonart (2015) and Cho et al. (2023).

**Engineering.** Cont–Kukanov (2017) and Moallemi–Yuan (2016) for placement and
queue value; Foucault–Menkveld (2008), Comerton-Forde et al. (2017) and
O'Donoghue (2015) for venue and fee logic; Aquilina–Budish–O'Neill (2022) to
price the latency you cannot win; Hasbrouck's NYU text (Chapters 16–17, 19) for
TCA and order-splitting mechanics; Azencott et al. (2013) for closed-loop TCA.

Skip Obizhaeva–Wang and your algorithm never takes the opening or closing block
that resilience makes optimal. Skip the calibration papers and your $\eta$ is a
guess. Skip the fee-model papers and your router optimises the quoted price while
paying the wrong total cost.

### Path C — doing research

Enter through **Donnelly (2022)** for optimal execution and **Hambly, Xu and Yang
(2023)** for learning; both fix the state of the art and its open ends. Then take
**Guéant (2015)** and **Donnelly (2022) §4** as the analytic spine and pick one
live seam: signals and execution (Lehalle–Neuman, 2019; Neuman–Voß, 2022);
transient impact and robustness (Lorenz–Schied, 2013; Schied, 2013; Predoiu–
Shaikhet–Shreve, 2011); joint scheduling and placement (Cheridito–Weiss, 2025;
Cont–Kukanov, 2017); generalisation (Kyle–Obizhaeva, 2016; Tran–Yu, 2026); or
crypto (Cartea–Drissi–Monga, 2023; Xi–Moallemi, 2026; Barone–Lillo, 2026).

Skip the surveys and you re-derive a known result; skip the specific seam paper
and you will present as open a problem closed in 2022.

---

## Open problems

Items marked *chapter judgement* are this chapter's own reading, not a source's.

1. **Robust optimality under nonlinear and transient impact.** The AC schedule is
   exactly optimal only under linear impact and a martingale price; under
   transient impact the strategy depends on the drift's derivative and may not
   exist (Lorenz–Schied, 2013). *Why open:* the arbitrage-free permanent impact is
   nonlinear (Guéant, 2013) and the decay kernel is estimated, not known. Closest:
   Predoiu–Shaikhet–Shreve (2011); Guéant (2015).
2. **Joint optimisation of signal and execution.** Static-signal solutions
   (Lehalle–Neuman, 2019) use the signal only at time 0; adaptive ones
   (Neuman–Voß, 2022) need a non-degeneracy condition. *Why open:* the interaction
   of a decaying alpha with a transient-impact distortion is unsolved in general.
   Closest: Neuman–Voß (2022); Cartea–Jaimungal (2016).
3. **Out-of-distribution generalisation and the sim-to-real gap in RL.** Policies
   are trained in replay or synthetic environments; Karpe et al.'s agent collapses
   to TWAP and invariant units buy only modest transfer (Tran–Yu, 2026). *Why
   open:* no accepted OOD protocol exists. Closest: Tran–Yu (2026).
4. **Joint optimisation of the crypto fee table and funding.** Funding timing and
   tiered maker/taker fees are modelled separately (He et al., 2022/2026;
   Donnelly et al., 2026); which tier to target, maker versus taker, and when to
   hold versus unwind a funding position are not one problem. Closest: He et al.
   (2022/2026); Kim–Park (2025).
5. **Optimal decomposition of cross-venue routing under latency and fees.**
   Routing is convex without fixed costs and a MICP with them (Angeris et al.,
   2022); measured sub-optimality is small and mostly a state-freshness problem
   (Xi–Moallemi, 2026). *Why open:* the latency-fee-inclusion trade-off is a
   control problem over the settlement window, not a static program. Closest:
   Xi–Moallemi (2026); Hautsch et al. (2018/2023).
6. **The unobservable information half-life.** Execution and adverse selection
   depend on how fast private information decays into price, and that half-life
   is latent. Roll (1984) and Kyle (1985) take it as given, and Macrì–Lillo (2024)
   infer latent impact but not the half-life. Closest: Macrì–Lillo (2024); Easley
   et al. (2012).
7. **Reconciling latency competition with its social value.** Batch auctions
   would remove the race (Budish et al., 2015) yet speed to connect fragmented
   venues is a real service (Menkveld, 2016). *Why open:* the two channels are
   measured separately and never netted in one counterfactual. Closest: Aquilina–
   Budish–O'Neill (2022).
8. **Display versus concealment as one optimised portfolio.** Display size trades
   priority against exposure (Cebiroğlu–Horst, 2011), hidden and dark trading
   substitute (Degryse et al., 2015), and announcing a TWAP lowers cost
   (Barone–Lillo, 2026). *Why open:* no model optimises order type, display size,
   splitting and venue jointly with the adversary's detection response. Closest:
   Barone–Lillo (2026); Buti–Rindi (2009). *chapter judgement.*

---

## The honesty ledger: how these sources were obtained

**Read in full (open access).** The large majority: every paper in Schools 3, 5,
6, 7 and 8 marked "full" — Almgren–Chriss, Bertsimas–Lo, Obizhaeva–Wang,
Predoiu–Shaikhet–Shreve, Alfonsi–Fruth–Schied, the Guéant papers, Schied and
Lorenz–Schied, Forsyth, Brigo–Piat, Lehalle–Neuman, Neuman–Voß, Donnelly,
Kyle–Obizhaeva, all the RL papers except Kolm–Ritter, and the crypto papers.
A further small set of empirical impact papers (Wyart et al. 2008; Madhavan
2000; Biais-Glosten-Spatt 2005; Tóth et al. 2011; Cont-Kukanov-Stoikov 2014;
Zarinelli et al. 2015) is held in the local corpus and used by Chapters 3 and 13;
they are catalogued here but are not separately extracted in the cluster note
files.

**Commercial books not obtained, and the substitutes used.**

- **O'Hara, *Market Microstructure Theory* (1995).** No lawful full text exists.
  Substitutes: the publisher's full 9-chapter table of contents and O'Hara's own
  2015 *Journal of Financial Economics* survey, which restates the inventory /
  information / strategic triad and adds speed as a fourth cost.
- **Hasbrouck, *Empirical Market Microstructure* (2007).** Substitutes: the
  author's own 202-page, 22-chapter NYU teaching text *Securities Trading:
  Principles and Procedures* (v14c, 2025); the publisher's two free sample
  chapters; the author's corrigenda; and the 21-page preview with the 2007 TOC.
- **Harris, *Trading and Exchanges* (2003).** Substitutes: the publisher's
  66-page preview (complete 29-chapter TOC, Chapters 1–2 in full) and the
  author's free CFA Institute Research Foundation volume *Trading and Electronic
  Markets* (2015). A 2002 pre-publication draft is hosted on a third-party course
  page but is not a rights-holder release and was not used.
- **Cartea–Jaimungal–Penalva, *Algorithmic and High-Frequency Trading* (2015).**
  Publisher's preview and excerpt only; the execution and market-making content
  is substituted by the authors' own arXiv and ORA papers and Drissi's Oxford
  notes.
- **Guéant, *The Financial Mathematics of Market Liquidity* (2016).** Substituted
  by the author's two complete lecture courses (422 slides, CFM–Imperial 2016,
  covering Parts I–II; 578 slides, KAUST 2021, the market-making half) plus the
  publisher's preview.
- **Lehalle–Laruelle, *Market Microstructure in Practice* (2013/2018).**
  Substituted by the authors' own IPAM/UCLA 2015 lecture series (7-slide overview,
  65-slide lecture).
- **Kissell, *The Science of Algorithmic Trading…* (2013) / *Algorithmic Trading
  Methods* (2020).** Publisher front matter (20-chapter TOC) and the author's own
  2015 multi-asset cost-model whitepaper containing the I-Star model.
- **Bouchaud, Bonart, Donier & Gould, *Trades, Quotes and Prices* (2018).**
  Chapters 3–24 not obtained; the check confirmed **no full draft exists
  publicly**. Substitutes: the 47-page preview (Chapters 1–2 full) and TOC, and
  the authors' own 111-page arXiv monograph "How markets slowly digest changes in
  supply and demand".
- **Johnson, *Algorithmic Trading & DMA* (2010).** Only its table of contents was
  obtained — the author's only channel, `algotradingdma.com`, is dead and no
  deposit or free release exists. This is the one work for which nothing but the
  TOC was obtained.

**Methodological statement.** Every statement traces to a document actually read.
Nothing is described from memory about any work not obtained: works marked "none"
contribute only what their abstracts, publishers' front matter, or the citing
papers that *were* read attribute to them.

**The unverified claim.** The statement that optimising against the *complete fee
schedule* yields "over 60% potential savings" could not be verified. Every
obtained source was searched; none reports a saving above 60% from fee-schedule
optimisation. The adjacent 60% figures mean other things (an annualised basis
deviation, a noise-trading share, sample coverage), and the closest measured
execution saving is Xi–Moallemi's 2.02 basis points of routing sub-optimality.
The figure is unsourced until a specific work is named, and this chapter does not
repeat it as fact. (The same finding is recorded in Chapter 11's section of the
same name.)

---

## Works not obtained

| Author–year | Title | What was tried | Why not obtained | Where a reader can look |
|---|---|---|---|---|
| Amihud & Mendelson (1986) | Asset Pricing and the Bid-Ask Spread | Unpaywall, NYU course URL, RePEc, Rochester WP | Publisher link returns a bot page; no repository copy | ScienceDirect (subscription); abstract via RePEc/IDEAS |
| Perold (1988) | The Implementation Shortfall: Paper vs. Reality | HBS faculty page, general search | *Journal of Portfolio Management* subscription-only | HBS faculty page (abstract); library copy of JPM 14(3) |
| Berkowitz, Logue & Noser (1988) | The Total Cost of Transactions on the NYSE | Publisher, repository searches | Wiley *Journal of Finance* subscription-only | Library copy of JF 43(1) |
| Perold & Sirri (2002) | Implementation Shortfall: A Practitioner's Guide | Search | Not located as an open PDF | Publisher/handbook volume |
| Konishi & Makimoto (2001) / Konishi (2002) | Optimal slice of a block / of a VWAP trade | Elsevier, Risk.net | Subscription-only | Library copy of *Journal of Risk* 3(4) / *JFM* 5 |
| Kissell (2006) | The Expanded Implementation Shortfall | Author site, journals | *Journal of Trading* paywalled | CFA curriculum treatments; Kissell's books |
| Kissell (2013/2020) | The Science of Algorithmic Trading… / Algorithmic Trading Methods | Publisher, author site | Elsevier releases no free chapters; only front matter and whitepapers | Publisher pages; library copy |
| Almgren (2003) | Optimal Execution with Nonlinear Impact Functions | Taylor & Francis, author pages | Publisher paywall; author pages 403/404 | *Applied Mathematical Finance* 10(1); Donnelly (2022) §4.4 |
| Almgren (2009) | Optimal Trading with Stochastic Liquidity and Volatility | SIAM, preprints | SIAM paywall; no open preprint | *SIAM J. Financial Mathematics* 3(1); Donnelly (2022) |
| Almgren & Lorenz (2007) | Adaptive Arrival Price | Institutional Investor / PM Research | Paywalled | *Journal of Trading* 2(1) |
| Gatheral & Schied (2011) | Optimal Trade Execution under GBM in the AC Framework | World Scientific, arXiv, author pages | Paywalled; not on arXiv | *IJTAF* 14(3); quoted in Schied (2013) |
| Gatheral (2010) | No-Dynamic-Arbitrage and Market Impact | Quantitative Finance, SSRN | Paywalled; SSRN blocks automated retrieval | *Quantitative Finance* 10(7); restated in Guéant (2013) |
| Cartea & Jaimungal (2016) | A Closed-Form Execution Strategy to Target VWAP | Oxford ORA, arXiv | ORA endpoint 404s; no arXiv version | Oxford ORA record; *SIAM J. Financial Mathematics* 7(1) |
| Alfonsi & Schied (2010) | Optimal Trade Execution and Absence of Price Manipulations | HAL, SSRN | HAL metadata only; SSRN blocked | HAL `hal-00397652`; *SIAM J. Financial Mathematics* 1(1) |
| Cheridito & Sepin (2014) | Optimal Trade Execution under Stochastic Volatility and Liquidity | Princeton OAR | Endpoint returned an HTML wrapper | Preprint servers; Donnelly (2022) §4.1 |
| Esser & Mönch (2007) | The Navigation of an Iceberg | EFMA 2005 copy, Wayback | Host 503; Wayback rate-limited (429) | *Finance Research Letters* 4(2) |
| Cartea & Jaimungal (2015) | Optimal Execution with Limit and Market Orders | arXiv, publisher | No open preprint; publisher paywall | *Quantitative Finance* 15(8); use the 2016 companion |
| Yang & Zhu (2020) | Back-Running | MIT author page, SSRN | MIT page 403; SSRN gated | *Review of Financial Studies* 33(4) |
| Degryse, de Jong & van Kervel (2015) | The Impact of Dark Trading and Visible Fragmentation | Tilburg, CEPR | Hosts timed out / returned HTML | *Review of Finance* 19(4) |
| O'Hara & Ye (2011) | Is Market Fragmentation Harming Market Quality? | ScienceDirect, SSRN, Columbia Law | Paywalled; SSRN auth; Columbia host dead; Wayback 429 | *JFE* 100(3); summarised in SEC Staff (2013) |
| Kolm & Ritter (2019) | Dynamic Replication and Hedging | Journal, author slides, SSRN | *JFD* paywalled; only slides and a companion survey open | *Journal of Financial Data Science* 1(1) |
| Aleti & Mizrach (2021) | Bitcoin Spot and Futures Market Microstructure | Wiley, SSRN, repositories | Wiley paywall; SSRN login wall | *Journal of Futures Markets* 41(2); SSRN abstract |
| Johnson (2010) | Algorithmic Trading & DMA | Publisher site, Wayback, searches | Self-published; the only channel is dead | Library/used copy; the TOC is in the local corpus |

---

## Sources

The works this chapter drew on directly, in addition to those catalogued above.

- Albert S. Kyle, "Continuous Auctions and Insider Trading", *Econometrica* 53(6), 1315–1336 (1985).
- Lawrence R. Glosten and Paul R. Milgrom, "Bid, Ask and Transaction Prices in a Specialist Market", *Journal of Financial Economics* 14(1), 71–100 (1985).
- Thomas Ho and Hans R. Stoll, "Optimal Dealer Pricing under Transactions and Return Uncertainty", *Journal of Financial Economics* 9(1), 47–73 (1981).
- Richard Roll, "A Simple Implicit Measure of the Effective Bid-Ask Spread", *Journal of Finance* 39(4), 1127–1139 (1984).
- Albert S. Kyle and Anna A. Obizhaeva, "Market Microstructure Invariance: Empirical Hypotheses", *Econometrica* 84(4), 1345–1404 (2016).
- Robert Almgren and Neil Chriss, "Optimal Execution of Portfolio Transactions", *Journal of Risk* 3(2), 5–39 (2001). [preprint](https://www.smallake.kr/wp-content/uploads/2016/03/optliq.pdf)
- Dimitris Bertsimas and Andrew W. Lo, "Optimal Control of Execution Costs", *Journal of Financial Markets* 1(1), 1–50 (1998).
- Anna Obizhaeva and Jiang Wang, "Optimal Trading Strategy and Supply/Demand Dynamics", *Journal of Financial Markets* 16(1), 1–32 (2013). [NBER wp](https://www.nber.org/system/files/working_papers/w11444/w11444.pdf)
- Olivier Guéant, "Optimal Execution and Block Trade Pricing: a General Framework", *Applied Mathematical Finance* 22(4), 336–365 (2015). [arXiv:1210.6372](https://arxiv.org/abs/1210.6372)
- Ryan Donnelly, "Optimal Execution: A Review", *Applied Mathematical Finance* 29(3), 181–212 (2022).
- Marco Avellaneda and Sasha Stoikov, "High-frequency trading in a limit order book", *Quantitative Finance* 8(3), 217–224 (2008).
- Olivier Guéant, Charles-Albert Lehalle and Joaquin Fernandez-Tapia, "Dealing with the inventory risk", *Mathematics and Financial Economics* 7(4), 477–507 (2013). [arXiv:1105.3115](https://arxiv.org/abs/1105.3115)
- Rama Cont and Arseniy Kukanov, "Optimal order placement in limit order markets", *Quantitative Finance* 17(1), 21–39 (2017). [arXiv:1210.1625](https://arxiv.org/abs/1210.1625)
- Thierry Foucault and Albert J. Menkveld, "Competition for Order Flow and Smart Order Routing Systems", *Journal of Finance* 63(1), 119–158 (2008).
- Eric Budish, Peter Cramton and John Shim, "The High-Frequency Trading Arms Race", *Quarterly Journal of Economics* 130(4), 1547–1621 (2015).
- Matteo Aquilina, Eric Budish and Peter O'Neill, "Quantifying the High-Frequency Trading 'Arms Race'", *Quarterly Journal of Economics* 137(1), 493–564 (2022).
- Yuriy Nevmyvaka, Yi Feng and Michael Kearns, "Reinforcement Learning for Optimized Trade Execution", *ICML 2006*, 673–680.
- Ben Hambly, Renyuan Xu and Huining Yang, "Recent Advances in Reinforcement Learning in Finance", *Mathematical Finance* 33(3), 437–503 (2023). [arXiv:2112.04553](https://arxiv.org/abs/2112.04553)
- Patrick Cheridito and Moritz Weiss, "Reinforcement Learning for Trade Execution with Market and Limit Orders", [arXiv:2507.06345](https://arxiv.org/abs/2507.06345) (2025).
- Hieu Tran and Hong Yu, "Does a Trading Policy Need to Know What It Trades?", [arXiv:2611.08701](https://arxiv.org/abs/2611.08701) (2026).
- Guillermo Angeris et al., "Constant Function Market Makers: Multi-Asset Trades via Convex Optimization", *Proceedings of the IEEE* (2022). [arXiv:2107.12484](https://arxiv.org/abs/2107.12484)
- Weiye Xi and Ciamac C. Moallemi, "Quantifying Sub-Optimality in Routing for Automated Market Makers", [arXiv:2607.20762](https://arxiv.org/abs/2607.20762) (2026).
- Songrun He, Asaf Manela, Omri Ross and Victor von Wachter, "Fundamentals of Perpetual Futures", [arXiv:2212.06888](https://arxiv.org/abs/2212.06888) (2026).
- Extractions for all of the above and the remaining works are in `materials/notes/cluster-*.md`.

---

## Key points

- The field splits into eight schools — microstructure foundations, TCA, the
  stochastic-control school, market making, order-book micro-execution, market
  structure and routing, learning, and crypto/AMM — each answering a different
  part of the same act of trading.
- The schools form a genealogy of relaxations and oppositions: AC generalises
  Bertsimas–Lo by adding risk, Obizhaeva–Wang generalises AC by endogenising
  trade timing, and Guéant reconciles Kyle's linear permanent impact with the
  empirical square-root law.
- The most load-bearing empirical fact across the schools is that impact is
  concave in size and transient in time, which no linear-permanent-impact model
  reproduces.
- Three reading paths serve three goals: intuition, a working system (theory →
  calibration → engineering), and research (surveys, then one specific seam).
- The live open problems are robust nonlinear/transient optimality, joint
  signal-and-execution optimisation, RL out-of-distribution generalisation,
  crypto fee-and-funding co-optimisation, latency-versus-fee routing,
  the unobservable information half-life, and reconciling display with
  concealment.
- Every statement rests on a document actually read; the nine commercial
  monographs were used only through their authors' or publishers' own public
  substitutes, and Johnson's *Algorithmic Trading & DMA* yields nothing but its
  table of contents.
- The "over 60% fee-schedule saving" figure could not be verified in any obtained
  source and is not repeated here as fact.
