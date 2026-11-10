# AC 之后：稳健性、供需动态与随机控制

第 5 章的 Almgren–Chriss 模型把一个问句答得干干净净，却把另外四个问题藏了起来：它假定价格无漂移、波动率恒定，假定永久冲击与临时冲击都关于交易速率线性，假定流动性是一个既不会被耗尽也不会被补回的常数；它用"期望成本 + 方差惩罚"衡量成败，而这个准则不具备时间一致性；它还把整条调度曲线事先定死。此后二十年的工作几乎就是逐条松开这四颗螺丝，而每松开一颗，模型和最优策略的形状都会变。本章沿用 Donnelly（2022）的分类框架来安放这些结果。

## 备选方案的版图

Donnelly（2022）先把基线模型在离散与连续两种时间设定下钉住，再把后续文献分成三组：**动态假设**的变更（随机冲击、随机波动率、复原与瞬态冲击、非线性冲击）、**目标函数**的变更（投机与信号、对冲、VWAP 追踪）以及**其他现象**（多主体、稳健性、用限价单执行）。这套框架对本章的价值一半是正向的——它标出了每篇论文究竟松开了哪条假设、代价是什么；一半是反向的——它本身就是一部"哪些假设最脆弱"的清单。保留结构最多的版本是连续时间问题：控制量是交易速率 $\nu_t$，库存 $dQ^\nu_t = \nu_t\,dt$，中价 $dS^\nu_t = f(\nu_t)\,dt + dM_t$，成交价 $\tilde S^\nu_t = S^\nu_t + g(\nu_t)$，目标是最大化带清算罚项的清仓价值 $H^\nu = \mathbb{E}\left[X^\nu_T + Q^\nu_T\left(S^\nu_T - l(Q^\nu_T)\right)\right]$；换成 CARA 效用，就得到与 AC 调度曲线互为连续时间孪生的 $\sinh$ 轨迹。

## 对漂移与方向的依赖

均值–方差准则有两个毛病，Gatheral 与 Schied（2011）试图一并除掉：它时间不一致；在价格分布不是常参数时，求均值–方差最优策略要靠繁重的数值方法，而且比较静态结果还会出现反直觉。他们的替代方案是用"持仓在时间上的平均 VaR"的期望代替方差惩罚，在几何布朗运动中价下最小化

$$\mathbb{E}\left[C(x) + \int_0^T \tilde\lambda\, x_t\left(S^0_t + \gamma x_t\right)dt\right].$$

当 $\gamma>0$ 时最优自适应策略为

$$x^*_t = \sinh\!\left(\nu(T-t)\right)\left[\frac{X}{\sinh(\nu T)} - \frac{\lambda}{2\nu}\int_0^t \frac{S^0_s}{1 + \cosh(\nu(T-s))}\,ds\right],$$

其中 $\lambda = \tilde\lambda/\eta$、$\nu^2 = \tilde\lambda\gamma/\eta$；$\gamma=\nu=0$ 的退化情形是 $x^0_t = \frac{T-t}{T}X - \frac{\lambda T}{4}\int_0^t S^0_s\,ds$。两条策略都是**随盈利而激进**的——价格上涨就卖得更快——而且都不依赖于波动率 $\sigma$。

Schied（2013）给出了更一般的命题：只要 $S^0$ 是任意右连续、平方可积的**鞅**，最小化 $\mathbb{E}\int_0^T(\dot x_t^2 + \lambda S^0_t x_t + \nu^2x_t^2)\,dt$ 的唯一解就是上式的 $x^*_t$；同一策略还最小化稳健目标 $\sup_{Q\in\mathcal Q}\mathbb{E}_Q[\cdot]$，其中 $\mathcal Q$ 是任意等价鞅测度集合。也就是说，最优性只依赖**鞅性**，不依赖具体分布律。一旦放弃鞅假设，漂移并非被忽略，而是以**积分形式**进入：对一般平方可积半鞅，令 $Y_t := -(S^0_t - S_0)/\eta + \lambda\int_0^t S^0_s\,ds$，最优策略是 $\int \sinh(\nu(T-u))\,dY_u$ 的条件期望的积分（原文定理 2）；纯期望成本最小化（推论 2）同样只含积分形式的漂移项。Schied 的判断是：漂移的误设会"被平均掉"。

换成**瞬态冲击**，结论立刻翻转。Lorenz 与 Schied（2013）在 Obizhaeva–Wang 的线性瞬态冲击模型里解同一个期望成本问题，但容许一般漂移 $A$。其定理 1 极为尖锐：若 $A$ 不绝对连续，期望成本的上确下界为 $-\infty$——不存在最优策略，交易者可以靠"冲击发生后订单簿缓慢复原"这一时间差无限获利；若 $A$ 绝对连续且导数 $A'$ 平方可积，则有限的最优值为

$$\inf \mathbb{E}[C(X)] = -xS_0 + \frac{x^2}{2+\rho T} + \frac{xY_0}{2+\rho T} - \frac{\rho}{4}\mathbb{E}\int_0^T\!\left(\frac{Y_s}{2+\rho(T-s)} - \frac{1}{\rho}A'_s\right)^2 ds,$$

且当 $A'$ 有界时，唯一最优策略中的持仓直接依赖漂移的**导数** $A'_t$，因而会剧烈波动。对比之下，AC 框架里漂移以积分形式进入、误设无害，瞬态冲击下它却以导数形式进入、误设致命。

## 线性冲击与非线性冲击

AC 把两个冲击函数都取成线性，而实证并不支持。Bouchaud（2010）报告冲击关于成交量是凹的幂律 $R\propto v^{\psi}$（$\psi\le1$）：单笔交易约为 0.1 至 0.3，而机构自身的大单（metaorder）约为 $1/2$，即平方根律 $R = A\,\sigma\sqrt{v/V}$，Almgren 等（2005）也得到同样形式。这里必须把两类陈述分清：冲击的凹性是**实证规律**，不是定理，而它恰恰是线性模型排除在外的区域。

对此有两条回应。其一是理论层面的。Gatheral 定理（由 Guéant（2013）转述）指出：在 AC 分解 $dS_t=\sigma\,dW_t-k(v_t)dt$ 中，若 $k(v)=kv$ 则任意闭合回路 $q(0)=q(T)$ 都满足 $\mathbb{E}[X_T]\le X_0$；若临时冲击成本 $h$ 为零而 $k$ 非线性，则存在动态套利——线性永久冲击是唯一不产生操纵的设定。Guéant 让**微观**冲击依赖已成交的累计量来调和矛盾：$dS_t = \sigma\,dW_t - f(|q_0-q_t|)v_t\,dt$，$f>0$ 且递减。命题 2 证明此时任意绝对连续闭合回路满足 $\mathbb{E}[X_T] = X_0 - \int_0^T h(t,v_t)v_t\,dt \le X_0$，无动态套利；命题 3 进一步给出完整交易的**宏观**冲击 $S_{T'}-S_0=-F(q_0)+\sigma W_{T'}$（$F(z)=\int_0^zf(|y|)dy$），在幂律 $f(q)=q^{1-\alpha}$ 下 $F(z)=z^{2-\alpha}/(2-\alpha)$，是凹的。于是凹性从"被假设的冲击函数"移到了"微观冲击的状态依赖"上。

其二是 Almgren（2003）。它给临时冲击加一个随机分量，$\tilde S_n = S_n + g(V_n) + h(V_n)\xi_{n+1}$。在确定性交易下期末财富方差变成 $V[X_T]=\sum_n\sigma^2Q_n^2+\sum_n V_n^2h^2(V_n)$，第二项即**交易放大的风险**（trading-enhanced risk）：交易本身越大，成交时所遭遇流动性的不确定性所贡献的方差越大。该文未取得，本段模型、目标与以下闭式路径均转述自 Donnelly（2022）§4.4。临时冲击 $g(\nu)=k\nu^m$ 时，最优库存路径是幂律：$0<m<1$ 为 $q_0\left(1+\frac{1-m}{1+m}\frac{t}{T^*}\right)^{(1+m)/(1-m)}$，$m=1$ 为指数 $q_0e^{-t/T^*}$，$m>1$ 为 $q_0\left(1-\frac{m-1}{m+1}\frac{t}{T^*}\right)^{(m+1)/(m-1)}$；若冲击含随机但幅度无关的部分 $h(\nu)=a$，路径又回到指数 $q_0e^{-t/T^*}$，其中 $T^*=(2k+\gamma a^2)/(\gamma\sigma^2)$。随机冲击项使交易慢于确定性基准——交易者宁愿多担一点价格风险，也要压低"碰上坏流动性"的风险。

## 随机的流动性与波动率

若冲击系数和波动率都在动，常参数调度就是错的。这正是 Almgren（2009）与 Forsyth（2011）所放松的方向。综述里明确记下了"插值启发式"：把当前观测到的随机量代入常参数策略，即 $k\to k_t$、$\sigma\to\sigma_t$，其含义是冲击低、波动高或临近到期时加快交易。Almgren 原文未取得，以上按 Donnelly（2022）§4.1–4.2 转述。

Forsyth（2011）说明了这类问题为何需要数值方法。他把**预承诺**（pre-commitment）的均值–方差目标——即只在初始时刻衡量得失、然后照此执行——用拉格朗日乘子嵌入线性二次问题，再对所得非线性 HJB 偏微分方程用**半拉格朗日**格式求解；该格式单调、相容且稳定，在强比较原理成立时收敛到 HJB 方程的**粘性解**。相似性约化还把原先三维的问题降到二维。回报是覆盖面：一次非线性求解加一次线性求解即可给出**整条**有效前沿，且不要求冲击是二次型（价格仍取几何布朗运动）。但它的数值结论本身就是对准则而非对调度的警告：**"在某些情形下，有很多不同的交易策略会给出几乎相同的有效前沿。"** 均值–方差目标是一个很弱的判别器，AC 调度只是那个近乎最优的大家族中的一员。

## 供需动态：确定性策略为何不再最优

以上所有模型都假定只有交易者自己的下单在推动订单簿，且冲击是交易量的函数。Obizhaeva 与 Wang（2013）两条都推翻。他们把单边限价订单簿显式地写出来：一笔 $x_0$ 的交易把中间价线性推高 $V_{0+}=F_0+\lambda x_0$，卖一跳到 $A_{0+}=F_0+s/2+x_0/q$，随后以指数速度回到稳态，

$$A_t = V_t + s/2 + \sum_{t_i<t} x_{t_i}\,\kappa\,e^{-\rho(t-t_i)}, \qquad \kappa = 1/q - \lambda,$$

其中 $\rho$ 是**复原速度**（resilience），即新订单补回缺口的快慢；$D_t = A_t-V_t-s/2$ 是卖一相对稳态的偏离。AC/Bertsimas–Lo 模型那个"成本退化为与策略无关的 $(F_0+s/2)X_0+\frac\lambda2X_0^2$"的退化极限，正是靠把供需写成**动态过程**而非静态冲击函数来修复的。

命题 3 给出值函数 $J_t=(F_t+s/2)X_t+\lambda X_0X_t+\alpha_tX_t^2+\beta_tD_t+\gamma_tD_t^2$ 与最优调度

$$x_0 = x_T = \frac{X_0}{\rho T + 2}, \qquad \mu_t = \frac{\rho X_0}{\rho T + 2}\ \ (t\in(0,T)).$$

这个式子应读出两层含义。第一，最优点不是连续的：它是 $t=0$ 与 $t=T$ 两笔集中成交，中间以**恒定速率**连续成交。所以"确定性平滑调度最优"这一 AC 结论，一旦交易时点内生化、且供给会回升，就不再成立。第二，策略完全不依赖静态冲击参数 $\lambda$ 与 $q$，只依赖动态属性 $\rho$ 与期限 $T$——静态供需在这里根本不决定调度，决定它的是订单簿的复原。$\rho\to0$ 时退化为两笔纯集中成交，$\rho\to\infty$ 时退化为纯连续流。

最优还确实降低了成本。相对于恒定速率基准的额外成本 $(\lambda/2)X_0^2+\kappa\frac{\rho T-(1-e^{-\rho T})}{(\rho T)^2}X_0^2$，最优策略只付 $(\lambda/2)X_0^2+\frac{\kappa}{\rho T+2}X_0^2$，差额恒非负。幅度依状态而定：取 $X_0=100{,}000$ 股、初始卖一 100、簿深 $q=5{,}000$、一个交易日，两端集中成交从 $\rho=0.001$ 时的 49,975 股降到 $\rho=5$ 时的 14,286 股、$\rho=50$ 时的 1,921 股，中间的连续部分相应变大。

## 瞬态冲击与价格操纵

Obizhaeva–Wang 假定订单簿是块状的。Predoiu、Shaikhet 与 Shreve（2011）把它推广到任意单边簿：影子簿由一个（可能带原子的）测度描述，卖一为 $A_t+D_t$，$D_t=\psi(E_t)$，$\psi$ 是分布函数的左连续逆。**成交量效应过程** $E$（仍留在簿上的那部分冲击）满足

$$E_t = X_t - \int_0^t h(E_s)\,ds,$$

$h$ 是簿的恢复速率。把未受影响价格归一为零后，一条确定的非减策略的成本是 $C(X)=\Phi(E_T)+\int_0^T D_t h(E_t)\,dt$，$\Phi(y)=\int_0^y\psi(\eta)\,d\eta$。主结果（定理 4.2、4.5）说：当 $g(y):=y\psi(h^{-1}(y))$ 凸时，存在唯一的 **Type A** 最优策略——在 $0$、某个中间时刻 $t_0$ 与 $T$ 各买一笔集中量，中间以恒定速率买——使卖一相对恢复路径保持恒定价差；$g$ 不凸时最优解是 $g$ 的凸包，"Type B"构造可达到它。边界的集中成交加内部的恒定速率这一结构跨各种簿形都稳健，这正是执行算法可以不依赖簿形的强论据。

这里排除各种病态的是**价格操纵**的禁令——从零头寸出发、来回交易、最终回到零，却赚到正收益。一般条件的出处是 Alfonsi 与 Schied（2010），该文未取得；我只能按 Predoiu–Shaikhet–Shreve 的转述报告其结论：在交易时点有限的模型里，"价格操纵在非常一般的条件下不可能"，而且执行整体买单时做中间卖单永远不利。能从已获取文献中写出的，是一条更强也更窄的条件：在线性瞬态冲击、清算罚项 $\alpha$、永久冲击 $b$ 的模型中，Donnelly（2022）指出优化问题的凹性要求 $2\alpha>b$；取反号时不凹，交易者"可以通过价格操纵赚取无限利润"。而在 AC 分解内部，Guéant（2013）转述的 Gatheral 定理给出了精确边界：线性永久冲击不产生操纵，任何非线性且临时成本为零的设定都存在动态套利，而 Guéant 的"依赖累计成交量的微观冲击"能在不产生套利的前提下恢复非线性。

## 块交易：把执行成本变成可报价的量

调度回答"怎么交易"，Guéant（2015）回答"一块头寸值多少"。他的框架是 CARA 投资者在 $[0,T]$ 内清仓 $q_0$ 股，执行成本函数 $L$ 一般地严格凸、超线性，市场成交量曲线 $V_t$ 确定，动力学为

$$dS_t = \sigma\,dW_t - f(|q_0-q_t|)v_t\,dt, \qquad dX_t = v_t S_t\,dt - V_t L\!\left(\tfrac{v_t}{V_t}\right)dt - \psi|v_t|\,dt,$$

AC 对应 $L(\rho)=\eta\rho^2$。定理 2.1 给出唯一的极小点 $q^*$，它非负、非增，且与线性费用 $\psi$ 无关；命题 2.2 用哈密顿系统刻画它：$\dot p=\gamma\sigma^2q$、$\dot q=V_tH'(p)$（$H$ 为 $L$ 的 Legendre 变换），边界条件 $q(0)=q_0$、$q(T)=0$——这一刻画不预设 $q^*$ 光滑，且在 AC 情形复现 $\sinh$ 路径。定理 2.3 把离散时间的直觉说成定理：任何随机策略都不会优于最好的确定性策略，$\sup_{v\in\mathcal A}\mathbb{E}[-\exp(-\gamma X_T)] = \sup_{v\in\mathcal A^{0,\det}}\mathbb{E}[-\exp(-\gamma X_T)]$；但在 Black–Scholes 价格、CARA 之外或随机成交量下，该结论失效。

定价结果是**无差异定价**。一个必须在一段时间内清仓 $q_0$ 股的组合，交易者愿意支付的最高价是

$$P(T,q_0,S_0) = q_0S_0 - \int_0^{q_0} F(z)\,dz - \psi q_0 - I_0(q^*),$$

$F(z)=\int_0^zf(|y|)dy$。四项分别是盯市价值、与策略无关的永久冲击折价、线性成本，以及经优化后的风险与冲击成本 $I_0(q^*)$——只有最后这项能靠"交易得好"来削减。这里的 $I_0(q^*)$ 来自值函数

$$\theta_T(t,q)=\inf_{q\in AC_{q,0}(t,T)}\int_t^T\left(V_sL(\dot q/V_s)+\tfrac12\gamma\sigma^2q^2\right)ds,$$

于是长度为 $t$ 的窗口内清仓 $q$ 股的价格为 $qS-\int_0^qF(z)dz-\psi q-\theta_T(T-t,q)$。执行成本由此变成一个可报价的数：经纪商承接下来这笔块头寸所必须收取的流动性溢价。

## 时间不一致与预承诺

方差惩罚是 AC 最深的结构性问题。因为 $\mathrm{Var}(C)$ 里含一个期望的平方，均值–方差准则**时间不一致**：$t=0$ 时的最优策略在任何更晚时刻都不再最优，动态规划也因此不能直接用。Forsyth（2011）把"预承诺"解释讲透——先解初始时刻的取舍，然后照此执行——并指出它的最优只是平均意义下的：在大量同质交易上，它在给定标准差下拥有最大的期望收益。Forsyth、Kennedy、Tse 与 Windcliff（2012）走另一条路，用**二次变差**替换方差，目标改为 $\mathbb{E}[\text{成本}]+\kappa_1\mathbb{E}\int_0^T(\text{持仓})^2dt+\kappa_2\mathbb{E}[\text{变差}]$，它时间一致，可套用标准 HJB。其结果颇出人意料：算术布朗运动下该问题的精确最优解**与静态 AC 解重合**，经典 $\sinh$ 调度因此是一个自洽准则的精确最优；而在几何布朗运动下最优解一般依赖价格，但静态策略即使在高波动率下也仍是极好的近似。

另一条路是保留时间一致性、换掉准则本身。Schied 与 Schöneborn（2009）在一般 von Neumann–Morgenstern 效用下最大化期末财富期望，用非线性 HJB 方程刻画整条自适应策略，最优反馈为 $c(X,R)=-v_X/(2\lambda v_R)$。定理 5 把调整方向与效用形状绑定：卖出速度随财富递增，当且仅当绝对风险厌恶递增——风险厌恶递减、不变、递增分别对应"随盈利而被动、中性、随盈利而激进"。CARA 下最优策略恰是指数确定性路径 $X_0\exp(-t\sqrt{\sigma^2A/(2\lambda)})$，与均值–方差最优完全相同——没有信息信号时，自适应什么也换不来。而一旦离开 CARA，比较静态会反直觉：更高的临时冲击系数可能**提高**卖出速度，更大的头寸也可能让清仓变慢。

## 从调度曲线到反馈规则

这些修正的共同主线，是把事先算好的调度换成对市场作出反应的规则。Gatheral–Schied 与 Schied（2013）给出闭式的"随盈利而激进"规则，不需要知道价格分布律；Schied–Schöneborn 给出由偏微分方程刻画的自适应规则，并说明它何时不同于确定性规则。Brigo 与 Piat（2016）在两个离散基准模型里量化这个差距：无信号时确定性最优解正是 AC/TWAP 调度，而自适应策略可证至少一样好，只要信号有信息量就严格更好。Almgren 与 Lorenz 的 *Adaptive Arrival Price* 是同一思想的实务表达，该文未取得，此处依据前两篇。

对追踪基准的订单，闭环有独特形状。Frei 与 Westray（2015）求解 VWAP 问题——同时最小化相对市场 VWAP 的滑点的均值与方差，而基准本身随机——用 gamma bridge 刻画日内相对成交量曲线，冲击为线性。其最优速率 $\hat u(s)=-\left(2a(s)\hat X(s)+b(s)\gamma(s)+c(s)\right)/(2\kappa)$ 分解为确定性 TWAP 分量加上由成交量曲线驱动的调整，且最多只会反向一次。Cartea 与 Jaimungal 的 VWAP 闭式策略未取得，此处依据 Frei–Westray。

## 本章要点

- Gatheral–Schied 策略随盈利而激进、与波动率无关；Schied（2013）证明它对**任意**平方可积鞅都最优，而价格是一般半鞅时漂移以积分形式进入，误设会被平均掉。
- 瞬态冲击下结论相反：漂移不绝对连续时期望成本问题不适定，且最优持仓依赖漂移的**导数**。
- 冲击的凹性是实证规律而非定理；Guéant（2013）说明依赖成交量的微观永久冲击可以既凹又无套利，而线性永久冲击是唯一不产生操纵的常系数设定。
- Obizhaeva–Wang 说明：交易时点内生化、订单簿以速率 $\rho$ 复原时，最优策略是两笔集中成交加中间恒定速率，只依赖 $\rho$ 与 $T$，且严格优于常规连续调度。
- Predoiu–Shaikhet–Shreve 在 $g(y)=y\psi(h^{-1}(y))$ 凸时把该结构推广到任意簿形；Guéant 则把执行成本变成块交易的无差异定价。
- 均值–方差准则时间不一致；预承诺（Forsyth 2011）与均值–二次变差（Forsyth 等 2012）修复了它，后者在算术布朗运动下精确复现 AC 静态调度。
- 闭环规则取代调度曲线：Schied–Schöneborn 把自适应方向与风险厌恶形状绑定，Frei–Westray 把 VWAP 追踪分解为 TWAP 分量加成交量驱动的调整。

## 本章依据的文献

- Ryan Donnelly, "Optimal Execution: A Review", *Applied Mathematical Finance* 29(3), 181–212 (2022), §3–4.4、§6.2。本地 PDF：`materials/papers/cluster-c/2022-donnelly-optimal-execution-review.pdf`。
- Alexander Schied, "Robust Strategies for Optimal Order Execution in the Almgren–Chriss Framework", *Applied Mathematical Finance* 20(3), 264–286 (2013), 定理 1、2 及推论 1、2。本地 PDF：`materials/papers/cluster-c/2013-schied-robust-strategies-ac.pdf`。
- Christopher Lorenz and Alexander Schied, "Drift Dependence of Optimal Trade Execution Strategies under Transient Price Impact", *Finance and Stochastics* 17(4), 743–770 (2013), 定理 1 及推论 1、2。本地 PDF：`materials/papers/cluster-c/2013-lorenz-schied-drift-dependence.pdf`。
- Anna Obizhaeva and Jiang Wang, "Optimal Trading Strategy and Supply/Demand Dynamics", *Journal of Financial Markets* 16(1), 1–32 (2013), §3、§6–7 及 Table 1。本地 PDF：`materials/papers/cluster-c/2013-obizhaeva-wang-supply-demand-dynamics.pdf`。
- Silviu Predoiu, Gennady Shaikhet and Steven Shreve, "Optimal Execution in a General One-Sided Limit-Order Book", *SIAM Journal on Financial Mathematics* 2(1), 183–212 (2011), 定理 3.2、4.2–4.5。本地 PDF：`materials/papers/cluster-c/2011-predoiu-shaikhet-shreve-onesided-lob.pdf`。
- Olivier Guéant, "Permanent Market Impact Can Be Nonlinear", arXiv:1305.0413 (2013), 定理 1、命题 2–3。本地 PDF：`materials/papers/cluster-c/2013-gueant-permanent-impact-nonlinear.pdf`。
- Olivier Guéant, "Optimal Execution and Block Trade Pricing: A General Framework", *Applied Mathematical Finance* 22(4), 336–365 (2015), 定理 2.1–2.3 及 §3。本地 PDF：`materials/papers/cluster-c/2015-gueant-block-trade-general-framework.pdf`。
- Peter Forsyth, "A Hamilton–Jacobi–Bellman Approach to Optimal Trade Execution", *Applied Numerical Mathematics* 61(2), 241–265 (2011), §2、§8。本地 PDF：`materials/papers/cluster-c/2011-forsyth-hjb-optimal-execution.pdf`。
- P. Forsyth, J. Kennedy, S. Tse and H. Windcliff, "Optimal Trade Execution: A Mean–Quadratic-Variation Approach", *Journal of Economic Dynamics and Control* 36(12), 1971–1991 (2012), §8 及命题 2。本地 PDF：`materials/papers/cluster-c/2012-forsyth-mean-quadratic-variation.pdf`。
- Alexander Schied and Torsten Schöneborn, "Risk Aversion and the Dynamics of Optimal Liquidation Strategies in Illiquid Markets", *Finance and Stochastics* 13(2), 181–204 (2009), 定理 1、5、6 及推论 4。本地 PDF：`materials/papers/cluster-c/2009-schied-schoeneborn-risk-aversion.pdf`。
- Christoph Frei and Nicholas Westray, "Optimal Execution of a VWAP Order: A Stochastic Control Approach", *Mathematical Finance* 25(3), 612–639 (2015), 定理 3.1。本地 PDF：`materials/papers/cluster-b/2015-frei-optimal-execution-vwap-order.pdf`。
- Damiano Brigo and Clément Piat, "Static vs Adapted Optimal Execution Strategies in Two Benchmark Trading Models", arXiv:1609.05523 (2016), 定理 2.8–2.10、命题 2.11–2.17。本地 PDF：`materials/papers/cluster-c/2016-brigo-piat-static-vs-adapted.pdf`。
- Jean-Philippe Bouchaud, "Price Impact", in *Encyclopedia of Quantitative Finance* (Wiley, 2010), 凹形冲击部分。本地 PDF：`materials/papers/cluster-b/2010-bouchaud-price-impact.pdf`。
- Robert Almgren, "Optimal Execution with Nonlinear Impact Functions and Trading-Enhanced Risk", *Applied Mathematical Finance* 10(1), 1–18 (2003) —— **未取得**；内容转述自 Donnelly (2022) §4.4。
- Robert Almgren, "Optimal Trading with Stochastic Liquidity and Volatility", *SIAM Journal on Financial Mathematics* 3(1), 163–181 (2009) —— **未取得**；内容转述自 Donnelly (2022) §4.1–4.2。
- Aurélien Alfonsi and Alexander Schied, "Optimal Trade Execution and Absence of Price Manipulations in Limit Order Book Models", *SIAM Journal on Financial Mathematics* 1(1), 490–522 (2010) —— **未取得**；其无操纵结论按 Predoiu–Shaikhet–Shreve (2011) §1 的转述报告。
- Robert Almgren and Julian Lorenz, "Adaptive Arrival Price", *Journal of Trading* 2(1) (2007) —— **未取得**；"随盈利而激进"的概念此处依据 Schied–Schöneborn (2009) 与 Brigo–Piat (2016) 展开。
