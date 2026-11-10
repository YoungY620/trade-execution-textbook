# 文献全景与阅读路径

最优执行不是一门文献，而是八门很少同时互相引用的文献。做市商的库存模型、券商的实施差额白皮书、讲 Hamilton–Jacobi 方程的数学金融论文、订单簿计量研究、监管机构的市场结构公告，以及一篇强化学习论文，都在回答"该怎么交易"——但各自回答的是其中不同的一段。本章先画这张地图，再给出三条穿行其中的阅读路线。

以下每一条陈述都追溯到实际读到的文档，抽取笔记在 `materials/notes/`。凡未能获取的工作，本章一律列为"未获取"，不对其内容作任何断言。

---

## 八大流派

### 1. 市场微观结构基础——成本从哪里来

**要解决的问题。** 在没有任何显式费用之前，一笔委托凭什么有成本？答案来自两个逻辑上互相独立的机制。

**核心方法。** 单一资产、一个中介（dealer 或 market maker）向两类不可观测的客户报价的均衡模型。*库存分支：* Ho–Stoll（1981）给出一个风险厌恶做市商在收益不确定性与到达不确定性下最大化终端财富期望效用，价差 = 风险中性收入项 + 风险溢价，而库存只移动中点、不改变价差。*信息分支：* Glosten–Milgrom（1985）证明即使做市商风险中性、完全竞争、期望利润恰好为零，价差依然存在，因为报价 $A=E[V\mid\text{买入}]$、$B=E[V\mid\text{卖出}]$ 把 $E[V]$ 夹在中间。*策略交易者分支：* Kyle（1985）让知情者主动计划并内生化自身冲击，得到线性系数 $\lambda$（Kyle's lambda，其倒数为市场深度）。*度量：* Roll（1984）仅凭价格自协方差复原有效价差；Amihud–Mendelson（1986）说明流动性不足会被定价。

**它解释不了什么。** 这些模型是单资产、单期收益、且大多流量外生（Kyle 除外）。它们不说明一笔具体的委托该如何排序，还假定基本面良定义——而由它们派生出的经验研究恰恰发现大部分价格变动来自订单流本身，而不是消息。

### 2. 交易成本分析——记账

**要解决的问题。** 成本如何对照某个决策基准来度量，哪个基准才对？

**核心方法。** 把实施差额（implementation shortfall, IS）定义为"纸面组合按决策价成交"与"真实组合"之间的收益差，再分解为延迟 + 执行（冲击）+ 机会 + 费用四部分。Perold（1988）确立决策价基准；Berkowitz–Logue–Noser（1988）引入 VWAP；Leinweber（2002）在 13,651 笔机构买入上度量 IS，发现成本对规模**不单调**，小额委托承担了不成比例的份额；Mittal（2005）主张 IS 才是目标，VWAP/POV 只是工具，并把机会成本定义为成本相对到达价的离散度。

**它解释不了什么。** 它是描述性的——告诉你付了多少，而不是你应该怎么做。它把你可控的成本（排程）与你不可控的成本（市场漂移）混在一起，而且当决策价不可观测时基准很脆弱。

### 3. 随机控制流派

**要解决的问题。** 给定成本模型，选择使期望成本加风险惩罚最小的排程。

**核心方法。** 一个受控的库存/价格过程、一个运行目标，以及由此得到的动态规划或 HJB 方程，逐步放松假设。Bertsimas–Lo（1998）确立动态规划表述，并证明在线性**永久**冲击随机游走下均匀切分最优。Almgren–Chriss（2000）加入价格风险，得到双曲正弦路径、半衰期 $1/\kappa$ 与均值—方差有效前沿。随后是"时间不一致性"被逐个攻克：Schied–Schöneborn（2009）用任意 VNM 效用；Gatheral–Schied（2011）用时间平均 VaR；Forsyth（2011）用数值 HJB 解出整条前沿；Forsyth、Kennedy、Tse 与 Windcliff（2012）用时间一致的"均值—二次变差"准则。瞬态冲击被内生化：Obizhaeva–Wang（2013）建模订单簿弹性，发现最优解是**两个离散块 + 匀速连续交易**；Predoiu–Shaikhet–Shreve（2011）与 Alfonsi–Fruth–Schied（2010）把簿形推广到一般形状；Guéant（2013）说明永久冲击可以非线性且仍然无套利。漂移稳健性由 Lorenz–Schied（2013）处理——在瞬态冲击下策略依赖漂移的**导数**甚至可能不存在——与 Schied（2013）——在 AC 下漂移以积分形式进入、会"平均掉"。Almgren（2003）处理非线性冲击；Guéant（2015）在一般凸成本下给出存在性、Hamiltonian 刻画与大宗交易定价；Lehalle–Neuman（2019）与 Neuman–Voß（2022）把 alpha 信号纳入。Donnelly（2022）逐篇标出谁放松了哪一条假设。

**它解释不了什么。** 冲击参数是输入而非输出；流动性通常被当作给定；而且这个目标函数区分力很弱——Forsyth（2011）发现许多策略的前沿几乎重合。

### 4. 做市与报价流派

**要解决的问题。** 当成交不确定、库存有风险时，双边报价者该如何定价？

**核心方法。** 以指数衰减的成交强度 $Ae^{-k\delta}$ 控制报价距参考价的距离，求解保留价格与价差。Avellaneda–Stoikov（2008）给出保留价格 $r=s-q\gamma\sigma^2(T-t)$ 与"风险项 + 流动性项"的价差分解；Guéant–Lehalle–Fernandez-Tapia（2013）加入硬库存上限、补上此前悬置的验证定理，并给出随库存仿射的 GLFT 闭式价差；Guéant（2017）推广强度形状并扩展到相关多资产，Bergault、Evangelista、Guéant 与 Vieira（2021）为后者给出 Riccati 型 ODE 的闭式代理。Cartea–Jaimungal–Ricci（2014）与 Cartea–Wang（2020）加入自激/互激订单流与 alpha 信号；Guilbaud–Pham（2013）以 tick 时钟建模真实限价簿与限价/市价混合控制；Cont–Stoikov–Talreja（2010）提供底层的排队模型。

**它解释不了什么。** 成交概率大体外生、报价场景接近场外，因此排位没有被求解；而且模型对流动性**吃单方**（做执行的券商）只字未提——那是执行流派要处理的一面镜像。

### 5. 订单簿与微观执行流派

**要解决的问题。** 在排程之下，每一笔子单该放在哪里——市价还是限价、哪个场所、哪个排位、显示多少？

**核心方法。** 给定排队流出量的分布，把下单归结为对少数几个委托量的优化，并把排位与显示当作经济对象。Cont–Kukanov（2017）证明限价/市价切分是一个凸规划，单一场所的解是流出量分布的某个分位数，且跨场所**有意的超量挂单**是最优的；Moallemi–Yuan（2016）把排队位置定价为"成交概率 × 价差 − 逆向选择"；Cebiroğlu–Horst（2011）刻画最优冰山显示为"优先权冲击恰与暴露冲击相抵"，而 Frey–Sandås（2009）证明被识别的冰山反而**吸引**市价单；Buti–Rindi（2009）与 Moinas（2010）给出隐藏的均衡理由及其福利含义；Brunnermeier–Pedersen（2005）证明暴露的平仓需求会招来掠夺性抢跑。Foucault–Kadan–Kandel（2005）提供限价/市价的均衡选择，以"耐心"作为分层原语；Easley–López de Prado–O'Hara（2012）实时度量订单流毒性（VPIN）；Abergel–Jedidi（2013）证明订单簿的扩散极限。

**它解释不了什么。** 这些是单委托或部分均衡模型，而且隐藏是一场胜负未定的识别博弈：高频交易者用非显示标志来抢价与择时波动（Chakrabarty、Hendershott、Nawn、Pascual，2019），而外汇中的拆单反而**更**影响市场（Stenfors–Susai，2021）。

### 6. 市场结构与路由流派

**要解决的问题。** 碎片化、费用、延迟与监管如何改变一笔委托该去哪里？

**核心方法。** 一组监管约束加上市场质量度量。Reg NMS（SEC，2005）把保护报价扫描与接入费上限确立为路由的法律基础，SEC 概念公告（2010）量化碎片化；Foucault–Menkveld（2008）证明路由自动化是竞争场所既可行又"向善"的前提；O'Hara–Ye（2011）发现碎片化平均而言降低成本，而 Haslag–Ringgenberg（2023）证明这个平均掩盖了小盘股更差的结果。费率模型是一阶变量：Comerton-Forde、Grégoire 与 Zhong（2017）证明反向费率场所改善质量；O'Donoghue（2015）证明做市—吃单费率可以既压窄报价价差又抬高总成本。延迟是被实测的"税"：Budish–Cramton–Shim（2015）提出频繁批量拍卖，Aquilina–Budish–O'Neill（2022）把竞速定价在约 0.5 个基点。Kirilenko、Kyle、Samadi 与 Tuzun（2017）是失效模式案例；Mishra–Zhao（2021）梳理路由文献。

**它解释不了什么。** 路由理论高度绑定美股与 NMS 规则，而实证结论随市值大小互相矛盾，所以"场所越多越好"不是定理。订单流付费（Ernst–Spatt，2022）显示这种利益冲突在期权上比股票更严重。

### 7. 学习与自适应流派

**要解决的问题。** 当冲击模型错误或未知时，能否不去推导而是学出一个策略？

**核心方法。** 把执行写成马尔可夫决策过程并学习策略。Nevmyvaka、Feng 与 Kearns（2006）做出第一个大规模 RL 应用，动作是给全部库存定一个价格；Hendricks–Wilcox（2014）对 AC 排程做缩放；Ning、Lin 与 Jaimungal（2018）用 Double DQN；Lin–Beling（2020）用 PPO、以原始 level-2 数据为状态、以相对基准的稀疏奖励为目标；Karpe、Fang、Ma 与 Wang（2020）把"下单"变成一等动作，Cheridito–Weiss（2025）给出最丰富的"排程 + 下单"联合动作。离线与模仿学习见于 Fang 等（2021）——把一个看得见未来的 oracle 蒸馏给只看当前可观测状态的学生——以及 Li–Chen（2025）——对一组经典专家策略做生成式拟合。跨资产泛化是开放端：Kyle–Obizhaeva（2016）给出不变单位，Tran–Yu（2026）发现零样本迁移真实但有限，且在状态分布漂移时失效。Macrì–Lillo（2024）与 Hambly、Xu、Yang（2023）勾勒 RL 与解析解之争。

**它解释不了什么。** 几乎全部结果都是仿真训练出来的；仿真—现实差距与分布外行为仍开放，而 Karpe 等的智能体在它自己的环境里退化成 TWAP。

### 8. 加密与 AMM 执行流派

**要解决的问题。** 没有统一行情带、成本栈分两层（CEX 费率 + 固定 gas）、且以资金费率锚定的永续合约为主力时，该如何执行？

**核心方法。** 跨场所错价的度量加上对 AMM 交易函数的凸优化。Makarov–Schoar（2020）记录持续的跨交易所偏离，Hautsch、Scheuch 与 Voigt（2018/2023）用结算延迟解释它；Barbon–Ranaldo（2021/2024）形式化 CEX—DEX 的规模阈值。Donier–Bonart（2015）在比特币上确认平方根冲击律，Cho、Norman 与 Nunes（2023）拟合出瞬态、价格回复的核。永续方面，He、Manela、Ross 与 von Wachter（2022/2026）给出纳入费率表的、以资金费锚定的套利界，Ackerer、Hugonnier 与 Jermann（2023）给出严格的随机到期定价，Kim–Park（2025）给出资金费率的设计，Donnelly、Lin 与 Lorig（2026）给出含资金费的 AC 平仓解。AMM 方面，Angeris、Agrawal、Evans、Chitra 与 Boyd（2021）把交易写成凸规划，Angeris、Chitra、Evans 与 Boyd（2022）在含固定成本时把它写成混合整数凸规划，Diamandis、Resnick、Chitra 与 Angeris（2023）给出对偶分解路由器；Wu、Sui、Thiery 与 Pai（2025）度量 CEX–DEX 被提取的价值，Xi–Moallemi（2026）度量路由次优。

**它解释不了什么。** 加密度量变化快、场所依赖强，而且流传的"优化完整费率表可省 60%"在任一已获文献中都无法核实（见来源小节）。

---

## 流派之间的继承与对抗

这些流派是一份谱系，它们的分歧与共识同样有用。

- **Glosten–Milgrom 对 Ho–Stoll。** GM 表明逆向选择是价差的**充分**原因，而不只是贡献之一；做市模型则源自 Ho–Stoll。
- **Kyle 对经验冲击文献。** Kyle 的线性、永久、常数 $\lambda$ 冲击，正是平方根律工作（Tóth 等，2011；Bouchaud，2010）所反驳的——冲击是凹的、瞬态的。Guéant（2013）让微观永久冲击依赖已成交量，从而调和两者。
- **Bertsimas–Lo 对 Almgren–Chriss。** BL 只最小化期望成本，得到均匀切分；AC 加入方差惩罚得到双曲正弦路径，说明均匀切分只是风险中性的角落。
- **Almgren–Chriss 对 Obizhaeva–Wang。** AC 把冲击当作交易量的静态函数；OW 把它当作供需的动态，于是光滑的 AC 排程只是"无穷弹性"的特例。
- **均值—方差对时间一致性。** Schied–Schöneborn、Forsyth 与 Forsyth 等都在攻击 AC 的预先承诺问题；Forsyth 证明该准则几乎无法区分不同排程。
- **静态对自适应。** Brigo–Piat 与 Neuman–Voß 说明只有在有 alpha 信号时自适应才有价值；在 CARA 且无信号时，确定性的 AC 路径就是最优（Schied–Schöneborn 推论 4）。
- **连续交易对批量拍卖。** Budish–Cramton–Shim 把竞速称为设计缺陷，Menkveld（2016）反驳说连接碎片化场所的速度是真实的流动性服务。两个渠道都成立。
- **模型驱动对学习驱动。** Macrì–Lillo 发现：当解析最优解存在时 RL 与之持平，只有当冲击是潜在变量、并把价格放进状态时 RL 才超过近似解。

---

## 主文献表

下表覆盖本书全部抽取笔记中出现过的每一篇工作，并标出使用它的章节。状态说明：**全文** = 已获取开放全文并通读；**部分** = 只有官方前置页、预览、样章或作者讲义（商业书）；**未获** = 未获取，原因见后文"未能获取清单"。

### 流派 1——市场微观结构基础

| 流派 | 作者-年份 | 标题 | 关键贡献 | 章节 | 状态 |
|---|---|---|---|---|---|
| 微观基础 | Ho & Stoll (1981) | Optimal Dealer Pricing under Transactions and Return Uncertainty | 风险厌恶做市商：价差 = 风险中性收入项 + 风险溢价；库存只移动中点 | 3 | 全文 |
| 微观基础 | Roll (1984) | A Simple Implicit Measure of the Effective Bid–Ask Spread | 仅凭负自协方差、无需报价的价差估计量 | 3 | 全文 |
| 微观基础 | Glosten & Milgrom (1985) | Bid, Ask and Transaction Prices in a Specialist Market | 逆向选择单独即可产生价差；零利润报价把 $E[V]$ 夹在中间 | 3 | 全文 |
| 微观基础 | Kyle (1985) | Continuous Auctions and Insider Trading | 策略型知情者给出线性永久冲击 $\lambda$，其倒数为深度 | 3 | 全文 |
| 微观基础 | Amihud & Mendelson (1986) | Asset Pricing and the Bid–Ask Spread | 价差被定价：期望收益随价差凹增 | 3 | 未获（付费墙） |
| 微观基础 | Wyart, Bouchaud, Farmer & Lillo (2008) | Relation between Bid–Ask Spread, Impact and Volatility | 价差与冲击线性相关、与每笔波动成正比 | 3 | 全文 |
| 微观基础 | Bouchaud, Farmer & Lillo (2009) | How Markets Slowly Digest Changes in Supply and Demand | 订单流长记忆、流动性有弹性、总量冲击公式 | 3 | 全文 |
| 微观基础 | Tóth, Lempérière, Deremble, de Lataillade, Kockelkoren & Bouchaud (2011) | Anomalous Price Impact and the Critical Nature of Liquidity | 平方根冲击律 $\Delta \propto \sigma\sqrt{Q/V}$ | 3 | 全文 |
| 微观基础 | Cont, Kukanov & Stoikov (2014) | The Price Impact of Order Book Events | 短时价格变动由最优档订单流失衡驱动 | 3 | 全文 |
| 微观基础 | Zarinelli, Treccani, Farmer & Lillo (2015) | Beyond the Square Root | 在五个数量级上对数形式比平方根拟合更好 | 3 | 全文 |
| 微观基础 | Bouchaud (2010) | Price Impact（Encyclopedia of Quantitative Finance） | 凹的、衰减的冲击；是订单预测价格而非冲击价格 | 3, 6, 13 | 全文 |
| 微观基础 | Madhavan (2000) | Market Microstructure: A Survey | 领域综述；价差的库存观与信息观 | 3 | 全文 |
| 微观基础 | Biais, Glosten & Spatt (2005) | Market Microstructure: A Survey of Microfoundations… | 该领域的经典综述 | 3 | 全文 |
| 微观基础 | Bouchaud, Bonart, Donier & Gould (2018) | Trades, Quotes and Prices | 传播子/平方根纲领的金融物理专著 | 3, 5, 6, 7, 12 | 部分（预览 + 目录；全书草稿不存在） |
| 微观基础 | Bouchaud, Farmer & Lillo (2009) | （上述 111 页 arXiv 综述，本书的前身） | 作者自行公开的、专著篇幅的 TQP 第四至八部分理论 | 3, 6 | 全文 |

### 流派 2——交易成本分析

| 流派 | 作者-年份 | 标题 | 关键贡献 | 章节 | 状态 |
|---|---|---|---|---|---|
| 成本分析 | Perold (1988) | The Implementation Shortfall: Paper vs. Reality | 以决策价定义 IS，纸面组合对真实组合 | 4 | 未获（付费墙） |
| 成本分析 | Berkowitz, Logue & Noser (1988) | The Total Cost of Transactions on the NYSE | 引入 VWAP 作为执行质量基准 | 4 | 未获（付费墙） |
| 成本分析 | Perold & Sirri (2002) | Implementation Shortfall: A Practitioner's Guide | IS 框架的实务指南 | 4 | 未获（未找到） |
| 成本分析 | Kissell (2006) | The Expanded Implementation Shortfall | 四分量分解：延迟 + 执行 + 机会 + 费用 | 4, 13 | 未获（付费墙；书目录 + 作者白皮书已读） |
| 成本分析 | Leinweber (2002) | Using Information from Trading in Trading and Portfolio Management | 大样本 IS 度量；成本对规模不单调 | 4 | 全文 |
| 成本分析 | Mittal / ITG (2005) | Implementation Shortfall — One Objective, Many Algorithms | IS 是正确目标；机会成本 = 成本的离散度 | 4 | 全文 |
| 成本分析 | Konishi & Makimoto (2001) / Konishi (2002) | Optimal slice of a block / VWAP trade | 静态最优 VWAP 平仓曲线等于相对成交量曲线 | 4 | 未获（付费墙） |
| 成本分析 | Kissell (2013/2020) | The Science of Algorithmic Trading… / Algorithmic Trading Methods | 商业 TCA 机制；I-Star 冲击模型 | 4, 5, 13 | 未获（目录 + 作者白皮书） |

### 流派 3——随机控制与最优执行

| 流派 | 作者-年份 | 标题 | 关键贡献 | 章节 | 状态 |
|---|---|---|---|---|---|
| 随机控制 | Bertsimas & Lo (1998) | Optimal Control of Execution Costs | 动态规划表述；线性永久冲击下均匀切分最优 | 4, 5 | 全文 |
| 随机控制 | Almgren & Chriss (2000) | Optimal Execution of Portfolio Transactions | 均值—方差前沿、双曲正弦路径、半衰期、与 IS 的联系 | 4, 5 | 全文 |
| 随机控制 | Labadie & Lehalle (2013) | Optimal Starting Times, Stopping Times and Risk Measures | 显式目标收盘与 IS 递推、起止时间、PVol 约束 | 4 | 全文 |
| 随机控制 | Lehalle (2013) | Market Microstructure Knowledge for Intra-Day Trading | 从 AC 排程通向微观结构；排程对路由 | 4, 13 | 全文 |
| 随机控制 | Guéant & Royer (2014) | VWAP Execution and Guaranteed VWAP | 成交量跟踪定理；保证 VWAP 定价 | 4 | 全文 |
| 随机控制 | Guéant (2014) | Execution and Block Trade Pricing with POV | 闭式最优参与率；风险—流动性溢价 | 4, 13 | 全文 |
| 随机控制 | Frei & Westray (2015) | Optimal Execution of a VWAP Order | 闭式 VWAP 跟踪；TWAP 加调整的分解 | 4, 6 | 全文 |
| 随机控制 | Guéant, Lasry & Pu (2014) | Optimal Liquidation with Participation Constraints | 硬参与上限的凸对偶方法 | 4 | 全文 |
| 随机控制 | Obizhaeva & Wang (2013) | Optimal Trading Strategy and Supply/Demand Dynamics | 有弹性时最优排程 = 离散块 + 匀速连续段 | 6 | 全文 |
| 随机控制 | Predoiu, Shaikhet & Shreve (2011) | Optimal Execution in a General One-Sided Limit-Order Book | 冲击由任意簿形导出；Type-A 最优策略 | 6 | 全文 |
| 随机控制 | Alfonsi, Fruth & Schied (2010) | Optimal Execution in LOBs with General Shape Functions | 与簿形无关的最优排程；允许自适应策略 | 6 | 全文 |
| 随机控制 | Guéant (2013) | Permanent Market Impact can be Nonlinear | 依赖成交量的永久冲击，仍然无套利 | 6, 13 | 全文 |
| 随机控制 | Guéant (2015) | Optimal Execution and Block Trade Pricing: a General Framework | 存在性、Hamiltonian 刻画、大宗交易定价 | 6, 13 | 全文 |
| 随机控制 | Schied & Schöneborn (2009) | Risk Aversion and the Dynamics of Optimal Liquidation | 基于效用的自适应策略；CARA 下静态即可 | 6 | 全文 |
| 随机控制 | Lorenz & Schied (2013) | Drift Dependence under Transient Price Impact | 瞬态冲击下策略依赖漂移的导数 | 6 | 全文 |
| 随机控制 | Schied (2013) | Robust Strategies for Optimal Execution in the AC Framework | 时间一致、闭式、与波动率无关的策略 | 6 | 全文 |
| 随机控制 | Forsyth (2011) | A Hamilton–Jacobi–Bellman Approach to Optimal Trade Execution | 数值解出整条前沿；许多策略几乎同样优 | 6 | 全文 |
| 随机控制 | Forsyth, Kennedy, Tse & Windcliff (2012) | Optimal Trade Execution: A Mean–Quadratic-Variation Approach | 时间一致准则；AC 排程即 ABM 最优解 | 6 | 全文 |
| 随机控制 | Brigo & Piat (2016) | Static vs Adapted Optimal Execution Strategies | 量化相对 AC 的自适应价值 | 6 | 全文 |
| 随机控制 | Lehalle & Neuman (2019) | Incorporating Signals into Optimal Trading | 统一 alpha 信号与瞬态冲击 | 6, 7 | 全文 |
| 随机控制 | Neuman & Voß (2022) | Optimal Signal-Adaptive Trading with Temporary and Transient Impact | 完全自适应、时间一致的信号策略 | 6 | 全文 |
| 随机控制 | Liu, Muhle-Karbe & Weber (2017) | Rebalancing with Linear and Quadratic Costs | 成本形式决定交易是块状还是连续 | 4 | 全文 |
| 随机控制 | Donnelly (2022) | Optimal Execution: A Review | 标出各篇放松了哪条 AC 假设 | 6 | 全文 |
| 随机控制 | Almgren (2003) | Optimal Execution with Nonlinear Impact Functions | 幂律冲击与特征时间 | 6 | 未获（付费墙；经 Donnelly §4.4） |
| 随机控制 | Almgren (2009) | Optimal Trading with Stochastic Liquidity and Volatility | 随机 $\sigma_t, k_t$ 的"代入即时值"启发式 | 6 | 未获（付费墙；经 Donnelly） |
| 随机控制 | Almgren & Lorenz (2007) | Adaptive Arrival Price | 顺钱激进/逆钱被动的自适应排程 | 6 | 未获（付费墙） |
| 随机控制 | Gatheral & Schied (2011) | Optimal Trade Execution under GBM in the AC Framework | 时间平均 VaR 准则；与波动率无关的策略 | 6 | 未获（付费墙；经 Schied 2013） |
| 随机控制 | Gatheral (2010) | No-Dynamic-Arbitrage and Market Impact | 无套利迫使永久冲击线性 | 6 | 未获（付费墙；经 Guéant 2013） |
| 随机控制 | Cartea & Jaimungal (2016) | A Closed-Form Execution Strategy to Target VWAP | 两个闭式 VWAP 跟踪策略 | 6 | 未获（404；仅摘要） |
| 随机控制 | Alfonsi & Schied (2010) | Optimal Trade Execution and Absence of Price Manipulations | 限价簿模型中的无价格操纵性 | 6 | 未获（仅元数据） |
| 随机控制 | Cheridito & Sepin (2014) | Optimal Trade Execution under Stochastic Volatility and Liquidity | 随机波动率与流动性的执行 | 6 | 未获（仅 HTML 包装；经 Donnelly） |

### 流派 4——做市与报价

| 流派 | 作者-年份 | 标题 | 关键贡献 | 章节 | 状态 |
|---|---|---|---|---|---|
| 做市 | Avellaneda & Stoikov (2008) | High-Frequency Trading in a Limit Order Book | 保留价格 $r = s - q\gamma\sigma^2(T-t)$；风险 + 流动性价差 | 7 | 全文 |
| 做市 | Guéant, Lehalle & Fernandez-Tapia (2013) | Dealing with the Inventory Risk | 硬库存上限；验证定理；GLFT 闭式价差 | 7 | 全文 |
| 做市 | Guéant (2017) | Optimal Market Making | 一般强度形状；CARA 等价于运行惩罚；多资产 | 7 | 全文 |
| 做市 | Bergault, Evangelista, Guéant & Vieira (2021) | Closed-Form Approximations in Multi-Asset Market Making | 相关资产报价的 Riccati-ODE 代理 | 7 | 全文 |
| 做市 | Cartea, Jaimungal & Ricci (2014) | Buy Low, Sell High: A High-Frequency Trading Perspective | Hawkes 自激/互激流；信号驱动的非对称报价 | 7 | 全文 |
| 做市 | Cartea & Wang (2020) | Market Making with Alpha Signals | 跳跃中点价；alpha 在限价与投机市价单间切换 | 7 | 全文 |
| 做市 | Guilbaud & Pham (2013) | Optimal High Frequency Trading with Limit and Market Orders | 真实限价簿：tick 时钟、价格优先、混合控制 | 7 | 全文 |
| 做市 | Cont, Stoikov & Talreja (2010) | A Stochastic Model for Order Book Dynamics | 订单簿排队模型；闭式成交与移动概率 | 7 | 全文 |
| 做市 | Fodra & Labadie (2012) | HF Market Making with Inventory Constraints and Directional Bets | 非鞅中点价；风险厌恶调节整个盈亏分布 | 7 | 全文 |
| 做市 | Drissi (2024) | Lecture Notes on Market Microstructure and Algorithmic Trading | 牛津课程，桥接 AS/GLFT 与 Cartea–Jaimungal 记号 | 7 | 全文 |
| 做市 | Cartea, Jaimungal & Penalva (2015) | Algorithmic and High-Frequency Trading | 专著：指数效用下的执行与做市 | 5, 6, 7, 10 | 部分（预览 + 摘录） |

### 流派 5——订单簿微观执行、下单与隐匿

| 流派 | 作者-年份 | 标题 | 关键贡献 | 章节 | 状态 |
|---|---|---|---|---|---|
| 微观执行 | Cont & Kukanov (2017) | Optimal Order Placement in Limit Order Markets | 凸的限价/市价切分 = 排队流出量的分位数；超量挂单最优 | 8 | 全文 |
| 微观执行 | Cartea & Jaimungal (2016) | Incorporating Order-Flow into Optimal Execution | AC 排程加未来净订单流的加权期望 | 5, 8 | 全文 |
| 微观执行 | Abergel & Jedidi (2013) | A Mathematical Approach to Order Book Modeling | 马尔可夫订单簿的遍历性与扩散极限 | 8 | 全文 |
| 微观执行 | Moallemi & Yuan (2016) | A Model for Queue Position Valuation in a Limit Order Book | 排队价值 = 成交概率 × 价差 − 逆向选择 | 8 | 全文 |
| 微观执行 | Cebiroğlu & Horst (2011) | Optimal Display of Iceberg Orders | 最优显示使优先权冲击与暴露冲击相抵 | 8 | 全文 |
| 微观执行 | Frey & Sandås (2009) | The Impact of Iceberg Orders in Limit Order Books | 被识别的冰山吸引市价单；隐藏流动性是非知情的 | 8 | 全文 |
| 微观执行 | Buti & Rindi (2009) | Hidden Orders and Optimal Submission Strategies | 隐藏单是应对被抢价风险的均衡策略 | 8 | 全文 |
| 微观执行 | Moinas (2010) | Hidden Limit Orders and Liquidity in Order-Driven Markets | 信息不对称下不透明改善流动性与福利 | 8 | 全文 |
| 微观执行 | Foucault, Kadan & Kandel (2005) | Limit Order Book as a Market for Liquidity | 耐心把交易者分层为流动性供给与需求方 | 8 | 全文 |
| 微观执行 | Brunnermeier & Pedersen (2005) | Predatory Trading | 暴露的平仓招来抢跑；主张不强制披露持仓 | 8 | 全文 |
| 微观执行 | Zhu (2014) | Do Dark Pools Harm Price Discovery? | 暗池未必损害价格发现 | 8 | 全文 |
| 微观执行 | Easley, López de Prado & O'Hara (2012) | Flow Toxicity and Liquidity in a High-Frequency World | VPIN：实时、成交量时间的订单流毒性度量 | 8 | 全文 |
| 微观执行 | Chakrabarty, Hendershott, Nawn & Pascual (2019) | Order Exposure in High-Frequency Markets | 高频交易者用非显示标志抢价与择时波动 | 8 | 全文 |
| 微观执行 | Degryse, Tombeur & Wuyts (2015) | Two Shades of Opacity | 隐藏单与暗池交易是（不完美）替代品 | 8 | 全文 |
| 微观执行 | Stenfors & Susai (2021) | Stealth Trading in FX Markets | 拆单可被识别，且更影响市场 | 8 | 全文 |
| 微观执行 | Esser & Mönch (2007) | The Navigation of an Iceberg | 隐藏单的最优使用 | 8 | 未获（503/429） |
| 微观执行 | Cartea & Jaimungal (2015) | Optimal Execution with Limit and Market Orders | 最优限价/市价组合（2016 年论文的前作） | 5, 8 | 未获（付费墙） |
| 微观执行 | Yang & Zhu (2020) | Back-Running: Seeking and Hiding Fundamental Information | 订单抢先/回跑 | 8 | 未获（403） |
| 微观执行 | Degryse, de Jong & van Kervel (2015) | The Impact of Dark Trading and Visible Fragmentation | 暗池交易与可见碎片化的影响 | 8 | 未获（超时） |
| 微观执行 | Hasbrouck (2007) | Empirical Market Microstructure | 教材；Roll 模型、VAR 分解、信息份额 | 3, 8, 9 | 部分（作者 NYU 讲义 + 样章） |

### 流派 6——市场结构与路由

| 流派 | 作者-年份 | 标题 | 关键贡献 | 章节 | 状态 |
|---|---|---|---|---|---|
| 路由 | SEC (2005) | Regulation NMS (Final Rule) | 订单保护、接入、次分币规则；SOR 的约束集 | 9 | 全文 |
| 路由 | SEC (2010) | Concept Release on Equity Market Structure | 碎片化的官方快照；显示与未显示流动性 | 9 | 全文 |
| 路由 | SEC Staff (2013) | Equity Market Structure Literature Review I: Fragmentation | 可见与暗碎片化；小盘股收益递减 | 9 | 全文 |
| 路由 | SEC Staff (2014) | Equity Market Structure Literature Review II: HFT | 高频交易事实上的流动性供给方 | 9 | 全文 |
| 路由 | Foucault & Menkveld (2008) | Competition for Order Flow and Smart Order Routing Systems | SOR 普及使新场所可行并降低合并成本 | 9 | 全文 |
| 路由 | Hendershott, Jones & Menkveld (2011) | Does Algorithmic Trading Improve Liquidity? | autoquote 自然实验：算法交易经由更少逆向选择压窄价差 | 9 | 全文 |
| 路由 | Kirilenko, Kyle, Samadi & Tuzun (2017) | The Flash Crash: HFT in an Electronic Market | 失效模式：不顾价格的委托遇上流失的深度 | 9, 12 | 全文 |
| 路由 | Budish, Cramton & Shim (2015) | The HFT Arms Race: Frequent Batch Auctions | 连续限价簿是设计缺陷；批量拍卖 | 9 | 全文 |
| 路由 | Menkveld (2016) | The Economics of High-Frequency Trading: Taking Stock | 数据加权的综述；连接场所的速度是一种服务 | 9 | 全文 |
| 路由 | Aquilina, Budish & O'Neill (2022) | Quantifying the HFT "Arms Race" | 竞速约每分钟一次、5–10 微秒、约 0.5 基点税、占流动性成本 17% | 9, 13 | 全文 |
| 路由 | Mishra & Zhao (2021) | Order Routing Decisions for a Fragmented Market: A Review | SOR 决定因素的梳理；各场所费率表 | 9 | 全文 |
| 路由 | Comerton-Forde, Grégoire & Zhong (2017) | Inverted Fee Venues and Market Quality | 费率模型是一阶路由变量 | 9 | 全文 |
| 路由 | O'Donoghue (2015) | The Effect of Maker-Taker Fees on Order Choice | 做市—吃单费率可以压窄价差同时抬高总成本 | 9 | 全文 |
| 路由 | Ernst & Spatt (2022) | Payment for Order Flow and Asset Choice | 订单流付费在股票上小、在期权上大 | 9 | 全文 |
| 路由 | Haslag & Ringgenberg (2023) | The Demise of the NYSE and Nasdaq | 碎片化利好大盘股、损害小盘股 | 9 | 全文 |
| 路由 | SEC (2022) | Order Competition Rule (Proposed) | 迫使零售可成交委托进入公开拍卖 | 9 | 全文 |
| 路由 | O'Hara & Ye (2011) | Is Market Fragmentation Harming Market Quality? | 碎片化平均降低交易成本并加快成交 | 9 | 未获（付费墙） |
| 路由 | Johnson (2010) | Algorithmic Trading & DMA | DMA/执行方法分类与基础设施 | 8, 13 | 未获（仅目录；站点已失效） |

### 流派 7——学习与自适应

| 流派 | 作者-年份 | 标题 | 关键贡献 | 章节 | 状态 |
|---|---|---|---|---|---|
| 学习 | Nevmyvaka, Feng & Kearns (2006) | Reinforcement Learning for Optimized Trade Execution | 首个大规模 RL 执行；动作是对全库存报一个价 | 10 | 全文 |
| 学习 | Azencott, Beri, Gadhyan, Joseph, Lehalle & Rowley (2013) | Realtime Market Microstructure Analysis: Online TCA | 闭环、横截面的 TCA；异常检测因子 | 10 | 全文 |
| 学习 | Hendricks & Wilcox (2014) | A RL Extension to the Almgren–Chriss Framework | RL 缩放 AC 排程；中位 IS 最多改善 10.3% | 10 | 全文 |
| 学习 | Kyle & Obizhaeva (2016) | Market Microstructure Invariance: Empirical Hypotheses | 业务时间下的不变单位；平方根是唯一形状 | 3, 10 | 全文 |
| 学习 | Ning, Lin & Jaimungal (2018/2021) | Double Deep Q-Learning for Optimal Execution | DQN 在 9 只票中的 7 只上胜过 TWAP | 10 | 全文 |
| 学习 | Karpe, Fang, Ma & Wang (2020) | Multi-Agent RL in a Realistic LOB Simulation | 首个"执行 × 下单"联合动作；退化为 TWAP | 10 | 全文 |
| 学习 | Lin & Beling (2020) | An End-to-End Optimal Trade Execution Framework Based on PPO | 原始 level-2 状态、相对基准的稀疏奖励 | 10 | 全文 |
| 学习 | Fang et al. (2021) | Universal Trading for Order Execution with Oracle Policy Distillation | 从看得见未来的 oracle 蒸馏；一个策略跨多标的 | 10 | 全文 |
| 学习 | Pan, Zhang, Luo, He & Liu (2022) | Learn Continuously, Act Discretely (HALOP) | 连续选范围 + 离散选 tick 的限价下单 | 10 | 全文 |
| 学习 | Hambly, Xu & Yang (2023) | Recent Advances in RL in Finance | 综述：MDP 形式化与六类应用 | 10 | 全文 |
| 学习 | Nagy, Calliess & Zohren (2023) | Asynchronous Deep Double Dueling Q-Learning | RL 把 alpha 信号转成逐单限价挂单并可跳过 | 7, 10 | 全文 |
| 学习 | Macrì & Lillo (2024) | RL for Optimal Execution when Liquidity is Time-Varying | 潜在时变冲击；只有价格在状态里 RL 才胜过 AC | 10 | 全文 |
| 学习 | Micheli & Monod (2024) | Deep RL for Online Optimal Execution Strategies | 带历史动作的 DDPG 处理瞬态非马尔可夫冲击 | 10 | 全文 |
| 学习 | Zhou, Chen & Xu (2024) | Two Kinds of Learning Algorithms for Continuous-Time VWAP Targeting | 显式高斯最优策略；ADP 需已知环境 | 10 | 全文 |
| 学习 | Li & Chen (2025) | FlowOE: Imitation Learning with Flow Matching | 对经典专家做模仿并超越；Heston + 凹冲击 | 10 | 全文 |
| 学习 | Olby, Bacalum, Baggott & Stillman (2025) | Right Place, Right Time: Market-Simulation-Based RL | 反应式仿真中 RL 逼近 AC 前沿 | 10 | 全文 |
| 学习 | Cheridito & Weiss (2025) | RL for Trade Execution with Market and Limit Orders | 最丰富的联合状态/动作：单纯形上的比例、档位、撤单 | 10 | 全文 |
| 学习 | Tran & Yu (2026) | Does a Trading Policy Need to Know What It Trades? | 零样本迁移有限，且在状态漂移下失效 | 10 | 全文 |
| 学习 | Kolm & Ritter (2019) | Dynamic Replication and Hedging: A Reinforcement Learning Approach | 有摩擦下的 RL 对冲 | 10 | 未获（付费墙） |

### 流派 8——加密与 AMM 执行

| 流派 | 作者-年份 | 标题 | 关键贡献 | 章节 | 状态 |
|---|---|---|---|---|---|
| 加密 | Makarov & Schoar (2020) | Trading and Arbitrage in Cryptocurrency Markets | 持续的跨交易所偏离；峰值约每日 USD 2bn | 11 | 全文 |
| 加密 | Hautsch, Scheuch & Voigt (2018/2023) | Building Trust Takes Time | 结算延迟解释了套利限制 | 11 | 全文 |
| 加密 | Barbon & Ranaldo (2021/2024) | On the Quality of Cryptocurrency Markets | 两层成本栈；小额 CEX 便宜、大额 DEX 便宜 | 11 | 全文 |
| 加密 | Wu, Sui, Thiery & Pai (2025) | Measuring CEX-DEX Extracted Value | 19 个套利者提取 USD 233.8m；头部占四分之三 | 11 | 全文 |
| 加密 | Plazuelo Pascual et al. (2025) | Price Discovery in Cryptocurrency Markets | 哪个场所是有效价的参照 | 11 | 全文 |
| 加密 | Aleti & Mizrach (2021) | Bitcoin Spot and Futures Market Microstructure | BTC 现货/CME 的价差与冲击基线 | 11 | 未获（付费墙；仅摘要） |
| 加密 | Agentic Sciences (2023/2026) | Order Book Microstructure of Bitcoin: Spot vs Perpetual | 永续深度 3.1 倍但报价价差更宽 | 11 | 全文 |
| 加密 | Easley, O'Hara, Yang & Zhang (2024) | Microstructure and Market Dynamics in Crypto Markets | Roll 与 VPIN 能预测加密动态，且在寒冬中稳定 | 11 | 全文 |
| 加密 | Donier & Bonart (2015) | A Million Metaorder Analysis of Market Impact on the Bitcoin | 平方根律跨四个数量级；冲击会衰减 | 11, 13 | 全文 |
| 加密 | Cho, Norman & Nunes (2023) | PRIME: A Price-Reverting Impact Model | 瞬态、价格回复的 BTC 冲击核 | 11 | 全文 |
| 加密 | He, Manela, Ross & von Wachter (2022/2026) | Fundamentals of Perpetual Futures | 纳入费率表、以资金费锚定的套利界；Sharpe 3.35 至 11.65 | 11, 13 | 全文 |
| 加密 | Ackerer, Hugonnier & Jermann (2023) | Perpetual Futures Pricing | 永续价 = 均值为 $1/\kappa$ 的随机时刻的现货风险中性期望 | 11 | 全文 |
| 加密 | Kim & Park (2025) | Designing Funding Rates for Perpetual Futures | 用倒向 SDE 设计路径依赖的资金费率 | 11 | 全文 |
| 加密 | Donnelly, Lin & Lorig (2026) | Optimal Liquidation of Perpetual Contracts | 含资金费的 AC 平仓；闭式与短时扰动 | 11 | 全文 |
| 加密 | Angeris, Agrawal, Evans, Chitra & Boyd (2021) | Constant Function Market Makers: Multi-Asset Trades via Convex Optimization | CFMM 交易是凸规划；对偶变量即价格 | 11 | 全文 |
| 加密 | Angeris, Chitra, Evans & Boyd (2022) | Optimal Routing for Constant Function Market Makers | 无固定成本时路由是凸的，有则为 MICP | 11 | 全文 |
| 加密 | Diamandis, Resnick, Chitra & Angeris (2023) | An Efficient Algorithm for Optimal Routing Through CFMMs | 对偶分解路由器；支持 v3 与多个费率档 | 11 | 全文 |
| 加密 | Xi & Moallemi (2026) | Quantifying Sub-Optimality in Routing for AMs | DEX 路由每笔损失 2.02 基点，主要是状态时效问题 | 11 | 全文 |
| 加密 | Zhang & Tessone (2025) | Measuring DEX Efficiency and the Effect of an Enhanced Routing Method | 以标准化总套利利润度量场所质量 | 11 | 全文 |
| 加密 | Cartea, Drissi & Monga (2023/2025) | DeFi and Automated Market Making: Execution and Speculation | AMM 版 AC；凸性成本随机；CEX–DEX 联合优化 | 11 | 全文 |
| 加密 | Cartea, Drissi & Monga (2023/2024) | DeFi and AMM: Predictable Loss and Optimal Liquidity Provision | 可预测损失（集中流动性版 LVR）；最优区间宽度 | 11 | 全文 |
| 加密 | Lehar, Parlour & Zoican (2023/2024) | Fragmentation and Optimal Liquidity Supply on DEXs | 高费率池占 58% 流动性、21% 成交量 | 11 | 全文 |
| 加密 | Canidio & Fritsch (2023/2025) | Arbitrageurs' Profits, LVR and Sandwich Attacks | 批量化交易（FM-AMM）消除 LVR 与三明治 | 11 | 全文 |
| 加密 | Kulkarni, Diamandis & Chitra (2022/2023) | Towards a Theory of MEV I: CFMMs | 常数无政府代价；三明治损害为 $O(\log n)$ | 11 | 全文 |
| 加密 | Di Nosse & Lillo (2026) | Mitigating Adverse Selection in CL AMMs with Dynamic Fees | 动态费率更多是补偿 LVR 而非减少 LVR | 11 | 全文 |
| 加密 | Barone & Lillo (2026) | Trading in the Sunshine or in the Shade (Hyperliquid) | 公开 TWAP 使自身冲击降低约 9 基点、逆向选择降低约 5 基点 | 11 | 全文 |
| 加密 | Binance (2026) | Fees & Transactions Overview (Spot & Margin) | 完整的分层做市/吃单费率表 | 11 | 全文（官方文档） |

---

## 三条阅读路径

### 路线 A｜建立直觉（一到两周）

1. **Almgren–Chriss（2000），第 1–3 节。** 双曲正弦路径、半衰期与前沿。*读完你应该能解释：为什么交易更快会抬高冲击却降低风险，并能画出前沿。*
2. **Bertsimas–Lo（1998），第 2–3 节。** *为什么只有在线性永久冲击下均匀切分才最优？*
3. **Kyle（1985）与 Glosten–Milgrom（1985）。** *零费用下价差为何存在？Kyle 的 $\lambda$ 是什么，其倒数为何称为深度？*
4. **Bouchaud（2010）与 Tóth 等（2011）。** *平方根律是什么，为什么冲击必须瞬态而非永久？*
5. **Cont–Kukanov（2017）。** *为什么限价/市价切分是排队流出量的分位数？*

跳过 AC，后面所有关于排程的词你都会看不懂；跳过 Kyle/GM，你会把每一个"冲击项"误读成费用；跳过 Bouchaud/Tóth，你会去校准一个数据拒绝的线性冲击模型；跳过 Cont–Kukanov，下单就只剩工程而不再是优化。

### 路线 B｜做出可用的执行系统

**理论。** 用 AC（2000）定排程；用 Obizhaeva–Wang（2013）判断排程何时必须包含离散块；用 Guéant（2015）处理一般凸成本、参与上限与大宗交易定价；用 Donnelly（2022）确认每条扩展放松了哪条假设。若要对标 VWAP 与 POV，再加 Guéant–Royer（2014）、Frei–Westray（2015）与 Guéant（2014）。

**校准。** 用 Bouchaud（2010）、Tóth 等（2011）与 Zarinelli 等（2015）定冲击律——默认用平方根，在大参与率处核对对数形式；用 Kyle–Obizhaeva（2016）做跨资产归一；用 Cont–Kukanov–Stoikov（2014）估计失衡对冲击的斜率；用 Labadie–Lehalle（2013）得到带 PVol 上限的显式 IS 与目标收盘递推。加密场景再加 Donier–Bonart（2015）与 Cho 等（2023）。

**工程。** 用 Cont–Kukanov（2017）与 Moallemi–Yuan（2016）处理下单与排队价值；用 Foucault–Menkveld（2008）、Comerton-Forde 等（2017）与 O'Donoghue（2015）处理场所与费率逻辑；用 Aquilina–Budish–O'Neill（2022）给那些赢不了的延迟定价；用 Hasbrouck 的 NYU 讲义（第 16–17、19 章）处理 TCA 与拆单机制；用 Azencott 等（2013）做闭环 TCA。

跳过 Obizhaeva–Wang，你的算法永远不会去做弹性使其最优的开盘或收盘块；跳过校准文献，你的 $\eta$ 就是猜的；跳过费率模型文献，你的路由器优化了报价价差却在支付错误的总成本。

### 路线 C｜做研究

从 **Donnelly（2022）**（最优执行）与 **Hambly、Xu、Yang（2023）**（学习）进入，两篇都标定了该领域现状与开放端。再以 **Guéant（2015）** 与 **Donnelly（2022）第 4 节** 为解析主干，挑一条活跃切口：信号与执行（Lehalle–Neuman，2019；Neuman–Voß，2022）；瞬态冲击与稳健性（Lorenz–Schied，2013；Schied，2013；Predoiu–Shaikhet–Shreve，2011）；排程与下单联合（Cheridito–Weiss，2025；Cont–Kukanov，2017）；泛化（Kyle–Obizhaeva，2016；Tran–Yu，2026）；或加密（Cartea–Drissi–Monga，2023；Xi–Moallemi，2026；Barone–Lillo，2026）。

跳过综述，你会把已有结论重新推一遍；跳过具体的切口文献，你会把一个 2022 年已被解决的问题当作开放问题来报告。

---

## 开放问题

标注为"**本章判断**"的条目是本章自己的读法，不是某来源的结论。

1. **非线性与瞬态冲击下的稳健最优解。** AC 排程只在线性冲击与鞅价格下严格最优；在瞬态冲击下策略依赖漂移的导数，甚至可能不存在（Lorenz–Schied，2013）。*为何未解决：* 无套利的永久冲击是非线性的（Guéant，2013），而衰减核是估计出来的、并非已知。最接近：Predoiu–Shaikhet–Shreve（2011）；Guéant（2015）。
2. **信号与执行的联合优化。** 静态信号解（Lehalle–Neuman，2019）只在初始时刻用信号；自适应解（Neuman–Voß，2022）需要非退化条件。*为何未解决：* 带自身衰减的 alpha 与瞬态冲击畸变之间的一般交互尚未求解。最接近：Neuman–Voß（2022）；Cartea–Jaimungal（2016）。
3. **RL 的分布外泛化与仿真—现实差距。** 策略多在回放或合成环境中训练；Karpe 等的智能体退化为 TWAP，而不变单位只买来有限迁移（Tran–Yu，2026）。*为何未解决：* 尚无公认的分布外评测协议。最接近：Tran–Yu（2026）。
4. **加密费率表与资金费率的联合优化。** 资金时点与分层做市/吃单费率被分开建模（He 等，2022/2026；Donnelly 等，2026）；"瞄准哪一档、做市还是吃单、何时持有/平掉资金头寸"并不是同一个问题。最接近：He 等（2022/2026）；Kim–Park（2025）。
5. **延迟与费用下跨场所路由的最优分解。** 无固定成本时路由是凸的、有则是 MICP（Angeris 等，2022）；实测次优很小，且主要是状态时效问题（Xi–Moallemi，2026）。*为何未解决：* 延迟—费率—纳入的权衡是结算窗口上的控制问题，而非静态规划。最接近：Xi–Moallemi（2026）；Hautsch 等（2018/2023）。
6. **不可观测的信息半衰期。** 执行与逆向选择取决于私有信息渗入价格的速度，而该半衰期是潜在变量。Roll（1984）与 Kyle（1985）把它当作给定，Macrì–Lillo（2024）能推断潜在冲击但推不出半衰期。最接近：Macrì–Lillo（2024）；Easley 等（2012）。
7. **如何让延迟竞争与其社会价值相协调。** 批量拍卖能消除竞速（Budish 等，2015），但连接碎片化场所的速度是真实服务（Menkveld，2016）。*为何未解决：* 两条渠道各测各的，从未在同一反事实里相抵。最接近：Aquilina–Budish–O'Neill（2022）。
8. **把"显示"与"隐藏"作为一个组合来优化。** 显示规模在优先权与暴露之间权衡（Cebiroğlu–Horst，2011），隐藏单与暗池互为替代（Degryse 等，2015），公开 TWAP 降低成本（Barone–Lillo，2026）。*为何未解决：* 尚无模型在考虑对手识别反应的前提下，把委托类型、显示规模、拆单与场所联合优化。最接近：Barone–Lillo（2026）；Buti–Rindi（2009）。**本章判断。**

---

## 来源交代：这些资料是怎么拿到的

**读到全文（开放获取）。** 占绝大多数：流派 3、5、6、7、8 中标为"全文"的每一篇——Almgren–Chriss、Bertsimas–Lo、Obizhaeva–Wang、Predoiu–Shaikhet–Shreve、Alfonsi–Fruth–Schied、各篇 Guéant、Schied 与 Lorenz–Schied、Forsyth、Brigo–Piat、Lehalle–Neuman、Neuman–Voß、Donnelly、Kyle–Obizhaeva，除 Kolm–Ritter 外的全部 RL 论文，以及加密各篇。另有少量经验冲击类论文（Wyart 等 2008；Madhavan 2000；Biais–Glosten–Spatt 2005；Tóth 等 2011；Cont–Kukanov–Stoikov 2014；Zarinelli 等 2015）存于本地语料并由第 3、13 章使用；本章将它们列入表内，但它们没有单独的集群抽取笔记。

**未获取正文的商业书，及其替代材料。**

- **O'Hara《Market Microstructure Theory》（1995）。** 无合法全文。替代：出版社完整的 9 章目录，以及 O'Hara 本人 2015 年发表于 *Journal of Financial Economics* 的综述——它重述了"库存 / 信息 / 策略交易者"三支柱，并把"速度"列为第四种成本。
- **Hasbrouck《Empirical Market Microstructure》（2007）。** 替代：作者本人 202 页、22 章的 NYU 教学书稿《Securities Trading: Principles and Procedures》（v14c，2025）；出版社提供的两章免费样章；作者的勘误；以及含 2007 年目录的 21 页预览。
- **Harris《Trading and Exchanges》（2003）。** 替代：出版社 66 页预览（完整 29 章目录，第 1–2 章全文）与作者免费公开的 CFA Institute Research Foundation 卷《Trading and Electronic Markets》（2015）。另有一份 2002 年出版前草稿挂在第三方课程页上，但它不是权利人发布，故未使用。
- **Cartea–Jaimungal–Penalva《Algorithmic and High-Frequency Trading》（2015）。** 仅有出版社预览与摘录；执行与做市部分由其作者本人的 arXiv/ORA 论文与 Drissi 的牛津讲义替代。
- **Guéant《The Financial Mathematics of Market Liquidity》（2016）。** 由其作者本人的两套完整讲义替代（422 页，CFM–Imperial 2016，覆盖第一、二部分；578 页，KAUST 2021，做市那一半），另加出版社预览。
- **Lehalle–Laruelle《Market Microstructure in Practice》（2013/2018）。** 由作者本人的 IPAM/UCLA 2015 讲义系列替代（7 页概览、65 页讲义）。
- **Kissell《The Science of Algorithmic Trading…》（2013）/《Algorithmic Trading Methods》（2020）。** 出版社前置页（20 章目录）与作者本人 2015 年含 I-Star 模型的多资产成本模型白皮书。
- **Bouchaud、Bonart、Donier、Gould《Trades, Quotes and Prices》（2018）。** 第 3–24 章未获取；本次核查确认**公开渠道不存在全书草稿**。替代：47 页预览（第 1–2 章全文）与目录，以及同作者 111 页的 arXiv 专著《How markets slowly digest changes in supply and demand》。
- **Johnson《Algorithmic Trading & DMA》（2010）。** 仅拿到目录——作者唯一的渠道 `algotradingdma.com` 已失效，且不存在机构存档或免费发布。这是九本中唯一只拿到目录的一本。

**方法论声明。** 本章每条陈述都追溯到实际读到的文档。对任何未获取的工作都不凭记忆描述：标为"未获"的条目只贡献其摘要、出版社前置页，或**确实读过**的引用论文所能归属给它的内容。

**存疑声明。** "针对完整费率表做执行优化可带来超过 60% 的潜在节省"这一说法无法核实。所有已获取文献都被检索过，没有任何一段报告过由费率表优化带来的超过 60% 的节省。相邻的那些 60% 数字含义都不同（年化基差偏离、噪音交易占比、样本覆盖率），而最接近的实测执行节省是 Xi–Moallemi 的 2.02 个基点路由次优。在有人给出具体出处之前该数字无来源，本章不把它当作事实重复。（同一结论也记录在第 11 章同名小节。）

---

## 未能获取清单

| 作者-年份 | 标题 | 尝试了什么 | 为什么拿不到 | 读者可去哪里找 |
|---|---|---|---|---|
| Amihud & Mendelson (1986) | Asset Pricing and the Bid-Ask Spread | Unpaywall、NYU 课程链接、RePEc、罗切斯特工作论文 | 出版社链接返回机器人页；无仓储副本 | ScienceDirect（订阅）；RePEc/IDEAS 摘要 |
| Perold (1988) | The Implementation Shortfall: Paper vs. Reality | HBS 教职页、通用检索 | *Journal of Portfolio Management* 仅订阅 | HBS 教职页（摘要）；图书馆 JPM 14(3) |
| Berkowitz, Logue & Noser (1988) | The Total Cost of Transactions on the NYSE | 出版社、仓储检索 | Wiley *Journal of Finance* 仅订阅 | 图书馆 JF 43(1) |
| Perold & Sirri (2002) | Implementation Shortfall: A Practitioner's Guide | 检索 | 未找到开放 PDF | 出版社/手册卷 |
| Konishi & Makimoto (2001) / Konishi (2002) | Optimal slice of a block / of a VWAP trade | Elsevier、Risk.net | 仅订阅 | 图书馆 *Journal of Risk* 3(4) / *JFM* 5 |
| Kissell (2006) | The Expanded Implementation Shortfall | 作者站点、期刊 | *Journal of Trading* 付费墙 | CFA 备考材料；Kissell 的书 |
| Kissell (2013/2020) | The Science of Algorithmic Trading… / Algorithmic Trading Methods | 出版社、作者站点 | Elsevier 不发布免费章节；仅前置页与白皮书 | 出版社页面；图书馆藏本 |
| Almgren (2003) | Optimal Execution with Nonlinear Impact Functions | Taylor & Francis、作者页 | 出版社付费墙；作者页 403/404 | *Applied Mathematical Finance* 10(1)；Donnelly (2022) §4.4 |
| Almgren (2009) | Optimal Trading with Stochastic Liquidity and Volatility | SIAM、预印本 | SIAM 付费墙；无开放预印本 | *SIAM J. Financial Mathematics* 3(1)；Donnelly (2022) |
| Almgren & Lorenz (2007) | Adaptive Arrival Price | Institutional Investor / PM Research | 付费墙 | *Journal of Trading* 2(1) |
| Gatheral & Schied (2011) | Optimal Trade Execution under GBM in the AC Framework | World Scientific、arXiv、作者页 | 付费墙；不在 arXiv | *IJTAF* 14(3)；Schied (2013) 中引用 |
| Gatheral (2010) | No-Dynamic-Arbitrage and Market Impact | Quantitative Finance、SSRN | 付费墙；SSRN 反爬 | *Quantitative Finance* 10(7)；Guéant (2013) 中重述 |
| Cartea & Jaimungal (2016) | A Closed-Form Execution Strategy to Target VWAP | Oxford ORA、arXiv | ORA 下载端点 404；无 arXiv 版 | Oxford ORA 记录；*SIAM J. Financial Mathematics* 7(1) |
| Alfonsi & Schied (2010) | Optimal Trade Execution and Absence of Price Manipulations | HAL、SSRN | HAL 仅元数据；SSRN 反爬 | HAL `hal-00397652`；*SIAM J. Financial Mathematics* 1(1) |
| Cheridito & Sepin (2014) | Optimal Trade Execution under Stochastic Volatility and Liquidity | Princeton OAR | 端点返回 HTML 包装 | 预印本服务器；Donnelly (2022) §4.1 |
| Esser & Mönch (2007) | The Navigation of an Iceberg | EFMA 2005 会议版、Wayback | 站点 503；Wayback 限流（429） | *Finance Research Letters* 4(2) |
| Cartea & Jaimungal (2015) | Optimal Execution with Limit and Market Orders | arXiv、出版社 | 无开放预印本；出版社付费墙 | *Quantitative Finance* 15(8)；可改用 2016 年前作 |
| Yang & Zhu (2020) | Back-Running | MIT 作者页、SSRN | MIT 页 403；SSRN 需登录 | *Review of Financial Studies* 33(4) |
| Degryse, de Jong & van Kervel (2015) | The Impact of Dark Trading and Visible Fragmentation | Tilburg、CEPR | 站点超时/返回 HTML | *Review of Finance* 19(4) |
| O'Hara & Ye (2011) | Is Market Fragmentation Harming Market Quality? | ScienceDirect、SSRN、Columbia Law | 付费墙；SSRN 需登录；Columbia 站点失效；Wayback 429 | *JFE* 100(3)；SEC Staff (2013) 中概述 |
| Kolm & Ritter (2019) | Dynamic Replication and Hedging | 期刊、作者幻灯片、SSRN | *JFD* 付费墙；仅幻灯片与一份伴随综述开放 | *Journal of Financial Data Science* 1(1) |
| Aleti & Mizrach (2021) | Bitcoin Spot and Futures Market Microstructure | Wiley、SSRN、仓储 | Wiley 付费墙；SSRN 登录墙 | *Journal of Futures Markets* 41(2)；SSRN 摘要 |
| Johnson (2010) | Algorithmic Trading & DMA | 出版社站点、Wayback、检索 | 自出版；唯一渠道已失效 | 图书馆/二手书；目录已在本地语料中 |

---

## 本章依据的文献

除上表所列外，本章直接依据的主要文献如下。

- Albert S. Kyle, "Continuous Auctions and Insider Trading", *Econometrica* 53(6), 1315–1336 (1985)。
- Lawrence R. Glosten and Paul R. Milgrom, "Bid, Ask and Transaction Prices in a Specialist Market", *Journal of Financial Economics* 14(1), 71–100 (1985)。
- Thomas Ho and Hans R. Stoll, "Optimal Dealer Pricing under Transactions and Return Uncertainty", *Journal of Financial Economics* 9(1), 47–73 (1981)。
- Richard Roll, "A Simple Implicit Measure of the Effective Bid-Ask Spread", *Journal of Finance* 39(4), 1127–1139 (1984)。
- Albert S. Kyle and Anna A. Obizhaeva, "Market Microstructure Invariance: Empirical Hypotheses", *Econometrica* 84(4), 1345–1404 (2016)。
- Robert Almgren and Neil Chriss, "Optimal Execution of Portfolio Transactions", *Journal of Risk* 3(2), 5–39 (2001)。[预印本](https://www.smallake.kr/wp-content/uploads/2016/03/optliq.pdf)
- Dimitris Bertsimas and Andrew W. Lo, "Optimal Control of Execution Costs", *Journal of Financial Markets* 1(1), 1–50 (1998)。
- Anna Obizhaeva and Jiang Wang, "Optimal Trading Strategy and Supply/Demand Dynamics", *Journal of Financial Markets* 16(1), 1–32 (2013)。[NBER wp](https://www.nber.org/system/files/working_papers/w11444/w11444.pdf)
- Olivier Guéant, "Optimal Execution and Block Trade Pricing: a General Framework", *Applied Mathematical Finance* 22(4), 336–365 (2015)。[arXiv:1210.6372](https://arxiv.org/abs/1210.6372)
- Ryan Donnelly, "Optimal Execution: A Review", *Applied Mathematical Finance* 29(3), 181–212 (2022)。
- Marco Avellaneda and Sasha Stoikov, "High-frequency trading in a limit order book", *Quantitative Finance* 8(3), 217–224 (2008)。
- Olivier Guéant, Charles-Albert Lehalle and Joaquin Fernandez-Tapia, "Dealing with the inventory risk", *Mathematics and Financial Economics* 7(4), 477–507 (2013)。[arXiv:1105.3115](https://arxiv.org/abs/1105.3115)
- Rama Cont and Arseniy Kukanov, "Optimal order placement in limit order markets", *Quantitative Finance* 17(1), 21–39 (2017)。[arXiv:1210.1625](https://arxiv.org/abs/1210.1625)
- Thierry Foucault and Albert J. Menkveld, "Competition for Order Flow and Smart Order Routing Systems", *Journal of Finance* 63(1), 119–158 (2008)。
- Eric Budish, Peter Cramton and John Shim, "The High-Frequency Trading Arms Race", *Quarterly Journal of Economics* 130(4), 1547–1621 (2015)。
- Matteo Aquilina, Eric Budish and Peter O'Neill, "Quantifying the High-Frequency Trading 'Arms Race'", *Quarterly Journal of Economics* 137(1), 493–564 (2022)。
- Yuriy Nevmyvaka, Yi Feng and Michael Kearns, "Reinforcement Learning for Optimized Trade Execution", *ICML 2006*, 673–680。
- Ben Hambly, Renyuan Xu and Huining Yang, "Recent Advances in Reinforcement Learning in Finance", *Mathematical Finance* 33(3), 437–503 (2023)。[arXiv:2112.04553](https://arxiv.org/abs/2112.04553)
- Patrick Cheridito and Moritz Weiss, "Reinforcement Learning for Trade Execution with Market and Limit Orders", [arXiv:2507.06345](https://arxiv.org/abs/2507.06345) (2025)。
- Hieu Tran and Hong Yu, "Does a Trading Policy Need to Know What It Trades?", [arXiv:2611.08701](https://arxiv.org/abs/2611.08701) (2026)。
- Guillermo Angeris et al., "Constant Function Market Makers: Multi-Asset Trades via Convex Optimization", *Proceedings of the IEEE* (2022)。[arXiv:2107.12484](https://arxiv.org/abs/2107.12484)
- Weiye Xi and Ciamac C. Moallemi, "Quantifying Sub-Optimality in Routing for Automated Market Makers", [arXiv:2607.20762](https://arxiv.org/abs/2607.20762) (2026)。
- Songrun He, Asaf Manela, Omri Ross and Victor von Wachter, "Fundamentals of Perpetual Futures", [arXiv:2212.06888](https://arxiv.org/abs/2212.06888) (2026)。
- 上述及其余各篇的抽取笔记见 `materials/notes/cluster-*.md`。

---

## 本章要点

- 整个领域可分为八个流派——微观结构基础、交易成本分析、随机控制、做市报价、订单簿微观执行、市场结构与路由、学习自适应、加密与 AMM 执行——它们回答的是同一件"交易"行为的不同侧面。
- 这些流派构成一份"放松假设与相互对抗"的谱系：AC 通过加入风险推广了 Bertsimas–Lo，Obizhaeva–Wang 通过内生化交易时点推广了 AC，而 Guéant 让 Kyle 的线性永久冲击与经验平方根律得以调和。
- 贯穿各流派最关键的经验事实是：冲击对规模是凹的、对时间是瞬态的，任何线性永久冲击模型都无法重现它。
- 三条阅读路线服务三个目标：建立直觉、搭建可用的系统（理论 → 校准 → 工程）、做研究（先综述、再选一条具体切口）。
- 仍在演进的开放问题包括：非线性/瞬态冲击下的稳健最优解、信号与执行的联合优化、RL 的分布外泛化、加密费率与资金费的联合优化、延迟与费用下的路由分解、不可观测的信息半衰期，以及显示与隐藏的统一优化。
- 本章每条陈述都基于实际读到的文档；九本商业专著仅通过作者或出版社自己发布的公开替代材料使用，而 Johnson 的《Algorithmic Trading & DMA》只拿到了目录。
- "针对完整费率表优化可省超过 60%"这一说法在本章所查文献中无法核实，本章不把它当作事实重复。
