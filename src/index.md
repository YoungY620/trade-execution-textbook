# From Signal to Execution

A textbook on **optimal trade execution**: how an investment view — or a piece
of private, still-underwater information — is turned into an actual trade.

Knowing what to trade is not the same as getting the trade done. Between the
moment a view is formed and the moment the position exists, a specific amount of
money is lost: to the arrival price, to market impact, to the risk the price
moves while you wait, to the people who read your order flow and trade ahead of
you, and to the exchanges that charge you for the privilege. Execution research
is the study of that loss and of how to minimise it.

This book assembles that research — from the 1970s floor to today's electronic
and crypto venues — into a single connected account: the theory, the history,
the competing schools, the differences between markets and situations, and the
measurement loop that closes it all.

## Chapters

**Foundations**

1. **From signal to execution: the whole stack** — the chain from a view to a
   filled position, its four layers (scheduling, placement, routing,
   measurement), and what changes when the edge is private.
2. **A short history: from the floor to the algorithm** — block trading by
   telephone, DOT, program trading and 1987, ECNs and decimalisation, Reg NMS
   and fragmentation, and the crypto venues.
3. **Market microstructure: where execution cost comes from** — inventory and
   information asymmetry, adverse selection, the permanent and temporary parts
   of price impact, and the empirical impact laws.

**Cost and measurement**

4. **Implementation shortfall and transaction cost analysis** — Perold's
   paper-portfolio-against-reality benchmark, its decomposition, and how TCA
   is actually run.

**The theory of optimal execution**

5. **The Almgren–Chriss framework** — the mean–variance problem, the
   hyperbolic-sine trajectory, the efficient frontier, and how the parameters
   are calibrated.
6. **Beyond Almgren–Chriss** — robustness to drift and model error, limit
   order book supply and demand, block trades, and the stochastic-control
   reformulations.
7. **Market making and the stochastic-control view** — inventory risk, optimal
   quotes, and why execution and market making are two faces of one problem.

**Micro-level execution**

8. **Order books, order types and stealth** — limit against market orders,
   queue position, icebergs and hidden orders, dark pools, information leakage,
   and predatory trading.
9. **Order routing and market fragmentation** — smart order routing, venue
   selection, fees and rebates, latency races, and what fragmentation does to
   execution quality.

**Learning and adaptation**

10. **Reinforcement learning and adaptive execution** — from the first
    Q-learning scheduler to deep and offline methods, and what they add to
    closed-form policies.

**Scenarios**

11. **Cryptocurrency markets** — maker–taker and tiered fees, cross-venue
    fragmentation, perpetual funding, AMMs and DEX routing.
12. **Scenarios side by side** — institutions against retail, high against low
    frequency, informed against uninformed, and how the optimal policy changes.

**Practice**

13. **Measurement, calibration and the engineering loop** — building TCA,
    calibrating impact, backtesting execution, and closing the loop.
14. **The literature: a full map and a reading path** — the schools, the key
    works, and an order in which to read them.

Plus a [glossary](glossary.md) of the field's terms, and a
[references and source material](sources.md) page listing everything consulted
here, including what could not be obtained.

## How to read it

Every chapter ends with a summary of its points and the sources it draws on.
The mathematics is typeset with MathJax. The whole book exists twice — English
at the site root, Chinese under `/zh/` — with a language switch in the top bar.

## Status of the sources

Every claim in this book is traced to a document that was actually read: the
primary papers where they are open access, and the authors' own public lecture
notes, slides and survey papers where a work exists only as a commercial book.
No work is described from its abstract alone. The [source material
page](sources.md) lists what was read, what was not obtained, and why.

---

Produced with the **course-textbook-builder** workflow
(<https://github.com/YoungY620/my-awesome-skills>).
