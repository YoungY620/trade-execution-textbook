# 术语表

本书出现的术语一览，与正文中悬浮卡片里的一句话释义一致。每条另附该术语首次出现的章节。

[A](#gloss-a) · [B](#gloss-b) · [C](#gloss-c) · [D](#gloss-d) · [E](#gloss-e) · [F](#gloss-f) · [G](#gloss-g) · [H](#gloss-h) · [J](#gloss-j) · [K](#gloss-k) · [L](#gloss-l) · [M](#gloss-m) · [N](#gloss-n) · [P](#gloss-p) · [Q](#gloss-q) · [R](#gloss-r) · [S](#gloss-s) · [W](#gloss-w) · [X](#gloss-x) · [Y](#gloss-y) · [Z](#gloss-z)

<a id="gloss-a"></a>

## A

<a id="term-dark-pool"></a>

**暗池** — *Dark pool*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

不公开报价、通常按公开市场中间价撮合的非显示交易场所。

Zhu 表明加入暗池可能抬高价差却改善价格发现，因为知情单方向一致、在暗池中成交概率更低。


<a id="gloss-b"></a>

## B

<a id="term-reservation-price"></a>

**保留价格** — *Reservation price*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

按持仓调整后的中间价，做市商在这一点上对是否报价无差异，又称无差异价格。

Avellaneda–Stoikov 给出闭式 r(s,q,t)=s-q*gamma*sigma^2*(T-t)：多头报在中间价之下、空头报在其上，且随到期临近趋于零。


<a id="term-guaranteed-vwap"></a>

**保证成交均价** — *Guaranteed VWAP*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

经纪商承诺按当期 VWAP 交付客户订单的合约，其溢价等于该义务的无差异价格。


<a id="term-picking-off-risk"></a>

**被抢单风险** — *Picking-off risk*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

显示中的挂单在撤单前被快速交易者利用过时报价抢先成交的风险。

Buti–Rindi 表明引入快速交易者后，隐藏单在两个决策节点都成为均衡策略，且挂在第二档。


<a id="term-iceberg-order"></a>

**冰山单** — *Iceberg order*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

只显示小部分峰量、其余隐藏，峰量成交后自动补出新峰的限价单。

在实行"显示优先"的场所，冰山单的显示部分在同一价位并不享受优先；反复补峰使其可被识别。


<a id="gloss-c"></a>

## C

<a id="term-participation-rate"></a>

**参与率** — *Participation rate*  <span class='bd-gloss-alpha'>首见: 第 13 章</span>

在成交窗口内交易者自身订单占市场成交量的比例，也是 POV 策略固定的变量。

Zarinelli 等写作 $\eta=Q/(V(t_e)-V(t_s))$，与时长 $F$ 及日内占比 $\pi=\eta F$ 相区别。


<a id="term-percentage-of-volume"></a>

**参与率** — *Percentage of Volume*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

POV 策略按市场成交量的固定比例成交，期限由此隐含；其最优比率在 Guéant 模型中有闭式解。


<a id="term-policy-gradient"></a>

**策略梯度** — *Policy gradient*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

一族直接优化策略的方法，通过估计期望收益对策略参数的梯度来更新。

与价值型方法不同，它天然处理连续动作，因此被用于动作是成交量的端到端执行。


<a id="term-policy-distillation"></a>

**策略蒸馏** — *Policy distillation*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

训练学生策略去复现教师策略的动作分布，使学生在不具备教师额外输入的情况下继承其行为。

Fang 等把能看到完美未来信息的教师蒸馏为只看当前可观测量的学生，从而在部署时消除未来信息泄露。


<a id="term-split-order"></a>

**拆单** — *Split order*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

把大单拆成小单以隐藏意图；Stenfors–Susai 定义为 0.1 秒内同价提交的成对限价单。

EBS 上约 17.42% 的 EUR/USD 限价单满足该规则；拆单会被识破，并在同等规模下引发更多流动性撤回。


<a id="term-fill-intensity"></a>

**成交强度** — *Fill intensity*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

挂单被成交的泊松速率，是报价离中间价距离的递减函数。

Avellaneda–Stoikov 取指数形式 A*exp(-k*delta)；Cont–Stoikov–Talreja 则从簿状态算出成交概率而非直接假设。


<a id="term-trade-through"></a>

**成交穿透** — *trade-through*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

以劣于市场他处最优挂单的价格成交，从而跳过了更优价的挂单。

在 CME 属违规（第 528 条）；在 Reg NMS 中，第 611 条只要求交易中心尽力避免穿透"受保护"报价。


<a id="term-volume-weighted-average-price"></a>

**成交量加权平均价** — *Volume-Weighted Average Price*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

按各价位成交量加权的期间均价，作为执行基准可被前倾下单所影响。


<a id="term-vpin"></a>

**成交量同步的知情交易概率** — *VPIN*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

按成交量时间计算的订单流毒性实时指标，取各成交量桶买卖失衡绝对值的均值。

定义见 Easley–López de Prado–O'Hara，基线取 $V$ 为日均量 1/50、$n=50$ 个桶。


<a id="term-volume-effect-process"></a>

**成交量效应过程** — *Volume effect process*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

交易仍留在订单簿上、随簿面恢复而衰减的那部分，决定当前卖一价。

Predoiu–Shaikhet–Shreve 把它写成 E_t = X_t 减去恢复速率 h(E_s) 的积分；卖一为影子价加上 psi(E_t)，psi 是簿面累计深度的逆函数。


<a id="term-impact-surface"></a>

**冲击面** — *Impact surface*  <span class='bd-gloss-alpha'>首见: 第 13 章</span>

把市场冲击同时视为参与率与时长两者的函数，而不只是订单规模的函数。

Zarinelli 等拟合 $g(\eta,F)=a\log_{10}(1+b\eta)\log_{10}(1+cF)$，$\hat a=0.035$、$\hat b=60$、$\hat c=61$；双重幂律在残差中留下结构，此式则不然。


<a id="gloss-d"></a>

## D

<a id="term-arrival-price"></a>

**到达价格** — *Arrival Price*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

订单交给交易台那一刻的中间价，是到达价（实施差额）算法使用的基准。


<a id="term-ecn"></a>

**电子通信网络** — *ECN*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

电子通信网络，指 Instinet、Island 这类无需注册为交易所即可撮合订单的电子场所。

SEC 1997 年通过 Reg ATS 后按另类交易系统监管；到 2009 年 10 月，五家活跃 ECN 约占美国股数成交量的 10.8%。


<a id="term-order-protection-rule"></a>

**订单保护规则** — *order protection rule*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

Reg NMS 第 611 条，要求交易中心尽量避免以劣于受保护报价的价格成交；只有可立即自动执行的报价才受保护。

“可立即执行”这一条件排除了缓慢的手工报价，迫使场内市场自动化；它不是禁令，交易中心只需尽力，也可改用跨市场扫单指令（ISO）。


<a id="term-payment-for-order-flow"></a>

**订单流付费** — *Payment for order flow*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

批发商为换取券商把客户订单路由给自己而支付的费用；SEC 允许但要求披露。

Ernst–Spatt (2022) 估计期权每 100 股约 40 美分、股票约 20 美分，且同样名义投资在期权上产生的 PFOF 是股票的十倍。


<a id="term-order-flow-imbalance"></a>

**订单流失衡** — *Order flow imbalance*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

最优价位上买卖的净差额；短时间尺度上驱动价格变动的是它而非成交量。

Cont–Kukanov–Stoikov（2014）发现订单流失衡与价格变动呈线性关系，斜率与市场深度成反比。


<a id="term-order-book-imbalance"></a>

**订单簿不平衡** — *Order book imbalance*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

最优报价处可见买卖流动性的比例，常作为短期价格与流量的预测指标。

在 Cebiroğlu–Horst 模型中它是显示决策影响市场的唯一渠道，最优显示量在时间优先与所造成的不平衡之间取舍。


<a id="term-queue-position"></a>

**队列位置** — *Queue position*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

订单在其价位先进先出队列中的排位，决定成交概率与被逆向选择的程度。

Moallemi–Yuan 把它写成 $V(q,\delta)=\alpha(q)\delta-\beta(q)$，即成交概率乘流动性溢价减去逆向选择成本。


<a id="gloss-e"></a>

## E

<a id="term-quadratic-cost-model"></a>

**二次成本模型** — *Quadratic cost model*  <span class='bd-gloss-alpha'>首见: 第 5 章</span>

线性冲击模型的别名：单区间成交 n 单位的成本为 ε|n|+(η/τ)n²，对切片大小是二次的。


<a id="gloss-f"></a>

## F

<a id="term-inverted-fee-venue"></a>

**反向费率场所** — *Inverted fee venue*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

把挂单—吃单定价颠倒过来的场所：对提供流动性收费，对移除流动性付返佣。

由于整数分格不允许把返佣表达为更优报价，反向场所靠在价差内部造出更细的有效网格竞争；在 2016 年分格试点中，trade-at 组的反向场所份额从 15% 升至 28%。


<a id="term-gas-fee"></a>

**Gas 费** — *Gas fee*  <span class='bd-gloss-alpha'>首见: 第 11 章</span>

链上交易的固定成本，按笔收取，而非随交易规模成比例。

因为按笔固定，它在总成本中的占比随名义金额上升而下降，这正是 DEX 只在约 USD 10,000 以上更便宜、以及带固定每池 gas 成本的路由成为混合整数问题的原因。


<a id="term-tiered-maker-taker-fee"></a>

**分层做市—吃单费率** — *Tiered maker-taker fee*  <span class='bd-gloss-alpha'>首见: 第 11 章</span>

按成交额比例收取的交易所费，费率取决于过去 30 天的成交量，且挂单与吃单不同。

Binance 现货从零售端的 0.1000% maker/taker 到 VIP 9 的 0.0110%/0.0230%，每档 maker 都低于 taker，做市商可协商至零。档位取决于已实现的成交量，因此边际费率依赖历史。


<a id="term-risk-aversion-parameter"></a>

**风险厌恶参数** — *Risk-aversion parameter*  <span class='bd-gloss-alpha'>首见: 第 5 章</span>

目标函数 E(x)+λV(x) 中的乘子 λ，衡量成本方差相对期望成本被惩罚的重度。

它是约束问题 min E s.t. V≤V* 的拉格朗日乘子，也是光滑凹效用函数的曲率，决定交易者落在有效前沿的位置。λ=0 即风险中性特例。


<a id="term-resilience"></a>

**复原速度** — *Resilience*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

新挂单补回交易造成的订单簿缺口的快慢，表现为卖一回到稳态的衰减速率。

Obizhaeva–Wang 用 rho 表示卖一偏离项的衰减率。决定最优调度的是它而非静态冲击系数：rho 趋于 0 退化为纯集中成交，趋于无穷则回到 AC 的连续流。


<a id="gloss-g"></a>

## G

<a id="term-open-outcry"></a>

**公开喊价** — *open outcry*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

场内以口头喊出买卖价、须有人主动"砸单/吃单"才成交的报价方式。

CME 规则中，报价被动，同一价位无时间优先；由此派生的价格优先、成交穿透规则与"主动/被动"之分一直沿用到电子市场。


<a id="term-colocation"></a>

**共置** — *colocation*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

把交易公司的服务器放进交易所机房、靠近撮合引擎，以最小化到市场的物理距离与延迟。

共置是场内信息与接入优势的现代继任者，新的"交易池"就是一台刀片服务器；SEC 2016 年批准 IEX 时加入 350 微秒延迟作为反制。


<a id="term-utilitarian-trader"></a>

**功利交易者** — *Utilitarian trader*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

为交易之外的目的而交易、并愿意承担成本的人；其支付意愿供养所有利润动机交易者。

与之相对的是利润动机交易者，再分为知情者、寄生交易者（抢单者、虚张声势者）与流动性提供者。


<a id="term-maker-taker-pricing"></a>

**挂单—吃单定价** — *Maker-taker pricing*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

交易所对挂单返还佣金、对吃单收取更高费用的定价模式，差额由交易所留存。

按 Hasbrouck 记录的纳斯达克价目表，显示挂单每股得 USD 0.0029、吃单付 USD 0.0030，因此相同报价在不同场所并非相同净价。


<a id="term-make-take-choice"></a>

**挂单—吃单选择** — *Make/take choice*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

挂出被动单还是主动跨越价差：挂单更便宜却可能不成交，吃单付出价差但确定成交。

撮合先按价格再按时间排序，显示订单优先于隐藏订单；这场竞赛里只有相对速度才有价值。


<a id="gloss-h"></a>

## H

<a id="term-constant-function-market-maker"></a>

**恒定函数做市商** — *Constant function market maker*  <span class='bd-gloss-alpha'>首见: 第 11 章</span>

一种去中心化交易所，仅当储备上的交易函数不下降时才接受交易，按曲线而非订单簿定价。

交易 Delta 可行当且仅当 phi(R - Delta) >= phi(R)。实际情形中交易函数是凹的（Uniswap、SushiSwap、Balancer、Curve），因此最优路由是一个凸优化问题。


<a id="gloss-j"></a>

## J

<a id="term-opportunity-cost"></a>

**机会成本** — *Opportunity Cost*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

未成交余量按期末价格计价的成本，只有完全成交时才为零。


<a id="term-concentrated-liquidity"></a>

**集中流动性** — *Concentrated liquidity*  <span class='bd-gloss-alpha'>首见: 第 11 章</span>

Uniswap v3 等 AMM 的设计，流动性提供者把流动性放在选定价格区间内，而非整条曲线。

区间越窄，在边际汇率位于区间内时手续费越多，但汇率一旦离开区间就停止收费，且可预测损失随区间收窄上升。Uniswap v3 提供 0.01%、0.05%、0.1% 三档费率。


<a id="term-price-discovery"></a>

**价格发现** — *Price discovery*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

买卖报价与成交通过聚合彼此不同的信念收敛到新价值的过程，结果事先未知。

Hasbrouck 强调价格发现必须经由交易，且即使没有消息，成交的事实与方向也会进入公共信息集。


<a id="term-price-manipulation"></a>

**价格操纵** — *Price manipulation*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

从零头寸出发、来回交易、最终回到零头寸却获得正期望利润的回路。

其不存在是限价订单簿模型的标准无套利条件。Gatheral 定理说明线性永久冲击是唯一不产生操纵的常系数设定；Guéant 让微观冲击依赖已成交累计量，从而在无套利下恢复非线性。


<a id="term-price-improvement"></a>

**价格改善** — *Price improvement*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

成交价优于近侧报价的幅度，例如在暗池成交或与隐藏单成交时。

这正是有效价差可以窄于报价差的原因；在 maker-taker 定价下报价差已不等于成交成本。


<a id="term-gamma-bridge"></a>

**伽马桥** — *Gamma Bridge*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

Frei 与 Westray 用于刻画日内相对成交量曲线的过程，实证上可拟合且与总成交量无关。

Frei–Westray（2015）用它闭式求解 VWAP 追踪问题；最优速率分解为确定性 TWAP 分量加上由观测成交量曲线驱动的调整。


<a id="term-transaction-cost-analysis"></a>

**交易成本分析** — *Transaction Cost Analysis*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

围绕执行的度量闭环：事前成本预估、事后分项归因、基准选择与统计显著性。


<a id="term-trading-enhanced-risk"></a>

**交易放大的风险** — *Trading-enhanced risk*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

随交易规模本身增大的执行成本方差，因为大单更易遭遇不确定的流动性。

Almgren（2003）给临时冲击加上随机分量 h(V)*xi，使期末财富方差多出 sum V^2 h(V)^2 一项，从而让最优调度变慢。


<a id="term-half-life-of-a-trade"></a>

**交易的半衰期** — *Half-life of a trade*  <span class='bd-gloss-alpha'>首见: 第 5 章</span>

头寸衰减到 1/e 所需的时间 θ=1/κ，只由流动性、波动率和风险厌恶决定，与期限无关。

它是交易的内在时间尺度：即使没有截止时间，风险厌恶者仍以 θ 为尺度清算。比值 κT=T/θ 表明主导力量：很小趋向线性清算，很大趋向立即清算。


<a id="term-trading-trajectory"></a>

**交易轨迹** — *Trading trajectory*  <span class='bd-gloss-alpha'>首见: 第 5 章</span>

从初始头寸 X 到期限时归零的持仓序列 x_0,…,x_N，等价地由交易清单 n_k 描述。


<a id="term-access-fee"></a>

**接入费** — *Access fee*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

交易中心对访问其受保护报价收取的费用，Rule 610 将上限定为每股 USD 0.003，以使显示价接近真实价格。

正是这个上限让路由器可以跨场所比较显示价；报价低于 USD 1.00 时，费用不得超过报价的 0.3%。


<a id="term-settlement-latency"></a>

**结算延迟** — *Settlement latency*  <span class='bd-gloss-alpha'>首见: 第 11 章</span>

区块链确认转账前的等待时间，其间跨所套利者暴露于价格风险。

它给出一个随延迟与波动率放大的无交易带，因此跨所路由是关于结算时间的控制问题，而不只是费率比较。在目标场所持有库存可消除延迟，但换来交易所违约风险。


<a id="term-proximal-policy-optimization"></a>

**近端策略优化** — *Proximal policy optimization*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

一种策略梯度方法，通过裁剪策略更新使新策略不远离旧策略，训练稳定且调参成本低。

Lin–Beling 用它在原始 level-2 数据上端到端训练，奖励是对 TWAP 的稀疏信号，二十万步内收敛。


<a id="term-decision-price"></a>

**决策价格** — *Decision Price*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

管理者决定建仓时的价格（通常取中间价），是实施差额的度量基准。


<a id="term-mean-quadratic-variation"></a>

**均值–二次变差** — *Mean-quadratic variation*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

时间一致的执行目标：惩罚期望成本、持仓平方的积分，以及策略的二次变差。

Forsyth、Kennedy、Tse 与 Windcliff（2012）证明它在算术布朗运动下的精确最优解与静态 AC 解重合，这解释了确定性 sinh 调度为何如此有效。


<a id="gloss-k"></a>

## K

<a id="term-kyle-s-lambda"></a>

**凯尔 λ** — *Kyle's lambda*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

Kyle 模型中价格对总订单流的斜率，经典线性冲击系数，其倒数为市场深度。

单次拍卖中为 (Σ₀/σ_u²)^(1/2) 的一半，连续极限中为整个值；它是永久冲击的均衡微观基础。


<a id="term-predictable-loss"></a>

**可预测损失** — *Predictable loss*  <span class='bd-gloss-alpha'>首见: 第 11 章</span>

集中流动性提供者的不可对冲损失，由池的曲率算出，随区间收窄而上升。

它是集中流动性情形下 loss-versus-rebalancing 的对应物，也是 AMM 吃单方所付凸性成本的镜像；手续费收入与它相权衡。


<a id="term-inventory-skew"></a>

**库存偏移** — *Inventory skew*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

因持仓而使报价中心偏离市场中间价的量，即保留价格减去中间价。

在指数成交强度下总价差与库存无关，库存信息全部由偏移承载。


<a id="term-inventory-risk"></a>

**库存风险** — *Inventory risk*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

成交后持有头寸、在平掉之前价格可能不利变动而承担的风险。

它通过风险厌恶与波动率进入价差，也是最优报价变宽、并逆持仓偏移的原因。


<a id="term-intermarket-sweep-order"></a>

**跨市场扫单** — *Intermarket sweep order*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

被标记为扫单并同时发出其他指令以覆盖全部更优受保护报价的限价单，接收方可立即执行而无须顾及那些报价。

定义见 Rule 600(b)(30)，许可见 Rule 611(b)(5)；它是在多个场所同时取用流动性的标准合规机制。


<a id="gloss-l"></a>

## L

<a id="term-offline-reinforcement-learning"></a>

**离线强化学习** — *Offline reinforcement learning*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

只从预先收集的固定数据集学习策略，不与环境交互，适用于在线探索代价过高的场景。

已知风险是分布偏移：离线训练的策略只会遇到自己犯错造出的状态，容易偏离市场真实状态。


<a id="term-zero-shot-transfer"></a>

**零样本迁移** — *Zero-shot transfer*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

把策略用在从未训练过的资产或市场上且不再学习，并度量其成本差距。

Tran–Yu 在 954 只股票上训练一次，原样测试于未见股票、加密与第二交易所，迁移差距在每股 0.015 个价差内，但无法覆盖未见的价差区间。


<a id="term-liquidity"></a>

**流动性** — *Liquidity*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

想交易时就能交易的能力，可拆为即时性、紧度、深度、弹性，并具有网络外部性。

Harris 的务实定义；Hasbrouck 将其拆为即时性、紧度、深度与弹性四项属性。


<a id="term-liquidity-supplier"></a>

**流动性提供者** — *Liquidity supplier*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

作为对手方让他人想交易时就能成交的利润动机交易者：做市商跨时间，套利者跨市场。

Harris 指出做市商靠同向移动买卖报价来恢复库存；逆向选择是其首要难题，并迫使其扩大价差。


<a id="term-liquidity-adjusted-value-at-risk"></a>

**流动性调整 VaR** — *Liquidity-adjusted Value at Risk*  <span class='bd-gloss-alpha'>首见: 第 5 章</span>

即 L-VaR，给定期限下任一静态清算策略所能达到的最小风险价值 VaR_p(x)=λ_v·√(V(x))+E(x)。


<a id="term-liquidity-rebate"></a>

**流动性返佣** — *Liquidity rebate*  <span class='bd-gloss-alpha'>首见: 第 13 章</span>

交易所对挂入订单簿并最终成交的限价单支付的挂单方返还，资金来自向可成交订单收取的吃单费。

Hasbrouck 记录的 NASDAQ 条款为可见挂单每股 USD 0.0029、吃单方 USD 0.0030；反向报价场所把两者倒置。


<a id="term-predatory-trading"></a>

**掠夺性交易** — *Predatory trading*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

针对可观测的清算需求抢跑，同向卖出后在其压低价格后买回。

Brunnermeier–Pedersen 证明由此产生的价格过冲使困境者的超额成本等于掠夺者利润，并据此反对严格披露持仓。


<a id="term-roll-s-implicit-spread"></a>

**罗尔隐含价差** — *Roll's implicit spread*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

仅由价格变动的一阶序列协方差反推有效价差的免报价估计量，s = 2√(−cov)。

之所以成立，是因为交易成本造成负的序列相关；Roll（1984）在 1963–1982 年 CRSP 上测得日均 0.298%，且随公司规模下降。


<a id="gloss-m"></a>

## M

<a id="term-markov-decision-process"></a>

**马尔可夫决策过程** — *Markov decision process*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

不确定性下序贯决策的形式化对象，由状态、动作、转移概率、奖励与折扣（或有限时域）构成。

固定状态、动作、转移与奖励后，执行问题即成为一个 MDP；其状态通常拆为私有变量（已用时间、剩余库存）与市场变量。


<a id="term-bid-ask-spread"></a>

**买卖价差** — *Bid-ask spread*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

最优卖价与最优买价之差，是需要即时成交者缴纳的通行费。

Wyart 等（2008）将其拆为订单处理、逆向选择与库存风险三类成分，三者都不是手续费。


<a id="term-impulse-control"></a>

**脉冲控制** — *Impulse control*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

在孤立停时施加的控制，使状态发生离散跳跃而非连续变化。

发市价单是脉冲控制：它瞬时发生，使库存与现金离散跳变，成本为半个价差加手续费。


<a id="term-imitation-learning"></a>

**模仿学习** — *Imitation learning*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

从其他策略的示范中学习，而不是依赖人工设计的奖励，从而避免昂贵的探索。

在执行中很有吸引力，因为在真实市场探索既费钱又不可重复；FlowOE 便模仿了经典与学习式专家的集合。


<a id="term-target-close"></a>

**目标收盘** — *Target Close*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

以收盘价为基准的执行方式，终点固定、起点未知，是实施差额的镜像。


<a id="gloss-n"></a>

## N

<a id="term-internalization"></a>

**内部化** — *Internalization*  <span class='bd-gloss-alpha'>首见: 第 12 章</span>

经纪自营商用自有库存或自身订单流撮合客户可成交订单、而不送往公开场所的做法。

美国股票中相当比例的零售流量被内部化或在场外路由；随之而来的价格改善是真实的，且在价差处于 1 美分最低值附近时最有价值。


<a id="term-hjb-quasi-variational-inequality"></a>

**拟变分不等式** — *HJB quasi-variational inequality*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

正则与脉冲混合控制问题的动态规划方程，写成"运行方程与脉冲条件取最小"的形式。

当一种控制连续施加、另一种只在孤立停时施加时（如限价单与市价单），普通 HJB 无法表示脉冲渠道，必须用拟变分不等式。


<a id="term-adverse-selection"></a>

**逆向选择** — *Adverse selection*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

与你成交的人可能是因为知道你不知道的事，因此报价必须按其反推盈亏平衡。

Glosten–Milgrom（1985）证明，仅逆向选择一项就能撑出价差，即使专营商风险中性、面临竞争且零利润。


<a id="gloss-p"></a>

## P

<a id="term-naive-strategy"></a>

**平均切分策略（naïve 策略）** — *Naive strategy*  <span class='bd-gloss-alpha'>首见: 第 5 章</span>

等量、等间隔切分的调度，在忽略风险时使期望成本最小。

它是 Almgren–Chriss 有效前沿的极小点；Bertsimas 与 Lo 证明它仅在"线性永久冲击 + 算术随机游走"下最优。由于前沿在那点平坦，风险厌恶者不应采用它。


<a id="term-square-root-law"></a>

**平方根律** — *Square-root law*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

经验规律：母单的平均价格变动正比于其规模相对成交量之比的平方根乘以波动率。

写作 Δ = Yσ(Q/V)^(1/2)，Y 为 O(1) 量级；指数一般在 0.4–0.7，Zarinelli 等（2015）发现对数形式拟合更好。


<a id="gloss-q"></a>

## Q

<a id="term-reg-nms"></a>

**全国市场系统条例** — *Reg NMS*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

SEC 2005 年通过的《全国市场系统条例》，把美国各股票交易中心联结成"虚拟合并"市场。

含订单保护规则（611）、接入规则（610，接入费上限每股 USD 0.003）、最小报价单位规则（612）与市场数据规则；2004 年 2 月提出、2005 年 6 月通过、2006 年分期实施。


<a id="term-nbbo"></a>

**全国最优买卖报价** — *NBBO*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

全国最优买卖报价，由合并报价数据算出的全市场最高买价与最低卖价，是执行与路由的参照。

对纽交所和美国交易所挂牌股票，NBBO 由合并报价系统算出；SEC 将这类系统的运营方认定为证券信息处理器（SIP）。


<a id="term-national-best-bid-and-offer"></a>

**全国最优买卖报价** — *National best bid and offer*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

全部自动化交易中心中最优的受保护买价与卖价，简称 NBBO，是价格保护的参照。

每个场所家族只有一条可访问的最优买卖价受保护，因此 NBBO 由少数几条报价构成，而非整本合并订单簿。


<a id="gloss-r"></a>

## R

<a id="term-circuit-breaker"></a>

**熔断机制** — *circuit breaker*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

指数相对前收盘大跌时触发的全市场停牌，用于打断暴跌、让参与者重新评估。

美国市场级熔断为应对 1987 年 10 月崩盘设立，1997 年 10 月 27 日首次触发，2020 年 3 月四次触发；加密市场没有统一市场，本章判断其没有直接对应物。


<a id="gloss-s"></a>

## S

<a id="term-sandwich-attack"></a>

**三明治攻击** — *Sandwich attack*  <span class='bd-gloss-alpha'>首见: 第 11 章</span>

一种 MEV 策略，在受害者链上交易前后各插入一笔交易以恶化其成交价并从中获利。

对 n 笔用户交易，三明治造成的最大价格冲击相对平均为 O(log n)；被三明治的交易在路由次优性上明显上移。按单一清算价批量成交可以消除它。


<a id="term-deep-q-network"></a>

**深度 Q 网络** — *Deep Q-network*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

用神经网络逼近 Q 函数，从而无需查表即可使用订单簿特征等高维状态。

用经验回放与目标网络训练；由于可行动作集随库存变化，Ning 等把状态与动作一起输入网络并读出 $Q(s,x)$。


<a id="term-implementation-shortfall"></a>

**实施差额** — *Implementation Shortfall*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

决策价位下的纸上组合与实际成交组合之间的差额，是度量交易成本的标准指标。

由 Perold（1988）提出，本书未获取其原文，此处定义据二手文献重建。


<a id="term-realised-spread"></a>

**实现价差** — *Realised spread*  <span class='bd-gloss-alpha'>首见: 第 13 章</span>

成交价与订单之后固定间隔（通常五分钟）所测报价中间价之差的二倍。

它与有效价差之差近似为该订单自身的价格冲击；美国 SEC Rule 605 要求交易中心按月公布两者。


<a id="term-realized-spread"></a>

**实现价差** — *Realized spread*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

成交价与五分钟后报价中间价之差的两倍，是价格移动后流动性提供者实际留下的补偿。

有效价差减实现价差近似等于该笔订单的价格冲击。O'Hara（2015）指出它依赖时间尺度，在高速电子市场常为负。


<a id="term-market-impact-model"></a>

**市场冲击模型** — *Market impact model*  <span class='bd-gloss-alpha'>首见: 第 14 章</span>

描述一定规模交易如何推动价格（通常分为临时与永久两部分）的函数形式，是每个执行排程优化的对象。

Kyle（1985）的线性永久冲击是理论基准；经验文献（Toth 等 2011；Bouchaud 2010）发现冲击对规模是凹的、对时间是瞬态的。Gueant（2013）证明永久冲击可以非线性且仍无套利。


<a id="term-market-resilience"></a>

**市场弹性** — *Market resilience*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

价格在随机、无信息冲击后回复的速度，也是暂时冲击可以复原的原因。

Bouchaud–Farmer–Lillo（2009）测得价差以 0.4–0.5 的幂律指数回复；订单流是长记忆的，冲击必须暂时化价格才能近似鞅。


<a id="term-market-microstructure-invariance"></a>

**市场微观结构不变性** — *Market microstructure invariance*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

假设：按单位业务时间（每次下注到达）度量，跨资产的下注规模与交易成本是常量。

Kyle–Obizhaeva（2016）导出下注笔数 ∝ W^(2/3)、非流动性指数 ∝ (σ²/PV)^(1/3)；平方根是唯一使成本只依赖 Q/V 的冲击函数。


<a id="term-market-depth"></a>

**市场深度** — *Market depth*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

使价格变动一个单位所需的订单流，即 Kyle's lambda 的倒数，正比于噪声交易量除以未消化信息。

在 Kyle 连续极限中深度随时间恒定，价格因此成为常数波动率的布朗运动。


<a id="term-market-fragmentation"></a>

**市场碎片化** — *Market fragmentation*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

同一标的的交易分散在多个场所，且需区分明牌场所之间的分散与明暗之比。

SEC 工作人员综述把"可见碎片化"与"暗碎片化"视为特征不同的两种现象（SEC 2013）。


<a id="term-time-consistency"></a>

**时间一致性** — *Time consistency*  <span class='bd-gloss-alpha'>首见: 第 14 章</span>

今天选定的策略在之后仍然最优、因而可以作为反馈规则执行的性质；Almgren-Chriss 的均值—方差目标不具备它。

均值—方差执行是一种预先承诺策略；Forsyth 等（2012）用时间一致的"均值—二次变差"准则替代它，并把 Almgren-Chriss 排程还原为算术布朗运动下的最优解。


<a id="term-time-inconsistency"></a>

**时间不一致** — *Time inconsistency*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

在某一时点算出的最优策略过后不再最优（即便条件未变），因此不能直接用动态规划。

均值–方差准则因方差项含期望的平方而时间不一致。均值–二次变差（Forsyth 等 2012）与期望效用目标（Schied–Schöneborn 2009）是时间一致的替代。


<a id="term-protected-quotation"></a>

**受保护报价** — *Protected quotation*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

同时是自动化报价、由自动化交易中心显示、且为该场所家族最优买卖价的报价，只有它受 Rule 611 保护。

Rule 600(b)(57) 规定三个要件；不符合 Rule 600(b)(3) 自动化要求的为人工报价，不受保护。


<a id="term-data-snooping"></a>

**数据窥探** — *Data snooping*  <span class='bd-gloss-alpha'>首见: 第 13 章</span>

在用于报告业绩的同一个样本上挑选模型或参数，使报告结果好于其真实水平。

对策是步进式切分：在一个窗口拟合、在下一个窗口打分，函数形式只选定一次。


<a id="term-hyperbolic-sine-trajectory"></a>

**双曲正弦轨迹** — *Hyperbolic-sine trajectory*  <span class='bd-gloss-alpha'>首见: 第 5 章</span>

线性冲击下最优持仓路径的闭式解 x(t)=X·sinh(κ(T−t))/sinh(κT)。

它是差分方程 (1/τ²)(x_{j−1}−2x_j+x_{j+1})=κ²x_j 的解，从 X 单调降到零且从不回购；连续时间极限下 κ=√(λσ²/η)。


<a id="term-double-deep-q-learning"></a>

**双重深度 Q 学习** — *Double deep Q-learning*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

深度 Q 网络的改进：用两个网络把动作选择与价值估计分开，抑制 Q 值的系统性高估。

是执行文献中最常见的价值型方法，Ning 等与 Karpe 等（多智能体订单簿仿真）都采用了它。


<a id="term-transient-price-impact"></a>

**瞬态冲击** — *Transient price impact*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

交易刚发生时最强、随后逐渐衰减的冲击，而非永久地移动价格。

它取代 AC 的"永久冲击 + 瞬时冲击"二分。Lorenz–Schied 证明瞬态冲击下最优策略依赖漂移的导数，稳健性因此远不如 AC 框架。


<a id="term-instantaneous-impact-function"></a>

**瞬时冲击函数** — *Instantaneous impact function*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

Guéant 的 L(ρ)，单位成交量成本关于参与率的函数；严格凸、偶、L(0)=0 且渐近超线性。

实践中取 L(ρ)=η|ρ|^(1+φ)+ψ|ρ|，可涵盖线性与平方根形状；永久冲击 k 只是换元，自由度在 L 上。


<a id="term-stochastic-control"></a>

**随机控制** — *Stochastic control*  <span class='bd-gloss-alpha'>首见: 第 14 章</span>

把执行问题写成受控状态过程（库存、价格）加目标函数，再解动态规划或 HJB 方程的范式。

谱系从 Bertsimas-Lo（1998）、Almgren-Chriss（2000）经 Obizhaeva-Wang、Gueant 到 Forsyth 的 HJB 方法。放松其假设——线性冲击、鞅价格、均值—方差目标——定义了 2000 年后的大部分执行文献。


<a id="term-aggressive-in-the-money"></a>

**随盈利而激进** — *Aggressive in the money*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

价格朝有利方向变动、头寸相对到达价处于盈利时卖得更快。

Gatheral–Schied 的闭式策略随盈利而激进，却与波动率无关。Schied–Schöneborn 说明效用最大化者随盈利而激进，当且仅当绝对风险厌恶递增；CARA 下最优策略是确定性的。


<a id="gloss-w"></a>

## W

<a id="term-principal-agent-problem"></a>

**委托代理问题** — *Principal–agent problem*  <span class='bd-gloss-alpha'>首见: 第 12 章</span>

执行交易的一方（经纪商）与承担其成本的一方（客户）之间的错位，由基准与费率结构共同决定。

基准归因决定谁承担价格移动的趋势成分，maker–taker 费用分配改变经纪商的路由动机；把两者合读为一个委托代理问题是本章判断。


<a id="term-microprice"></a>

**微价格** — *Microprice*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

给定当前订单簿失衡下的长期预期价格，用作挂限价单时的比较基准。

Lehalle–Mounjid 把它定义为失衡条件下价格期望的极限；与成交价比较即可编码逆向选择厌恶。


<a id="term-microstructure-invariance"></a>

**微观结构不变性** — *Microstructure invariance*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

实证规律：以商业时间度量时，各资产的下注金额分布与单笔成本相同。

它给出让同一策略在各资产间可比的单位（时间、规模、价格）；Kyle–Obizhaeva 估得下注到达率为 $W^{2/3}$、平均下注为 $W^{1/3}$。


<a id="term-indifference-pricing"></a>

**无差异定价** — *Indifference pricing*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

以使交易者在接手与不接手之间无差异的现金额来定价块交易，前提是该头寸必须被清掉。

Guéant（2015）得出 P = q*S 减永久冲击积分、减线性费用、减优化后的风险与冲击成本；最后一项是"交易得好"能削减的部分，即流动性溢价。


<a id="term-model-free-reinforcement-learning"></a>

**无模型强化学习** — *Model-free reinforcement learning*  <span class='bd-gloss-alpha'>首见: 第 14 章</span>

把执行当作马尔可夫决策过程、从经验中学出策略、不假定已知冲击模型的学习范式。

状态通常是已过时间、剩余库存与市场特征；动作是交易量、限价，或跨委托类型与档位的比例。结果多在仿真中训练，因此分布外泛化与仿真—现实差距仍开放。


<a id="gloss-x"></a>

## X

<a id="term-alpha-signal"></a>

**alpha 信号** — *Alpha signal*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

对下一次价格变动方向的短周期预测，由订单流或失衡驱动，用来偏移报价或择时交易。

Lehalle–Neuman 把它建为马尔可夫信号，Cartea–Wang 把它建为由市价单驱动的动量；两者中信号都进入最优报价，信号足够大时做市商转为吃单者。


<a id="term-information-half-life"></a>

**信息半衰期** — *Information half-life*  <span class='bd-gloss-alpha'>首见: 第 12 章</span>

私有估值信号在被市场价格消化前预期能维持的时间，决定知情交易的节奏。

与 Almgren–Chriss 的交易半衰期 $\theta=1/\kappa$（由流动性、波动率与风险厌恶决定）不同；活跃信号会把持仓的有效衰减压到低于纯风险框架的水平。


<a id="term-q-learning"></a>

**Q 学习** — *Q-learning*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

无模型算法，通过不断由转移更新估计，学出每个状态下各动作的期望成本或价值。

用于执行时常用反向归纳，利用私有变量与市场变量近似独立，使表格状态空间保持很小。


<a id="gloss-y"></a>

## Y

<a id="term-latency-arbitrage"></a>

**延迟套利** — *Latency arbitrage*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

在一个场所报价已过期、另一场所报价已变动的极短瞬间，靠比所有人更快而获利。

Aquilina–Budish–O'Neill (2022) 在英国股票上测得它约占成交量的 0.42–0.53 个基点、约为有效价差的三分之一，并估计消除它可使市场流动性成本下降 17%。


<a id="term-delay-cost"></a>

**延迟成本** — *Delay Cost*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

决策到首笔交易释放之间的价格暴露成本，等于成交量乘以该期间的价格变动。

Hasbrouck 把实施差额拆为价格冲击与延迟成本两项，并引 Wagner（2003）对机构延迟成本约 77 bp 的估计。


<a id="term-latency-cost"></a>

**延迟成本** — *Latency cost*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

反应慢所损失的价值，等于即时反应的最优值减去给定延迟下可达的值。

Lehalle–Mounjid 用"每 tau 次簿更新才能改单"的慢代理与正常代理的值之差来衡量，并把它读作交易者应为速度支付的最高代价。


<a id="term-actor-critic"></a>

**演员–评论家** — *Actor–critic*  <span class='bd-gloss-alpha'>首见: 第 10 章</span>

策略梯度的设计，含两部分：演员提出动作，评论家估计价值并提供学习信号。

连续动作的演员–评论家（DDPG）让执行策略连续选择成交量，并在瞬时冲击下把过去动作纳入状态。


<a id="term-hidden-order"></a>

**隐藏单** — *Hidden order*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

完全不显示数量的限价单，整个数量不进入可见订单簿。

隐藏单在多数场所同价位让位于显示单；实证显示它既用于隐藏大额规模，也被高频交易者用于抢单降价与择时。


<a id="term-permanent-impact"></a>

**永久冲击** — *Permanent impact*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

冲击中反映信息释放、留在价格里的部分，等待也无法收回。

等于 λ 乘以净订单流；它是公共信息事件，而非付给流动性提供者的租金。


<a id="term-perpetual-futures"></a>

**永续合约** — *perpetual futures*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

没有到期日、靠周期性资金支付锚定现货的期货合约，加密市场的主要执行场所，BitMEX 2016 年首创。

2022 年全市场永续日成交量中位数约 1019 亿美元，为现货的两到三倍；永续腿通常报价价差更宽，但深度更好、流更均衡。


<a id="term-effective-spread"></a>

**有效价差** — *Effective spread*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

成交价与成交时报价中间价之差的两倍，是立即成交订单的实际代价。

Hasbrouck 的分解：有效成本为 p−m、实现成本为 p−m5；价格改善为成交价优于近侧报价的差额。


<a id="term-efficient-frontier"></a>

**有效前沿** — *Efficient frontier*  <span class='bd-gloss-alpha'>首见: 第 5 章</span>

在每个成本方差水平下期望成本最小的一族调度，随风险厌恶参数变动扫出的曲线。

在执行为题中是一条光滑凸曲线，极小点为 naïve 策略，该处曲率度量证券流动性。对一系列 λ 求解 min E(x)+λV(x) 即可重现该族。


<a id="term-effective-rebate"></a>

**有效返佣** — *Effective rebate*  <span class='bd-gloss-alpha'>首见: 第 8 章</span>

交易所返佣减去该场所的逆向选择惩罚后的净收益。

在 Cont–Kukanov 模型中记作 $r_k=r_k^{e}+AS_k$，$AS_k$ 通常用限价单成交后短窗口的平均收益估计。


<a id="term-pre-commitment"></a>

**预承诺** — *Pre-commitment*  <span class='bd-gloss-alpha'>首见: 第 6 章</span>

按初始时刻衡量的均值–方差目标求解，然后照此执行，即便它在之后已不再最优。

这是对均值–方差准则时间不一致的标准补救；Forsyth（2011）用非线性 HJB 方程数值求解，并指出它只在大量同质交易的平均意义下最优。


<a id="term-metaorder"></a>

**元订单** — *Metaorder*  <span class='bd-gloss-alpha'>首见: 第 13 章</span>

实现同一次交易决策的一束子单，即方根律所描述其冲击的父订单。

Tóth 等称之为"对应同一次交易决策的一束订单"，冲击由首末子单之间的价格变化度量。


<a id="gloss-z"></a>

## Z

<a id="term-loss-versus-rebalancing-lvr"></a>

**再平衡损失** — *Loss-versus-rebalancing (LVR)*  <span class='bd-gloss-alpha'>首见: 第 11 章</span>

流动性提供者因池机械地卖出升值资产、买入贬值资产而承受的不可对冲损失。

在集中流动性 AMM 上它被度量为可预测损失，随流动性区间收窄而上升。它是 AMM 执行成本的下界：吃单成本不可能低于手续费加上这笔交易造成的损失。


<a id="term-temporary-impact"></a>

**暂时冲击** — *Temporary impact*  <span class='bd-gloss-alpha'>首见: 第 3 章</span>

冲击中因耗尽挂单流动性导致的部分，随盘口回填而衰减，放慢节奏即可优化掉。

在传播模型中由衰减核 G(l) 承载；当符号相关为 l^-γ 时，核须按 l^-β 衰减，β=(1-γ)/2。


<a id="term-smart-order-routing"></a>

**智能订单路由** — *Smart order routing*  <span class='bd-gloss-alpha'>首见: 第 9 章</span>

自动整合多个场所的报价，并在价格、费率与成交概率之间优化母单的跨场所拆分。

在 Foucault–Menkveld (2008) 中，智能路由券商的比例决定新场所能否存活，因此路由技术是场所进入的前提条件。


<a id="term-paper-portfolio"></a>

**纸上组合** — *Paper Portfolio*  <span class='bd-gloss-alpha'>首见: 第 4 章</span>

想象中按决策时中间价取得的全部目标持仓，不含佣金、价差与冲击。


<a id="term-specialist"></a>

**专家经纪人** — *specialist*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

纽交所管理某只股票唯一限价订单簿的会员，负有维持窄价差等义务，并享有知晓簿面、优先接单的特权。

专家经纪人的特权是现代高频做市商信息与速度优势的前身，其义务则是后来"肯定性/否定性交易义务"讨论的源头。


<a id="term-funding-rate"></a>

**资金费率** — *funding rate*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

永续合约多空之间的周期性支付，是期货与现货价差的截断函数，用于锚定现货并直接进入执行成本。

资金费率按剩余头寸计提，等于给持有成本加了一项 carry，使最优清算路径偏离纯粹的 Almgren–Chriss 计划。


<a id="term-best-execution"></a>

**最优执行义务** — *Best execution*  <span class='bd-gloss-alpha'>首见: 第 12 章</span>

经纪商为客户订单争取最优成交条件的监管义务，它确定可行的路由集合，以及成交可以公允地对照哪个基准来评估。

到达价在决策瞬间已知，而 VWAP 与收盘价是价格路径自身的函数，因此按随机基准工作的经纪商与拿到达价考核它的客户度量的并非同一件事。


<a id="term-decimalisation"></a>

**最小报价单位十进制化** — *decimalisation*  <span class='bd-gloss-alpha'>首见: 第 2 章</span>

美国股票报价增量由 1/8 美元改为 1 美分，1997 年立法、2001 年完成，压缩了价格竞争的网格。

用 Hasbrouck 的话说，最小报价单位是时间优先的价格：网格越细，插到同价长队队首就越便宜。


<a id="term-market-making"></a>

**做市** — *Market making*  <span class='bd-gloss-alpha'>首见: 第 7 章</span>

在参考价两侧同时报价、赚取价差，并承担成交带来的库存风险。

在随机控制表述里，做市商控制的是两个报价距离而非交易速率，目标是期末盯市财富的期望效用。
