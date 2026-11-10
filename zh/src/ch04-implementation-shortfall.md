# 实施差额与交易成本分析

交易是投资想法的实现过程，实现的质量和想法本身同等重要。要把这句话变成可度量的东西，只需要一个观察：管理者决定持有的"纸上组合"，几乎永远无法按决策当时的价位真正建起来。佣金、买卖价差和市场冲击实实在在要付，而且有意持有的仓位总有一部分根本没能成交。纸上组合与真实组合之间的差额，就是**实施差额**（implementation shortfall，IS）。

## 纸上组合与决策价格

Leinweber 把这件事说得很直白：纸上组合是"一个想象中的持仓，包含投资者决定持有的所有证券头寸，按他们决定持有那一刻的中间价取得"，它不承担佣金、税、价差、冲击和机会成本。于是度量交易成本就只是"真实组合与纸上组合之差"。Mittal 引述 Perold 的原始定义：实施差额是"理论组合与实际执行组合之间的收益差"，并强调管理者在构建组合时看到的价位——**决策价格**——并不是执行真正能拿到的价位。

因此有两个时刻要紧。设 $t_d$ 为决策时刻、$P_d$ 为**决策价格**（$t_d$ 时的中间价）；$t_1$ 为第一笔交易被释放的时刻。Labadie 与 Lehalle 把这个选择讲明了：如果把 $t_d$ 到 $t_1$ 之间的价格滑移也算作成本，IS 的基准就是决策时刻价 $S_0$；如果忽略它，基准就是第一笔交易时的价 $S_1$。当基准取订单交给交易台那一刻的中间价时，从业者的说法是**到达价格**（arrival price）。

固定记号。目标为 $Q$ 股；交易台以价格 $P_i$ 分 $v_i$ 笔成交，共成交 $q=\sum_i v_i$ 股；剩余 $Q-q$ 股在期末价格 $P_T$ 下未能成交；$F$ 表示显性费用。对买入而言，纸上组合本应花掉 $Q P_d$，真实组合则花掉 $\sum_i v_i P_i + P_T(Q-q)$ 再加 $F$，两者之差即

$$
\mathrm{IS} = \sum_i v_i P_i + P_T(Q-q) - Q P_d + F,
$$

约定正值表示不利。卖出时各项符号全部反转。Almgren 与 Chriss 把卖出侧的成交写成 $\sum_k n_k\tilde S_k$，并指出总量 $XS_0-\sum_k n_k\tilde S_k$"正是 Perold（1988）所称的实施差额"。

## 四项分解

上面那个总差额可以拆成四项，每一项都有独立的行为含义。

**延迟成本**。从决策到订单释放之间，仓位仍然暴露在市场里：

$$
\text{delay} = q\,(P_{t_1}-P_d).
$$

只有当交易台在决策价位立即成交时它才为零。

**执行（冲击）成本**。从开始执行时的价格到实际成交价之间的滑移：

$$
\text{execution} = \sum_i v_i P_i - q\,P_{t_1} = \sum_i v_i\,(P_i-P_{t_1}).
$$

跨越价差、临时冲击，以及任何反向累积的永久冲击，都落在这里。

**机会成本**。未成交的余量按期末价格计价：

$$
\text{opportunity} = (Q-q)(P_T-P_d).
$$

只有订单完全成交它才为零。本质上它是一笔放弃的收益：如果价格朝管理者有利的方向走了，未成交的那部分没能吃到。Mittal 的 ITG 白皮书把算法机会成本定义为相对到达价格交易成本的**标准差**——因为让实际成本范围变宽的正是未成交部分，而不是均值。

**显性费用**。佣金、税费、交易所与清算费用，这一项不需要价格模型。

四项相加回到总差额。这个"扩展版 IS"分解——延迟、执行/冲击、机会、费用——是交易台实际使用的形式，其中延迟项被 Kissell（2006）归功于 Wagner（1991）。

哪一项占主导取决于速度。慢的调度在价格偏离决策价位时累积延迟成本和机会成本，同时完不成订单的风险上升；快的调度累积冲击成本。这正是 Almgren–Chriss 有效前沿所刻画的取舍，其中风险厌恶参数在期望成本与成本方差之间取舍（见第 5 章）。

## 关于材料来源的说明

Perold 1988 年的文章《The Implementation Shortfall: Paper vs. Reality》在付费墙内，本书**未能获取原文**。因此上面的定义与分解式，是**依据实际拿到、使用并转述该分解的二手文献重建的**：Mittal/ITG 对定义的引述、Leinweber 以"纸上减真实"作为可操作的成本度量、Labadie 与 Lehalle 的相对财富写法、Almgren 与 Chriss 的卖出侧成交式，以及 Kissell/Wagner 四分量拆分在二手材料中的通行描述。凡二手文献本身没有给出的样本、数字或公式，本书一律不归于 Perold 名下。

## 基准定在哪里

以到达价格计的实施差额只是若干选择之一。每个算法把参照价定在不同的位置，而这一选择——不只是调度——决定了它在优化什么。

- **TWAP** 以窗口内时间加权平均价为参照，目标是均匀的成交速度。它只在 Bertsimas 与 Lo（1998）那条特定的"线性永久冲击 + 随机游走"下期望最优，此时动态规划给出的正是等分策略 $S_t^\ast=S/T$。
- **VWAP** 以成交量加权平均价为参照。Berkowitz、Logue 与 Noser 最早提出用它衡量执行质量；它不需要成本预测，Mittal 称它是"一个移动靶，比到达价格宽容得多"。
- **POV**（按成交量比例）根本不设定价格参照：它把成交约束为市场成交量的固定比例，期限由约束隐含决定。
- **到达价/IS** 以决策时刻价为参照，目标是在给定风险下最小化期望差额。

最关键的区别是**随机基准与固定基准**。到达价格在决策瞬间就已确定；VWAP 和目标收盘按定义是随机的，本身是价格路径的函数，拿执行结果去和它们比，会把基准自身的方差叠加到被测成本上。这带来两个后果：其一，和 VWAP 比是可以被"做"出来的，因为前倾成交量分布会改善实际均价，同时抬高真实冲击；其二，以随机基准最小化离差，和以到达价格最小化实施差额，并不是同一个策略。

### VWAP 与保证 VWAP

Guéant 与 Royer 为 VWAP 执行给出了严格形式。设市场成交量曲线 $V_t$、价格 $S_t$、作用于参与率 $v_t/V_t$ 的执行成本函数 $L$、以及作用于价格的永久冲击 $F$。签订**保证 VWAP** 合约的经纪商，必须按当期 VWAP 交付 $q_0\,\mathrm{VWAP}_T$，对应期末现金 $X_T$。其溢价即无差异定价

$$
\sup_{v\in\mathcal{A}_{\det}} \mathbb{E}\!\left[-\exp\!\left(-\gamma\left(X_T - q_0\,\mathrm{VWAP}_T\right)\right)\right], \tag{3.1}
$$

Lemma 3.1 表明 $X_T - q_0\,\mathrm{VWAP}_T$ 服从正态，均值为

$$
-\int_0^{q_0} F(z)\,dz - \int_0^T V_t L\!\left(\frac{v_t}{V_t}\right)dt + q_0\int_0^T \frac{V_t}{Q_T}\,F(q_0-q_t)\,dt
$$

方差为 $\sigma^2 q_0^2\int_0^T\left(\frac{q_t}{q_0}-\left(1-\frac{Q_t}{Q_T}\right)\right)^2 dt$。于是指数目标坍缩为一个确定性的变分问题

$$
\inf_{v}\ \int_0^T V_t L\!\left(\frac{v_t}{V_t}\right)dt - q_0\!\int_0^T \frac{V_t}{Q_T}F(q_0-q_t)\,dt + \frac{\gamma}{2}\sigma^2 q_0^2\!\int_0^T\!\left(\frac{q_t}{q_0}-\left(1-\frac{Q_t}{Q_T}\right)\right)^2 dt. \tag{3.2}
$$

其核心结论是：**没有永久冲击**（$f=0$）时，最优曲线为

$$
q^\ast(t)=q_0\left(1-\frac{Q_t}{Q_T}\right),
$$

交易曲线与相对成交量曲线同形，溢价为 $\pi(q_0)=Q_T L(q_0/Q_T)$。由此有两层实务含义：跟踪 VWAP 归结为预测**相对**成交量曲线（它跨日稳定）而非总成交量（它不稳定）；只有当永久冲击可忽略时，VWAP 算法才严格呈成交量形状，否则两条曲线会以可量化的幅度分离。

### POV

Guéant 补上了一个实务中重要、学术上却几乎空白的缺口。交易者按 $v_t=\rho V_t$ 执行，期限由 $\int_0^T \rho V_s\,ds=q_0$ 隐含决定，只需选择单一参数 $\rho$。取执行成本 $L(\rho)=\eta\rho^{1+\varphi}$、成交量曲线平坦 $V_t\equiv V$，均值–方差准则为

$$
J(\rho)=\eta\rho^{\varphi} q_0 + \frac{\gamma}{6}\sigma^2\frac{q_0^3}{\rho V},
$$

一阶条件 $\eta\varphi\rho^{\ast\,\varphi-1}=\frac{\gamma}{6}\sigma^2\frac{q_0^2}{\rho^{\ast 2}V}$ 给出闭式最优参与率

$$
\rho^\ast=\left(\frac{\gamma\sigma^2 q_0^2}{6\,\eta\,\varphi\,V}\right)^{\frac{1}{1+\varphi}}.
$$

该比率随风险厌恶 $\gamma$、波动率 $\sigma$、订单规模 $q_0$ 上升，随流动性 $V$ 下降；它不被 1 约束，且与线性执行成本常数和线性永久冲击系数无关——无论怎么切片，这两项都要付。

### VWAP 作为一个随机控制问题

Frei 与 Westray 让 VWAP 订单相对**随机的市场 VWAP**的滑移，在均值与方差两个维度上同时最小，采用线性临时冲击 $\kappa$ 与**gamma 桥**相对日内成交量模型。买入 $Y$ 股的总支出为

$$
TE^u = YP(0) - \sigma\!\int_0^T X^u(t)\,dW(t) + \sigma Y W(T) + \kappa\!\int_0^T u^2(t)\,dt. \tag{1}
$$

他们得到显式值函数 $v(t,x,\gamma)=a(t)x^2+b(t)\gamma x+c(t)x+d(t)\gamma^2+f(t)\gamma+g(t)$ 与最优成交速度

$$
\hat u(s) = -\frac{2a(s)\hat X(s)+b(s)\gamma(s)+c(s)}{2\kappa}. \tag{3}
$$

最值得从业者带走的是 Corollary 3.4：最优速度分解为 $\hat u=\hat u_1+\hat u_2$，其中 $\hat u_1=1/T$ 正是 TWAP 速度，$\hat u_2$ 则对 gamma 桥偏离均值的部分作出反应。也就是说，VWAP 跟踪器就是 TWAP 加一个由成交量驱动的修正项，且最优速度只允许一次越到市场另一侧，之后不再回头。

## 目标收盘与实施差额互为镜像

Labadie 与 Lehalle 把目标收盘（TC）与 IS 放在同一框架里，指出它们"是同一枚硬币的两面"。TC 有固定终点（收盘）与未知起点，以收盘价为基准；IS 有固定起点与未知终点，以开始执行时的价为基准。以 $S_1$ 为基准，IS 的**相对财富**为

$$
\widetilde W = W - S_1\sum_{n=1}^{N} v_n,
$$

其均值 $\mathbb{E}(\widetilde W)=\sum_{n=1}^{N}\kappa\sigma_n\frac{(x_n-x_{n+1})^{\gamma+1}}{V_n^\gamma}$、方差 $\mathbb{V}(\widetilde W)=\sum_{n=2}^{N}x_n^2\sigma_n^2$，其中 $\gamma$ 为冲击指数、$V_n$ 为市场成交量。最小化 $\mathbb{E}(\widetilde W)+\lambda\mathbb{V}(\widetilde W)$ 得到显式的递推交易曲线

$$
v_{n-1} = V_{n-1}\left[\frac{\sigma_n}{\sigma_{n-1}}\left(\frac{v_n}{V_n}\right)^{\gamma} + \frac{2\lambda(n,1)\,\sigma_n^2}{\kappa(\gamma+1)\,\sigma_{n-1}}\sum_{i=n}^{N} v_i\right]^{1/\gamma}. \tag{14}
$$

TC 的递推式是它的镜像：波动率恒定时，两者只差时间方向相反。当波动率随时间变化时，差别对交易台很重要：TC 交易者必须用**前瞻**波动率 $\sigma_{n+1}$ 给已成交股份加权，因为收盘固定、每日切片越靠后越大，所以调度很"硬"，必须提前一步预判波动率；IS 交易者用**当期**波动率 $\sigma_n$ 加权，在末端保留了更多调整余地。因此把收盘选作基准并非表面选择：它改变最优调度的形状与对波动率的敏感度，并把成交量推向尾盘。

## 实证交易数据说了什么

Leinweber 的研究是标准的经验记录。样本是 1991 年一家美国大型公司养老金计划的 13,651 笔股票**买入**交易，总额近 20 亿美元，单笔从 100 股到 40 万股以上，且全部成交，因而机会成本被排除，成本简化为"成交价减决策价再加佣金"。结果与通常认知相反：最大笔交易的成本远低于 Loeb 大宗成本模型的预测；百分比成本最高和最低的都出现在**最小**的交易上；不少交易甚至是负成本，即交易盈利。小笔交易在总成本中占了不成比例的份额。某个管理人的组合当年净交易成本为 35 个基点。最醒目的例子是：1979 至 1991 年，ValueLine 纸上组合年化收益 26.2%，而真实的 ValueLine 基金只有 16.1%，差额就是实现的成本。

我们拿到的两处文献记载了 Perold 同一个 Value Line 例子的不同版本。Bertsimas 与 Lo 转述的是 1965–1986 年间纸上组合每年跑赢市场近 20%、而真实基金只有 2.5%。时段与量级都与 Leinweber 的 1979–1991 数字不同，两者并不一致；本书两个都列出，且不据其中任何一个作精确论断。

Mittal 的 ITG 白皮书给出从业者的综合判断。IS 才是正确的基准，因为 VWAP 可以被影响、会把风险厌恶误导到偏离到达价的目标上、且比决策价宽容。但 VWAP 与 POV 仍是有用的**降低 IS 的工具**。示例算术：10 万股订单，三小时成交量 300k/400k/300k，应成交 30k/40k/30k 以维持 10% 的恒定参与率，"这正是 VWAP 算法的成交方式"；POV 算法把恒定参与率一般化，能接受 10% 参与冲击的交易者直接跑 10% 参与率算法即可。白皮书中的图表显示，VWAP 与 IS 算法相对到达价格的 P&L **均值**相近，但 IS 在**离散度**上明显占优，对占 ADV 比例很小的订单尤为清楚——这正是机会成本与冲击之间取舍的经验形态。

## 交易成本分析作为闭环

上面的度量不是会计练习，它闭合了一个环。**事前**，用订单规模、日均成交量、波动率、价差和预期参与率预估成本，并据此选择调度与算法。**事后**，把实现的差额归因回延迟、执行、机会和费用四项，而决策价格必须事先固定，归因才有意义。**基准选择**本身是会带来后果的决策，因为采用随机基准既改变被度量的对象，也改变最优策略。**统计显著性**是长期难题：单笔成本的噪声相对其均值极大，可信的比较需要大量订单并对规模、波动率、流动性做控制，而不是靠几个轶事。

归因的产物直接反哺调度器和下单逻辑：哪些切片偏贵、成本来自等待还是冲击、参与率是否过于保守或激进。这条"度量—校准—执行栈工程"的闭环，是第 13 章的主题。

## 本章依据的文献

- Leinweber, D. J. (2002). *Using Information from Trading in Trading and Portfolio Management: Ten Years Later.* Caltech Social Science Working Paper 1135.
- Mittal, H. / ITG (2005). *Implementation Shortfall — One Objective, Many Algorithms.* ITG Inc. 白皮书.
- Labadie, M. & Lehalle, C.-A. (2013). *Optimal Starting Times, Stopping Times and Risk Measures for Algorithmic Trading: Target Close and Implementation Shortfall.* arXiv:1205.3482.
- Guéant, O. & Royer, G. (2014). *VWAP Execution and Guaranteed VWAP.* SIAM Journal on Financial Mathematics 5(1), 445–474. arXiv:1306.2832.
- Guéant, O. (2014). *Execution and Block Trade Pricing with Optimal Constant Rate of Participation.* Journal of Mathematical Finance 4(3), 255–264. arXiv:1210.7608.
- Frei, C. & Westray, N. (2015). *Optimal Execution of a VWAP Order: a Stochastic Control Approach.* Mathematical Finance 25(3), 612–639.
- Almgren, R. & Chriss, N. (2000/2001). *Optimal Execution of Portfolio Transactions.* Journal of Risk 3(2), 5–39.
- Bertsimas, D. & Lo, A. W. (1998). *Optimal Control of Execution Costs.* Journal of Financial Markets 1(1), 1–50.
- Lehalle, C.-A. (2013). *Market Microstructure Knowledge Needed for Controlling an Intra-Day Trading Process.* 载于 *Handbook on Systemic Risk*, Cambridge University Press. arXiv:1302.4592.
- Perold, A. F. (1988). *The Implementation Shortfall: Paper vs. Reality.* Journal of Portfolio Management 14(3), 4–9. **未获取原文，仅经由上述二手文献转引。**

## 本章要点

- 实施差额是"以决策价计价的纸上组合"与"实际执行得到的真实组合"之差，它把"执行得好不好"变成可度量的成本。
- 实施差额可分解为延迟成本、执行/冲击成本、未成交部分的机会成本与显性费用四项，四者相加回到总差额；哪项占主导取决于速度，慢调度付延迟与机会成本，快调度付冲击，Almgren–Chriss 前沿就是这个取舍的定量形式。
- Perold 1988 原文未获取，本章的定义与分解系据实际拿到并使用该分解的二手文献重建，文中已明确标注来源。
- 各算法把参照价定在不同位置——TWAP 取时间加权均价、VWAP 取成交量加权均价、POV 不设价格参照、IS/到达价取决策价——而到达价在决策瞬间已知、VWAP 与目标收盘是随机的，因此以随机价为基准会把基准自身的方差叠加进被测成本，并改变最优策略。
- Guéant–Royer 证明无永久冲击时 VWAP 最优曲线恰为相对成交量曲线；Guéant 给出 POV 最优参与率的闭式解；Frei–Westray 表明 VWAP 跟踪器等于 TWAP 加成交量驱动的修正。
- Labadie–Lehalle 表明目标收盘与 IS 是同一条递推式的正反两个方向，故以收盘为基准会重塑最优调度及其对波动率的敏感度。
- Leinweber 的交易数据显示实施差额很大、最小笔交易主导总成本，且成本并非随交易规模单调变化。
- 交易成本分析形成"事前预估—事后归因—基准选择—统计显著性"的闭环，并反哺调度与下单（第 13 章）。
