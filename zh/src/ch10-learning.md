# 强化学习与自适应执行

第 5 章用闭式解回答了清算问题，第 6 章把它推向反馈规则——在钱规则、效用型规则、盯住成交量的规则。但这些结果都要求事先知道三件事：冲击函数 $g$ 与 $h$、波动率 $\sigma$，以及一个对库存或成本二次的目标。Hambly、Xu 与 Yang 把剩下的质疑说得很直白：Almgren–Chriss 的解是"一个不依赖实时市场状况的预先排定策略"，而"对动态和冲击的错误设定可能导致糟糕的策略乃至亏损"。当冲击是凹的、当流动性系数是潜在的且随时间变化、当目标不是均值–方差、或者当决策既要决定下单量又要决定挂在哪一档时，闭式解根本不存在。强化学习（reinforcement learning, RL）就是那条替代路径：把执行当成一个模型未知的控制问题，用数据学出策略，而瞄准的目标没有变——最小化实施差额（implementation shortfall, IS）或成本加风险的泛函。

## 为什么要学习，以及"同一个目标"指什么

经典路线从模型解析地算出价值函数，学习路线从转移样本估计它、始终不看模型。两者在数学上并非对手：它们面对同一个目标，RL 只用在第一条路没有答案的地方。动因有三。**模型被设错**：冲击在实证上是凹的，临时冲击系数一天之内还在移动且不可观测；第 6 章说明解析修正需要额外结构，而 RL 可以直接从价格里把它推断出来。**状态维度高且非马尔可夫**：真实决策要用订单簿、近期订单流、自己的成交量以及剩余日程，表格法或偏微分方程承载不了，神经网络可以。**动作空间是联合的**：Almgren–Chriss 只选日程，而券商要同时选日程、订单类型和挂单档位，迄今为止只有学习式方法把这二者放进同一个策略里。

有一个结构性假设让执行对 RL 变得可解：状态拆成**私有变量**（已用时间、剩余库存）与**市场变量**，且训练时假设智能体自身的动作不改变市场变量。

## 形式化：把执行写成 MDP

一旦选定状态、动作、转移和奖励，执行问题就成了 MDP $(\mathcal{S},\mathcal{A},P,r,\gamma)$。时域有限，每个回合的收尾方式都一样：$T$ 时刻仍持有的库存被强制清算。下表记录本章所引文献对这些对象的确切定义。

| 文献 | 状态 | 动作 | 奖励 | 算法 |
|---|---|---|---|---|
| Nevmyvaka 等 2006 | $t$、$i$，加 $R$ 个市场变量（价差、不平衡、即时市价单成本、过去 15 秒带符号成交量） | 把所有剩余库存重新挂限价单的价格（相对买/卖价） | 部分成交的现金流入；期末市价单 | 反向归纳式 Q 学习 |
| Hendricks–Wilcox 2014 | $t,i$、分位数价差状态、分位数成交量状态、起始小时 | AC 子单的缩放系数 $\beta\in[0,2]$，步长 0.25 | 子单穿行订单簿后的实际 IS | 表格 Q 学习 |
| Ning 等 2018 | 时间、库存，可选中价与二次变差 | 本期卖出的股数 $x_t\in[0,q_t]$ | 盯市现金减去 $a\,(x_{T_k}/M_k)^2$ | double DQN |
| Lin–Beling 2020 | 原始 level-2（最优五档价量）、库存、已用时间 | 成交量，范围 $[0,2\cdot\mathrm{TWAP}]$ | 相对 TWAP 的稀疏终点信号 | PPO |
| Karpe 等 2020 | 归一化时间、库存、价差、最优档不平衡、单期与自期初收益 | （成交量）×（市价单或挂在最优 1/2/3 档） | 滑点 × 成交比例 | double DQN |
| Macrì–Lillo 2024 | 库存、时间，可选中价 | 每步卖出股数 | IS | double DQN |
| Fang 等 2021 | 分钟价量与订单簿；教师另可见未来 | 每分钟成交股数 | 归一化即时奖励 | RL 教师蒸馏出学生 |
| FlowOE 2025 | Heston 环境状态 | 成交动作的分布 | 模仿专家集合 | 流匹配策略 |

奖励设计是各文献分歧最大的地方。Nevmyvaka 等与 Hendricks–Wilcox 直接用 IS。Ning 等把每秒股数的平方当作"吃穿订单簿"的代理来惩罚。Lin–Beling 则拒绝用 IS，理由是它"噪声大、非平稳……只是一个代理奖励，并不是直接优化目标"，转而只奖励相对 TWAP 的表现：策略的 IS 至少优于 TWAP 一成时给稀疏终点奖励 $+1$，更差给 $-1$，介于两者以及所有中间步给 $0$。

## 最早的大规模解法：Nevmyvaka–Feng–Kearns（2006）

这是开山之作。在固定时域 $H$ 内卖出 $V$ 股，环境由真实 INET（NASDAQ）订单流驱动，智能体自己的限价单被插入重建后的订单簿。状态为 $\langle t,i,o_1,\dots,o_R\rangle$：分辨率为 $T$ 的已用时间与分辨率为 $I$ 的剩余库存，加上概括价差、买卖量不平衡、即时市价单成本和过去 15 秒带符号成交量的市场变量。动作是一个限价，用来把**全部**剩余库存重新挂出，相对卖价计；$a=0$ 贴着最优价，$a>0$ 穿价差。奖励是部分成交带来的现金，期末残余被强制清算，成本以回合开始时中价为基准用基点衡量。

算法是反向归纳的 Q 学习，靠私有–公开独立性变得廉价：训练时假定智能体的动作不推动市场变量，于是优化器可以对每个观测到的订单簿遍历所有 $\{t,i\}$ 单元，运行时间与 $R$ 无关。测试阶段的仿真则**包含**策略自身的冲击，这也正是该假设被验证的方式。

在一年半的毫秒级 INET 数据上（12 个月训练、6 个月测试；AMZN、QCOM、NVDA；$V\in\{5{,}000,10{,}000\}$ 股；$H\in\{2,8\}$ 分钟），只用私有变量、把 $T$ 或 $I$ 提到 8 时，相对优化后的"挂单不管"（submit-and-leave, S&L）改善 **27.16% 到 35.50%**。加入市场变量后进一步降本：只用价差 **+7.97%**，价差加即时市价单成本 **+8.69%**，价差加即时成本加带符号成交量 **+12.85%**；而成交量不平衡单独只贡献 **+0.13%**，因为它能预测成本却不改变使成本最小的动作。作者总体上报告"相对 S&L 改善 50% 或更多，相对市价单改善数倍"。策略在时间将尽而库存仍多时变得更激进。作者自陈两条局限：其他交易者不做策略性反应，且忽略手续费与延迟。结构上动作把下单量固定为全部剩余库存，所以这是"只做挂单、不做日程"。

## 学一个对 Almgren–Chriss 的修正：Hendricks–Wilcox（2014）

Hendricks 与 Wilcox 把 AC 闭式解当作**先验**，只学一个偏差。智能体不选日程，只选每期要执行 AC 子单的比例 $a_{jt}=\beta_j\,\mathrm{AC}_t$，$\beta_j\in[0,2]$、步长 0.25，残余量期末执行。状态是 $\langle t_n,i_n,s_n,v_n\rangle$——已用时间、剩余库存，以及按分位数分桶的价差状态与买卖量状态——并由交易小时决定分桶所依据的分布。奖励是子单穿行订单簿后的实际 IS，方法是表格 Q 学习，并用吸收态把有限时域 MDP 变成收敛的（$\gamma=1$）。

在 2012 年南非 ALSI 全部 166 只股票、五档深度、六个月训练上，RL 模型把 AC 的中位数 IS 最多压低 **10.3%**（短时域 $T=4$，即 20 分钟），平均压低 **4.8%**。逐格结果从 **SBK 在 16:00–17:00 的 +15 个基点**到 **SAB 的 −7 个基点**；更细的状态网格提高了"动作正确率"，却没有换来更好的 IS。最要紧的警示来自作者自己：由于奖励是风险中性的 IS，RL 轨迹的 IS 标准差反而**高于** AC（0.17–0.26 对 0.13–0.14），收益是用额外的执行风险买来的——缺陷的修法是把方差放进奖励。

## 深度强化学习

**双层 DQN。** Ning、Lin 与 Jaimungal 用网络把表格法做大，只做市价单。核心状态是已用时间与剩余库存，可选加入变换后的中价和上一期的二次变差；因为可行动作集随库存变化，网络以 $(s,x)$ 为输入、输出 $Q(s,x)$。奖励是盯市现金减去对每秒股数的二次惩罚 $a\,(x_{T_k}/M_k)^2$。实现是带经验回放的 Double DQN、$\varepsilon$-greedy 探索、六层二十节点的 ReLU 网络与 RMSprop，参数 $a=0.01$、回放 $N=10{,}000$、目标网络每 $\rho=15$ 次更新、$\gamma=0.99$，并用"先持有后倾泻"与"立刻倾泻"两个边界情形预训练。在 2017-01-02 至 2018-03-30、只取 11–13 点、$Q_0=20$ 手（2,000 股）、一小时分五期的设定下，相对 TWAP 的中位数损益改善为：INTC **+11.63 基点**（价、时、量），加二次变差后 **+11.96**；MSFT **+5.97/+5.89**；NTAP **+10.13**；VOD **+14.68/+15.72**；AAPL **+2.92**；FB **+2.61**。$P(\Delta>0)$ 在 INTC、MSFT、NTAP 上是 95.8–97.8%，AAPL 只有 77.4%；**GOOG 与 AMZN 没有显著优势**（AMZN 均值 −0.28）。学到的策略在价格高、时间将尽、二次变差大时卖得更快。

**端到端策略梯度。** Lin 与 Beling 把人工状态和塑形奖励换成原始 level-2（最优五档价量）加剩余库存与已用时间，奖励改为相对基准的稀疏信号。动作是 $[0,2\cdot\mathrm{TWAP}]$ 区间的股数，其上限保证 TWAP 平均至少吃到第二档。算法是 PPO-clip，两种结构：两层 128 单元网络接一个 cell 为 128 的 LSTM 输出策略与价值，或把近期订单簿档位堆叠作输入；零期末库存靠合并最后两步实现。在 WRDS 毫秒级 TAQ、2018 年、14 只美股上训练（1 分钟时域、5 秒决策；1–9 月训练、10–12 月测试），智能体在**不到 200,000 步**内收敛，并在多数标的上胜过 TWAP、VWAP、AC 与两种 DQN。波音上相对 TWAP 的平均 $\Delta\mathrm{IS}$：PPO-LSTM 72.53，DQN（Ning）91.35，稀疏奖励 DQN −6.82，AC −0.78。作者还指出 Ning 的方法在某些股票上危险地振荡。

**多智能体仿真。** Karpe、Fang、Ma 与 Wang 在 ABIDES 里训练 double-DQN，这是一个带真实延迟、回放 NASDAQ 历史订单流的智能体式离散事件订单簿仿真器。作者称这是"第一个把最优执行与最优挂单合并进动作空间的建模"：动作先把 TWAP 子单量乘一个标量 $a\in\{0.1,0.5,1.0,\dots,2.5\}$，再选挂法——市价单、100% 挂最优档、最优两档各 50%、或最优三档各三分之一。奖励是滑点乘成交比例。在 2003-01-13 至 02-06 的五只 NASDAQ 股票（CSCO、IBM、INTC、MSFT、YHOO）、10:00–15:30、30 秒一期、训练 9 天测试其后 9 天上，智能体**在约 9 天训练后退化为 TWAP**，挂最优档限价单或市价单。作者把这种塌缩归因于 30 秒的节奏看不到更短周期信号。

## 学习模型不知道的流动性：Macrì–Lillo（2024）

Macrì 与 Lillo 在 AC 按构造就错的地方检验 RL：临时与永久冲击系数都随时间变化且**潜在**。当冲击确定性上升、状态只有库存与时间时，学到的 IS 为 0.2401，而理论最优 0.1449、TWAP 0.2326——不比 TWAP 好，相对理论差 −4.76 基点。加入中价后，学到的 IS 降到 0.1944，离理论只差 −2.42 基点。决定性实验是**混合训练**：用上升与下降两类冲击轨迹各 10,000 条训练，只测其中一类，这才是生产场景。此时 TWAP 是天然的不可知基准：库存加时间对 TWAP 的 $\Delta$ 损益为 −0.92 基点，而库存加时间加价格得到学到的 IS 0.1319、**相对 TWAP +5.2 基点**——既胜过不可知基准，也胜过理论最优的平均成本，仅凭价格就推断出了冲击的方向与大小。该研究只卖出，省略价差。

## 模仿学习与离线学习

在真实市场里探索要花钱且不可重复，而靠试错训练的策略只遇到自己犯错造出的状态、因而偏离市场，于是重心转向从示范或固定数据集学习。

Fang 等训练一个能看到完美未来信息的**教师**和一个只看当前可观测状态的**学生**，再用策略蒸馏把教师的动作分布交给学生。学生在部署时不泄露任何未来信息，却学到了"先知"的行为样式，且**同一个**策略在多标的上训练。在中国 A 股（2017-01 至 2019-06，在 CSI 800 上评估）上，蒸馏策略的价格优势（Price Advantage）为 **6.17 基点**，TWAP 为 0（AC 2.33、VWAP 0.32、DDQN 4.13、PPO 2.52），盈亏比为 1.35；案例研究显示 DDQN 每天几乎以同样方式执行，而蒸馏策略跟随价格。

FlowOE 更进一步：它学一个条件于可观测状态的生成式流匹配模型来建模动作分布，模仿一个**专家集合**——TWAP、VWAP、按非线性冲击调整过的 AC 近似、Heston 最优专家与 PPO 专家。环境是 Heston 随机波动率加平方根凹冲击。当 $\beta=0.5$、样本外 10,000 次试验时，在高波动高冲击场景下，用 PPO 专家训练出的"Shortcut"策略 IS 为 **189,332**、总成本 **269,147**；对照 TWAP 211,139 / 476,592、VWAP 210,908 / 455,611、AC 近似 210,880 / 427,384、Heston 最优专家 210,592 / 406,607——在 IS、风险与成本三项上都超过了它所模仿的专家和全部经典基线。模仿绕开了奖励设计（专家的动作本身就编码了目标），但这里的一切都是仿真。

## 统一的视角：执行与对冲

Kolm 与 Ritter 关于动态复制与对冲的工作用 RL 处理同一问题的期权侧对应物，此处属于形式上的相关性。原文（2019）在付费墙后未能取得，其方法只按作者公开演示稿与一篇综述所能支持的程度描述。它的形状就是执行 MDP 的形状：状态是组合与市场，动作是用于对冲的一笔交易，奖励是期末复制误差减去交易成本，目标是让学到的策略把"Black–Scholes delta 加固定不交易带"所支付的摩擦内化。执行与对冲是同一个控制问题在不同目标下的两面，因此为一个开发的奖励归一化或策略类可以迁移到另一个。关于 Kolm–Ritter 自身结果的任何定量说法在此都属未经验证。

## 归纳偏置与跨资产泛化

在一只股票上训练的策略最好也能交易另一只。理论杠杆是**微观结构不变性**（Kyle–Obizhaeva, 2016）：在**商业时间**——风险转移（"下注"）的到达率——下度量，单笔下注金额的分布与预期成本在各资产间相同。记 $W$ 为每日转移的风险金额，则下注到达率按下注活动的三分之二次幂、平均下注按三分之一幂缩放，$\gamma_{jt}\propto W_{jt}^{2/3}$、$\bar Q\propto W_{jt}^{1/3}$，而百分比价差正比于（收益方差／金额成交量）的立方根。这些就是可以把规模、时间和价格重新表达、从而让一个策略适配许多资产的单位；它们由 2001–2005 年四十万笔以上的组合转仓订单估计得到。

Tran 与 Yu 检验这套东西究竟能换来什么。决策被压到最小：在最优价挂限价单等待，还是穿价差。策略在 2019 年的 954 只股票上训练一次，随后**原样不变**地在 2020–21 年的 1,964 只未见股票、2025–26 年的 2,629 只、加密对以及第二个交易所上评估。一个只通过近期成交率、成交规模、价差与最小变动价位认识某只股票的策略，成本在**每股 0.015 个价差**以内，即可与训练数据包含该股票的策略相比——约为"学习相对于挂单等待所节省"的五分之一——而该股票自身的历史毫无增益。但不变量单位相对原始单位、z-score 与资产描述子只胜出**不到 1 个基点**，其中大部分优势只是一个成本尺度和一份学习者本来就能提供的输入：把这些窗口统计量作为输入、成本以价差计，原始特征即可找回 **81%** 的差距。**没有任何一种表示能外推到它没见过的价差区间。** 作者还提醒：与"混合池"比较器之间近乎为零的迁移差距是弱证据，因为该比较器记录一种表示的损失时，只按测试资产在它数据中的占比缩放。

## 诚实的评估

若干失效模式反复出现，且为文献所承认。仿真与真实市场的差距最大：多数研究回放历史数据或运行智能体式仿真，只有少数（Tran–Yu 的逐笔 Nasdaq 精确回放、Karpe 等在 ABIDES 中的反应式智能体）让智能体自己的订单推动订单簿。做不到时，"市场是有弹性的"就只是一个**假设**——用 Lin 与 Beling 的话说，这是"本文以及此前所有把 RL 用于最优执行的研究的核心假设"，因为用历史数据训练"无法计入永久市场冲击"。第二是奖励设计：同一个问题分别用 IS、二次速率惩罚和稀疏的 TWAP 相对信号来解，各文献公开争论谁对；Hendricks–Wilcox 更表明风险中性的 IS 奖励反而**抬高**了 IS 的方差，而方差恰是经典解法要控制的准则。第三是样本效率：本章文献覆盖 45,000 个回合、200,000 步、800 万步和 300,000 次转移，远多于校准一个 AC 模型所需的样本量。第四是分布外泛化：不变量单位只在训练覆盖的区间内迁移，换个价差区间就不行；只在一种流动性方向上训练的策略，必须从价格推断出另一种，否则退回 TWAP。

**本节判断**（以下为本章的归纳，而非文献原话）：报告中的改善幅度必须透过三层滤镜来读。一是**高度依赖基线**：在 Macrì–Lillo 的"库存加时间"情形里，那个相对 S&L 改善 50% 的方法对 TWAP 毫无优势；在 Ning 等那里也只对九只中的七只有效。二是**依赖成本假设**：本章几乎所有研究都假设无手续费、无延迟、订单簿有弹性，极少纳入费率或返佣，而费用结构足以吞掉一位数基点的优势。三是**对照物常常是理论最优解**，而后者是在 RL 本要逃离的同一套线性冲击假设下算出来的，于是"胜过 AC"可能只意味着智能体复现了解析解已经给出的线性最优。Karpe 等的"退化为 TWAP"是最干净的提醒：当状态在决策节奏上不含信号时，再丰富的动作空间与再正确的奖励，也只会产出经典答案。

## 这给第 5、6 章留下了什么

RL 并不取代第 5、6 章的随机控制，也没有哪位作者这样宣称。它在闭式解不可得时，用数据逼近**同一个**目标；它从两个方向继承了经典解：作为标尺（AC 有效前沿，数据驱动的策略被放上去比较）和作为先验（Hendricks–Wilcox 只学 AC 轨迹上的一个有界修正，Macrì–Lillo 在状态里加上正确的变量后就恢复了最优）。经典模型仍负责提供单位——冲击、风险厌恶、前沿——而学到的策略负责提供模型从未见过的市场里的那份反应。按这个顺序读，第 5 章、第 6 章与本章是一个故事：能解就解，知道它错在哪就修正，写不下来就学。

## 本章依据的文献

- Yuriy Nevmyvaka, Yi Feng, Michael Kearns, "Reinforcement Learning for Optimized Trade Execution", *ICML 2006*, 673–680. 本地 PDF：`materials/papers/cluster-g/2006-NevmyvakaFengKearns-rl-optimized-execution.pdf`。
- Dieter Hendricks, Diane Wilcox, "A Reinforcement Learning Extension to the Almgren–Chriss Framework for Optimal Trade Execution", *IEEE CIFEr 2014*, 457–464; arXiv:1403.2229（表 I、II）。本地 PDF：`materials/papers/cluster-g/2014-HendricksWilcox-rl-almgren-chriss.pdf`。
- Brian Ning, Franco Ho Ting Lin, Sebastian Jaimungal, "Double Deep Q-Learning for Optimal Execution", arXiv:1812.06600 (2020); *Applied Mathematical Finance* (2021)（第 7 节、表 1）。本地 PDF：`materials/papers/cluster-g/2018-NingLinJaimungal-double-deep-q-execution.pdf`。
- Siyu Lin, Peter A. Beling, "An End-to-End Optimal Trade Execution Framework based on Proximal Policy Optimization", *IJCAI-20*, 4548–4554。本地 PDF：`materials/papers/cluster-g/2020-LinBeling-end-to-end-ppo-execution.pdf`。
- Michaël Karpe, Jin Fang, Zhongyao Ma, Chen Wang, "Multi-Agent Reinforcement Learning in a Realistic Limit Order Book Market Simulation", *ICAIF '20*, arXiv:2006.05574（第 3–4 节）。本地 PDF：`materials/papers/cluster-g/2020-Karpe-multiagent-rl-lob-execution.pdf`。
- Andrea Macrì, Fabrizio Lillo, "Reinforcement Learning for Optimal Execution when Liquidity is Time-Varying", arXiv:2402.12049 (2024)（表 4、7）。本地 PDF：`materials/papers/cluster-g/2024-MacriLillo-rl-time-varying-liquidity-execution.pdf`。
- Yuchen Fang 等, "Universal Trading for Order Execution with Oracle Policy Distillation", *AAAI-21*, arXiv:2103.10860。本地 PDF：`materials/papers/cluster-g/2021-Fang-oracle-policy-distillation-execution.pdf`。
- Yang Li, Zhi Chen, "FlowOE: Imitation Learning with Flow Matching for Optimal Execution under Heston Volatility and Concave Market Impacts", arXiv:2506.05755 (2025)（表 2）。本地 PDF：`materials/papers/cluster-g/2025-FlowOE-imitation-flow-matching-execution.pdf`。
- Ben Hambly, Renyuan Xu, Huining Yang, "Recent Advances in Reinforcement Learning in Finance", *Mathematical Finance* 33(3), 437–503 (2023); arXiv:2112.04553（第 4 节）。本地 PDF：`materials/papers/cluster-g/2023-HamblyXuYang-recent-advances-rl-finance.pdf`。
- Albert S. Kyle, Anna A. Obizhaeva, "Market Microstructure Invariance: Empirical Hypotheses", *Econometrica* 84(4), 1345–1404 (2016)（第 2.1 节）。本地 PDF：`materials/papers/cluster-g/2016-KyleObizhaeva-market-microstructure-invariance.pdf`。
- Hieu Tran, Hong Yu, "Does a Trading Policy Need to Know What It Trades? Microstructure Invariance and Zero-Shot Transfer of Execution Policies", arXiv:2611.08701 (2026)（第 1、6 节）。本地 PDF：`materials/papers/cluster-g/2026-microstructure-invariance-zero-shot-transfer-execution.pdf`。
- Petter N. Kolm, Gordon Ritter, "Dynamic Replication and Hedging: A Reinforcement Learning Approach", *Journal of Financial Data Science* 1(1), 159–171 (2019)。**未能取得**（付费墙），仅据作者公开演示稿与综述描述。

## 本章要点

- Almgren–Chriss 的最优解是一个忽略实时状况、且依赖冲击设定正确的预先排定日程；当冲击非线性、流动性潜在、目标非二次、或动作必须包含挂单时，RL 是数据驱动的替代方案。
- 执行被写成有限时域 MDP：状态拆为私有变量（时间、库存）与市场变量，动作是成交量与/或价格，奖励是实际现金、实施差额、速率惩罚或相对基准的信号。
- Nevmyvaka–Feng–Kearns 的反向归纳 Q 学习只用私有变量即相对优化后的"挂单不管"改善 27.16–35.50%，加入价差、即时成本与带符号成交量后再改善 12.85%，而成交量不平衡单独只贡献 0.13%。
- Hendricks–Wilcox 只学 AC 轨迹上的有界修正，把中位数实施差额在短时域压低最多 10.3%、平均 4.8%，但代价是更高的实施差额标准差——风险中性奖励的代价。
- 深度 RL 扩展了状态与动作：双层 DQN 在九只美股中的七只上胜过 TWAP（INTC 最多 +11.96 基点）但败于 GOOG 与 AMZN；基于原始 level-2 的 PPO 在不到 20 万步内收敛并在 14 只中的多数上胜过 TWAP、VWAP 与 AC；而把下单量与挂单合并的动作空间，在决策节奏看不到信号时仍退化为 TWAP。
- 模仿与离线学习之所以有吸引力，是因为在真实市场探索昂贵且不可逆；先知策略蒸馏与流匹配模仿都胜过经典基线，并能训练出跨标的的单一策略。
- 微观结构不变性带来真实但有限的零样本迁移——跨未见股票与交易所约为每股 0.015 个价差——且没有任何表示能外推到未见的价差区间。
- RL 并不取代第 5、6 章的随机控制：它用数据逼近同一目标，以 AC 前沿为标尺、在混合式设计里以 AC 日程为先验。
