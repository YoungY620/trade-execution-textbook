# From signal to execution: the whole stack

A trading decision and a trading outcome are two different things, and the gap
between them is where a large part of a strategy's return is won or lost.
Knowing that an asset is mispriced does not put a position on at that price. The
moment the decision is made, a clock starts: the price moves, the order is
noticed, the book thins out, other people trade ahead, the venue charges a fee.
Everything in this book lives in that interval.

Execution research asks a narrow question with a wide answer: **given a decision
to trade a certain quantity in a certain direction, how should it be traded?**
The answer is not a single technique. It is a stack of decisions, each with its
own theory, its own costs, and its own way of failing.

## What "execution" means

Take the simplest possible case. A portfolio manager decides at 09:31 that she
wants 400,000 shares of a stock that trades two million shares a day. By the
time the position exists, the average price she paid may be 30 basis points
above the price that was showing when she decided. That 30 basis points is not a
mistake; it is the cost of the trade, and it arrives whether or not anyone
measures it.

The benchmark that makes this cost visible is the **arrival price**, or decision
price: the mid-quote at the moment the intent was formed. Chapter 4 formalises
the comparison between the portfolio that was decided on — the *paper
portfolio*, bought entirely at the decision price — and the portfolio that
actually exists. The difference is the **implementation shortfall**, and it
decomposes into things that can be attacked separately: the cost of waiting
before the first order goes in, the cost of moving the price while trading, the
cost of whatever was never filled, and the fees.

That decomposition is the spine of the field, and it is worth internalising now
because every later chapter is a different answer to one of its terms.

## The chain: from a view to a position

An investment view becomes a position through four layers. The layers are not
optional and they are not simultaneous; each one constrains the next.

### 1. The decision layer

What comes out of the decision layer is not "buy" but a set of quantities: a
direction, a size, a time window, and a confidence. All four matter, and the
last two are the ones execution people care about. Size relative to the market's
volume determines how much the trade can move the price. The time window — how
long the view is expected to stay true before the market catches up — determines
how fast the trade *has* to be done. Confidence determines how much price risk
is tolerable while it is being done.

The same view, expressed as a firm order to be filled today, and as a patient
working order to be filled over a month, are two completely different execution
problems. Nothing in execution theory can fix a decision that never stated its
horizon.

### 2. The scheduling layer

Scheduling decides **how much to trade when**. A large order split into small
pieces costs less per piece, because each piece has a smaller impact. But every
minute spent splitting is a minute in which the price can move against the
position, and in which other participants can read the pattern. Scheduling is
the trade-off between those two costs, and it has the most developed theory in
the field: the mean–variance problem of Chapter 5, its closed-form solution in
the shape of a hyperbolic sine, and the many relaxations of its assumptions in
Chapter 6.

The result to keep in mind is that the optimal schedule is not "as fast as
possible" or "as slow as possible". It is a specific curved path determined by
volatility, the cost of impact, and risk aversion — and it front-loads when the
order is urgent and drags out when it is not.

### 3. The placement layer

Placement decides **what kind of order to send, and where in the book**. A
market order buys immediacy and pays the spread plus whatever impact it causes.
A limit order offers price improvement and takes on the risk of never being
filled, plus the adverse selection of being filled exactly when the market is
moving away. Between those extremes sit hidden and iceberg orders, pegged
orders, and the choice of which price level to join — and behind them a queue,
whose position is worth real money.

This layer is where a private edge is most exposed. An order that stays visible
long enough to be read becomes a signal to everyone else, and the market's
response is not neutral: Chapter 8 covers the mechanisms by which a large,
predictable order gets traded ahead of.

### 4. The routing layer

Routing decides **which venue each piece goes to**. Once a market has more than
one place to trade, a quote is no longer a single number, and the best price for
a given size may be split across several books with different fees, different
latencies, and different depths. Chapter 9 covers the rules that force routing
decisions, the economics of the choices, and what fragmentation does to the
cost the trader actually pays.

### 5. The cost and risk layers, and the measurement loop

Running alongside the four layers is what those decisions cost and what risks
they carry: market impact (the part that is information and does not come back,
and the part that is liquidity and does), inventory and timing risk, the risk of
having been read, and liquidity risk — the possibility that the depth you were
relying on is no longer there when you need it.

None of this is controllable without measurement. Chapter 4 states the benchmark;
Chapter 13 turns it into an operating loop: record every fill with the fields
that make attribution possible, estimate the impact parameters from your own
history and the market's, compare what happened against what the model predicted,
and feed the difference back into scheduling and placement.

## Why execution is a system, not a step

The layers interact, and the interaction is not additive. Trading faster reduces
timing risk but increases impact per unit time and makes the order easier to
read. Splitting more finely reduces impact per piece but lengthens the exposure
window and creates a more detectable pattern. Choosing a venue for its rebate may
mean giving up price priority. Each layer's optimum depends on what the other
layers are doing, which is why an execution algorithm is a policy rather than a
recipe, and why the same algorithm can be excellent in one stock and harmful in
another.

It also means that a claim about execution — "this algorithm saves 20 basis
points" — is meaningless without naming the benchmark, the horizon, the
liquidity regime, and the fee schedule it was measured against. Chapter 10
returns to this point when reviewing what machine-learned execution policies
actually demonstrate.

## What changes when the edge is private

Most of execution theory was developed for orders that carry no information:
rebalancing, index changes, cash flows, a portfolio manager's slow opinion. For
those, the objective is to minimise the expected cost plus a variance penalty,
and the fact that other participants notice the order is a second-order concern.

An order that does carry information is a different problem. The information
decays — the market will eventually price it in — so there is a real cost to
being slow. But trading visibly to save that cost hands the information to
everyone else, and the people who read it will trade ahead, at your expense.
Kyle's framework in Chapter 3 makes this precise: the price impact of an order
is proportional to how much it is believed to be informed, and the informed
trader's optimal strategy is to hide inside the noise.

The practical consequences run through the book: schedule aggressively enough to
beat the decay but not so aggressively that the pattern is obvious; prefer hidden
and conditional order types; randomise the parts that would otherwise be
predictable; use venues where exposure is lower; and accept a higher variance in
outcomes in exchange for not revealing the trade. Chapter 12 puts this side by
side with the uninformed case and shows how the optimal policy changes.

## The vocabulary you will need

Four terms recur from here on. Market impact is the movement in price caused by
the trade itself. Implementation shortfall is the gap between the decision price
and the realised average price. Adverse selection is the cost of being filled
precisely when the counterparty knows more than you do. Participation rate is
your volume as a share of the market's over the same interval. All of them, and
several hundred more, are collected in the [glossary](./glossary.md) with a
one-line gloss and a pointer to the chapter that introduces them.

## How this book is organised

Chapters 2 and 3 give the historical and theoretical footing: how the market
came to look the way it does, and where execution cost comes from. Chapters 4
and 13 are the measurement layer, framing the problem and closing the loop.
Chapters 5, 6 and 7 are the optimal execution and stochastic control core.
Chapters 8 and 9 are the micro-level decisions — placement and routing.
Chapter 10 covers what learning adds. Chapter 11 covers cryptocurrency markets,
where the fee structure, the fragmentation and the instruments differ enough to
change the answer. Chapter 12 compares the situations side by side. Chapter 14
maps the literature and gives three reading paths.

Each chapter ends with the points it established and the works it drew on.
Every claim in the book is traced to a document that was actually read; the
[source material page](./sources.md) lists what was read and what could not be
obtained, with reasons.

## Key points

- A trading decision and a trading outcome differ by the implementation
  shortfall, which decomposes into delay cost, market impact, opportunity cost
  and fees; this decomposition organises the whole field.
- Execution is a stack of four decisions — scheduling, placement, routing, and
  the measurement loop that feeds back into all three — and each layer's optimum
  depends on the others.
- The scheduling problem is a trade-off between impact and timing risk, and its
  solution is a curved path rather than a constant rate.
- Placement is where an order becomes observable, and observability is a real
  cost for anyone whose order carries information.
- Routing is forced by market structure, and the cheapest venue for a given
  order is not the one with the best quoted price once fees and depth are
  included.
- When the edge is private, the objective changes from minimising expected cost
  to trading fast enough to beat the decay of the information while staying
  hidden enough not to be front-run.
- No execution claim is meaningful without naming its benchmark, horizon,
  liquidity regime and fee schedule.

## Sources

The framing in this chapter is the book's own, and the four-layer decomposition
of the execution problem follows the structure the field is organised around in
its surveys and textbooks. Each element is developed from primary sources in the
chapter that covers it:

- Perold, A. (1988). *The Implementation Shortfall: Paper vs. Reality*. Journal
  of Portfolio Management — the benchmark and its decomposition (Chapter 4;
  original not obtained, see [sources](./sources.md)).
- Kyle, A. (1985). *Continuous Auctions and Insider Trading*. Econometrica — the
  price impact of informed order flow (Chapter 3).
- Almgren, R. and Chriss, N. (2000). *Optimal Execution of Portfolio
  Transactions*. Risk — the scheduling problem and its closed-form path
  (Chapter 5).
- Ryan Donnelly (2022). *Optimal Execution: A Review*.
  <https://arxiv.org/abs/2210.05121> — the taxonomy of the optimal execution
  literature (Chapters 5, 6, 14).
