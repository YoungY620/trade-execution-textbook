# 做市与随机控制视角

做市是执行的镜像。主动执行者走向订单簿、为即时性付出价差；做市商站在簿中、双边报价、靠站在这里收取价差。两者都持有一份不想要的库存，价格朝不利方向变动时都受罚，也都希望只与无信息的对手成交。写成随机控制，它们是同一个问题、只是控制量互换：执行者选择交易速率并付钱，做市商选择一对报价距离并收钱。本章推导做市商那一版，并用它从另一侧读出执行者的那一版。

## 控制问题

$t$ 时刻的标准状态是三元组（库存 $q$、现金 $X$、参考价或中间价 $S$）。做市商控制报价相对中间价的两个距离：卖出距离 $\delta^a=p^a-S$ 与买入距离 $\delta^b=S-p^b$。中间价是局部鞅，$dS_u=\sigma\,dW_u$，$S_t=s$，因此没有可预期的漂移可供交易，这一点后面放宽。库存与现金只在市价单到达时变动，

$$q_t=N^b_t-N^a_t,\qquad dX_t=(S_t+\delta^a_t)\,dN^a_t-(S_t-\delta^b_t)\,dN^b_t,$$

其中 $N^a$（$N^b$）计数吃掉卖价、打到买价的市价单。关键的建模一步是到达强度：报价离中间价越远越难成交，故取递减函数 $\lambda^a(\delta^a)$ 与 $\lambda^b(\delta^b)$。目标是期末盯市财富的期望效用，

$$u(s,x,q,t)=\max_{\delta^a,\delta^b}\mathbb E_t\!\left[-\exp\!\left(-\gamma(X_T+q_TS_T)\right)\right],$$

$\gamma>0$ 为绝对风险厌恶。动态规划原理把它变成 Hamilton–Jacobi–Bellman（HJB）方程，局部项为 $u_t+\tfrac12\sigma^2u_{ss}$，非局部项对两个跳跃项
$\lambda^b(\delta^b)[u(s,x-s+\delta^b,q+1,t)-u]$ 与对称的卖方向取极大。这个结构——价格的扩散、成交的泊松跳、对报价距离的极大化、期末效用——就是以下一切的模板。

## Avellaneda–Stoikov：保留价格与最优价差

Avellaneda 与 Stoikov 用指数衰减的到达强度封闭模型，这一形式来自经济物理学中幂律的报单规模与冲击统计：

$$\lambda^a(\delta^a)=Ae^{-k\delta^a},\qquad \lambda^b(\delta^b)=Ae^{-k\delta^b}.$$

$A$ 是零距离处的强度，量纲为每单位时间的成交次数；$k$ 是成交强度对价格的衰减率，使 $k\delta$ 无量纲。在指数效用下，试探解 $u=-\exp(-\gamma x)\exp(-\gamma\theta(s,q,t))$ 把现金变量剥离。代理对股票的主观估值由**保留价格**（也称无差异价格）概括，即按持仓调整后的中间价：

$$r(s,q,t)=s-q\gamma\sigma^2(T-t).$$

修正项 $-q\gamma\sigma^2(T-t)$ 就是库存偏移：多头（$q>0$）把股票估得低于中间价、倾向压低报价卖出，空头则抬高报价买回；当 $t\to T$ 时该项趋于零，因为临近到期任何库存都能按中间价平掉。把"冻结库存"下的价格 $\theta(s,q+1,t)-\theta(s,q,t)$ 与 $\theta(s,q,t)-\theta(s,q-1,t)$ 分开读，得到买、卖保留价 $r^b(s,q,t)=s+\tfrac{(-1-2q)\gamma\sigma^2(T-t)}{2}$ 与 $r^a(s,q,t)=s+\tfrac{(1-2q)\gamma\sigma^2(T-t)}{2}$，其平均即 $r$。

最优报价距离由隐式一阶条件给出
$s-r^b(s,q,t)=\tfrac1\gamma\ln\!\left(1-\tfrac{\lambda^b(\delta^b)}{\gamma\,(\partial\lambda^b/\partial\delta)(\delta^b)}\right)$，卖方向对称。对指数强度，以 $r$ 为中心的总价差有闭式：

$$\delta^a+\delta^b=\gamma\sigma^2(T-t)+\frac{2}{\gamma}\ln\!\left(1+\frac{\gamma}{k}\right).$$

两点要紧。第一，价差分成风险项 $\gamma\sigma^2(T-t)$（随波动率、风险厌恶与剩余持仓时间增大）与流动性项 $\tfrac2\gamma\ln(1+\gamma/k)$（随 $\gamma$ 与 $k$ 增大）——报单集中在中间价附近（$k$ 大）的市场更容易成交，做市商可以报得更紧。第二，价差*与库存无关*：指数强度下库存信息全部由偏移 $r-s$ 承载，而非宽度。这是指数形式的产物而非普遍规律，后面的模型会打破它。作者模拟（1000 条路径，$s=100$，$T=1$，$\sigma=2$，$\gamma=0.1$，$k=1.5$，$A=140$）中，库存策略均值盈亏 65.0、标准差 6.6、期末库存 0.08，对照围绕中间价对称的基准为均值 68.4、标准差 12.7、库存 0.26：平均收益略低，方差约为一半，库存向零均值回复。

## Guéant–Lehalle–Fernandez-Tapia：库存上界与谱解法

Avellaneda–Stoikov 只是近似：闭式来自对 $q$ 的一阶展开，且报价的容许性此前是未决问题。Guéant、Lehalle 与 Fernandez-Tapia 用硬库存上界 $q\in\{-Q,\dots,Q\}$（$q=Q$ 不报买价，$q=-Q$ 不报卖价）取代展开，精确求解。记 $\alpha=\tfrac12 k\gamma\sigma^2$、$\eta=A\left(1+\tfrac{\gamma}{k}\right)^{-(1+k/\gamma)}$，HJB 化为关于各层 $v_q(t)$ 的线性三对角常微分方程组

$$\dot v_q(t)=\alpha q^2 v_q(t)-\eta\left(v_{q-1}(t)+v_{q+1}(t)\right),$$

两端为吸收式改动、$v_q(T)=1$，值函数为 $-\exp(-\gamma(x+qs))\,v_q(t)^{-\gamma/k}$。最优报价为

$$\delta^{b*}(t,q)=\frac1k\ln\!\left(\frac{v_q(t)}{v_{q+1}(t)}\right)+\frac1\gamma\ln\!\left(1+\frac{\gamma}{k}\right),\qquad
\delta^{a*}(t,q)=\frac1k\ln\!\left(\frac{v_q(t)}{v_{q-1}(t)}\right)+\frac1\gamma\ln\!\left(1+\frac{\gamma}{k}\right),$$

价差
$\psi^*(t,q)=-\tfrac1k\ln\!\left(\tfrac{v_{q+1}(t)v_{q-1}(t)}{v_q(t)^2}\right)+\tfrac2\gamma\ln\!\left(1+\tfrac{\gamma}{k}\right)$。
$v_q$ 是某个生灭问题的各层，且远离到期时报价几乎与 $t$ 无关——这与展开所暗示的"价差随时间增大"相悖。令 $T\to\infty$，渐近报价为
$\delta_\infty^{b*}(q)=\tfrac1\gamma\ln(1+\tfrac\gamma k)+\tfrac1k\ln(f^0_q/f^0_{q+1})$，
$f^0$ 是三对角矩阵最小特征值对应的特征向量。把有限格点换成 $L^2(\mathbb R)$ 得高斯型
$f^0(x)\propto\exp(-\tfrac12\sqrt{\alpha/\eta}\,x^2)$，以及著名的、关于库存仿射的近似：

$$\delta_\infty^{b*}(q)\simeq\frac1\gamma\ln\!\left(1+\frac{\gamma}{k}\right)+\frac{2q+1}{2}\sqrt{\frac{\sigma^2\gamma}{2kA}\left(1+\frac{\gamma}{k}\right)^{1+k/\gamma}},$$

卖方向对称，且
$\psi_\infty^*(q)\simeq\tfrac2\gamma\ln(1+\tfrac\gamma k)+\sqrt{\tfrac{\sigma^2\gamma}{2kA}(1+\tfrac\gamma k)^{1+k/\gamma}}$。
报价对库存的敏感度就是根号因子 $\sqrt{\sigma^2\gamma/(2kA)}$：波动率与风险厌恶越大报价越宽，成交强度越大报价越紧。比较静态给出 $\partial\psi_\infty^*/\partial\sigma^2>0$、$\partial\psi_\infty^*/\partial A<0$、$\partial\psi_\infty^*/\partial k<0$；$\gamma$ 的作用则是模糊的，因为更厌恶风险的做市商一方面为价格风险而放宽价差，一方面又为降低不成交风险而收窄价差。

## Guéant–Royer：一般强度、库存惩罚与多标的

Guéant 用期末库存惩罚 $\ell(|q_T|)$（非降凸函数）取代硬上界，并允许强度 $\Lambda^b,\Lambda^a$ 为任意递减函数（只需关于 $\Lambda\Lambda''/(\Lambda')^2$ 的一个温和条件，保证单笔问题 $\delta\Lambda(\delta)$ 有唯一极大点）。两个目标属于同一族：

$$\text{(Model A)}\quad \sup\mathbb E\!\left[-\exp\!\left(-\gamma(X_T+q_TS_T-\ell(|q_T|))\right)\right],$$

$$\text{(Model B)}\quad \sup\mathbb E\!\left[X_T+q_TS_T-\ell(|q_T|)-\tfrac12\gamma\sigma^2\!\int_0^T q_t^2\,dt\right].$$

Model A 是期末财富的 CARA 效用；Model B 是 Cartea–Jaimungal 型准则，通过运行项*持续*惩罚库存。两者都归结为关于 $\theta(t,q)$ 的同一个方程

$$0=-\partial_t\theta(t,q)+\tfrac12\gamma\sigma^2q^2-\mathbf 1_{q<Q}H^b_\xi\!\left(\tfrac{\theta(t,q)-\theta(t,q+\Delta)}{\Delta}\right)-\mathbf 1_{q>-Q}H^a_\xi\!\left(\tfrac{\theta(t,q)-\theta(t,q-\Delta)}{\Delta}\right),$$

终值 $\theta(T,q)=-\ell(|q|)$，其中
$H^b_\xi(p)=\sup_\delta\tfrac{\Lambda^b(\delta)}{\xi}(1-e^{-\xi\Delta(\delta-p)})$，
$\Delta$ 为成交规模，Model A 取 $\xi=\gamma$、Model B 取 $\xi=0$。参数 $\xi$ 正是区分*价格风险*厌恶（Model A：运行惩罚是假的，风险只经由效用曲率进入）与*不成交风险*厌恶（Model B：$\gamma$ 惩罚持有期间的库存）的东西。指数强度下换元 $v_q(t)=\exp(\tfrac k\Delta\theta(t,q))$ 把方程组线性化，从而回到 Guéant–Lehalle–Fernandez-Tapia 的约化；把 $H_\xi$ 二阶展开并取连续极限，得近似

$$\delta^{b*}_{\text{approx}}(q)=\frac1{\xi\Delta}\ln\!\left(1+\frac{\xi\Delta}{k}\right)+\frac{2q+\Delta}{2}\sqrt{\frac{\gamma\sigma^2}{2\Delta kA}\left(1+\frac{\xi\Delta}{k}\right)^{1+k/(\xi\Delta)}},$$

在 $\xi=\gamma,\Delta=1$ 时即 Guéant–Lehalle–Fernandez-Tapia 公式，在 $\xi=0$ 时覆盖 Model B。第 5 节把整套构造推广到管理 $d$ 个相关标的、协方差阵 $\Sigma=(\rho^{i,j}\sigma^i\sigma^j)$ 的做市商：单标的矩阵 $M$ 变成矩阵 Riccati 型方程组，遍历极限下二次系数稳定在 $\tfrac12\sqrt{\gamma/2}\,\Gamma$，其中 $\Gamma=D^{-1/2}(D^{-1/2}\Sigma D^{-1/2})^{1/2}D^{-1/2}$，$D$ 汇集二阶导 $H^{i,\prime\prime}_\xi(0)$。实务含义是：对每个标的独立报价是次优的——手里一只债券做空、另一只高度相关的做多时，组合几乎持平，就不该把两只的报价都偏移，而单标的模型会建议这么做。

## Guilbaud–Pham：把限价单与市价单放进同一个问题

以上都是场外式模型：单位规模成交按泊松强度到达。Guilbaud 与 Pham 建的是真正的限价订单簿模型。时间是用数据估计强度的 tick 时钟，价差是 tick 格点上取有限值的连续时间马尔可夫链，代理可以把最优报价改进一个 tick 以获得执行优先权——报得更好成交更频繁。关键是代理还有*第二个*控制：可以发市价单。市价单是瞬时的、可在任意停时发出，故被建模为**脉冲控制**而非速率。由此得到的动态规划方程不是 HJB 方程，而是 **HJB 拟变分不等式（HJBQVI）**：

$$\min\left\{-\frac{\partial v}{\partial t}-\sup_{(q,\ell)\in\mathcal Q(s)\times[0,\bar\ell]^2}\mathcal L^{q,\ell}v+\gamma g\;,\; v-\mathcal Mv\right\}=0,$$

其中 $\mathcal L^{q,\ell}$ 是限价单动态的生成元（价格、价差链与两个成交跳跃项），脉冲算子 $\mathcal Mv=\sup_{e\in[-\bar e,\bar e]}v(t,x-c(e,p,s),y+e,p)$ 是通过*主动吃单*得到的值，成本 $c$ 为半个价差加手续费。这个不等式说的是：要么状态在最优限价策略下继续演化（第一项为零），要么代理通过发市价单跳跃（第二项为零）。普通 HJB 承载不了它，因为两个控制作用在不同的时间集上——限价单连续施加，市价单在孤立时刻施加——而取最小化正是让脉冲渠道与限价渠道竞争、而非相加的原因。这是本聚类中最贴近真实簿机制的模型，也是最干净地说明"做市与执行是同一个问题"的陈述：同一个代理既报价又吃单。

## Cont–Stoikov–Talreja：强度背后的微观基础

那些成交强度从何而来？Cont、Stoikov 与 Talreja 直接把订单簿建成 $n$ 个价位、单位规模的排队系统。其状态是各层队列长度（等价地，最优买卖价序号与各距离处的队列），六类事件驱动它：限价买、限价卖到达，市价买、市价卖，以及挂单的撤销，各为独立泊松过程，速率只依赖到最优报价的距离。所得过程是连续时间马尔可夫链；容许的（金字塔形、无交叉）状态集是吸收集，且最小市价单速率若为正，链遍历并有显式平稳分布。回报是可解性：借助两侧拉普拉斯变换对生灭过程做首达时分析，可给出**闭式条件概率**——中间价下一次变动向上、买价订单在卖价移动前成交、以及买卖双方在价格移动前都成交的概率。"成交概率"因此不是假设的 $Ae^{-k\delta}$，而是可由簿状态计算的函数。这正是报价与报单放置策略赖以建立的微观基础。

## Cartea–Jaimungal–Ricci：低买高卖

以上模型给库存风险定价，却不含方向观点。Cartea、Jaimungal 与 Ricci 通过自激与互激的订单流加入方向：中间价带有可预期的短期 alpha $\alpha_t$，买卖市价单到达强度服从耦合的 Hawkes 型动态，于是成交与簿形相互影响。做市商最大化期末财富减去运行库存惩罚 $\varphi\int q^2$。最优报价是基础距离加两项修正，

$$\delta^{\pm*}=\delta^\pm_0+B(\delta^\pm_0;\kappa^\pm)\left\{\pm\mathcal S^\pm_\lambda\,\mathbb E\!\left[\int_t^T\alpha_u\,du\right]+\varphi\left[\pm\mathcal S^\pm_\lambda b_\varphi+(1\mp2q)(T-t)\right]\right\},$$

$\delta^\pm_0$ 为风险中性报价。两个角色此时分明：alpha 项*同时*修正逆向选择并给报价定向——预期漂移为正时卖单报得更深（避免卖在上涨前）、买单报得更近（抢在上涨前买）——惩罚项则提供库存均值回复。做市与投机性持有被区分开来，做市商通过权衡价差收入与方向暴露带来的被吃单风险，来决定各占多少。

## 信号，以及与执行章的接口

两篇论文把与第 6 章的联系具体化。Lehalle 与 Neuman 研究带马尔可夫信号 $I$（alpha）与瞬时市场冲击的最优执行。最小化者由一阶条件刻画：$X^*$ 最优当且仅当存在常数 $\lambda$ 使 $\int_0^t\mathbb E_\iota[I_s]ds+\int_0^tG(|t-s|)dX^*_s-2\varphi\int_0^tX^*_s ds=\lambda$ 对一切 $t$ 成立。对 Ornstein–Uhlenbeck 信号 $dI_t=-\gamma I_t\,dt+\sigma\,dW_t$ 与指数衰减冲击，策略有闭式且*对初始库存与信号均线性*。值得注意的定性结果是策略未必单调：信号可以使代理一边清仓一边买入，而这恰是价格操纵可能成立的条件。Cartea 与 Wang 明确写出做市一侧的对应：alpha 由市价单驱动时，$|\alpha|$ 增大促使做市商撤销或推远风险一侧的限价单，越过阈值后转为沿信号方向发*市价*单。当其对库存风险的容忍度上升，策略发更多投机性市价单、做更多限价单往返，且存在一段容忍度区间使预期收益上升而收益波动几乎不变。执行与做市于是跑在同一套信号驱动的机制上，只是活动的方向相反。

## 同一个问题的两面

合起来看，本聚类是一个命题：做市与执行是同一个随机控制问题从相反两侧看的结果。Drissi 的牛津讲义把这一点作为整个领域的组织原则——最优交易问题"share a common structure"：识别决策问题、用少数状态变量概括环境、写成优化问题、再求解，无论主题是限价订单簿里的最优执行还是场外市场里的最优做市。两者的状态都是库存、现金与价格；控制只差一个符号（被动做市商报价并被成交，主动执行者吃单并付钱）；权衡也相同——库存风险、逆向选择与冲击。一张激进单就是做市商决定"吃"而非"报"，正是 Guilbaud–Pham 的脉冲渠道；一张挂单就是执行者决定"报"而非"吃"。Guéant 指出许多做市模型"can be regarded as generalizations of optimal execution models"，指向同一处。

## 逆向选择：通往第 3 章的桥

第 3 章 Glosten–Milgrom 的论证说，价差可以纯粹因信息而存在：若部分对手知道价值，则对交易条件化后盈亏平衡的报价必须跨在价值两侧，于是 $A=E[V\mid\text{买}]\ge E[V]\ge B=E[V\mid\text{卖}]$，且知情到达更频繁或更知情时价差变宽。做市文献以两种方式引入它。其一是定量的：在 Guéant–Lehalle–Fernandez-Tapia 模型里通过价格冲击 $dS_t=\sigma\,dW_t+\xi\,dN^a_t-\xi\,dN^b_t$ 加入逆向选择，会把非对角成交耦合缩放 $e^{-k\xi/2}$、并给价差注入常量 $\xi$，故做市商的报价是基准加一笔逆向选择附加费。其二是程序性的：Lehalle 与 Mounjid 追问*在何处、何时*挂一张限价单以最小化被吃单的风险。订单簿失衡能预测下一次价格移动，代理把预期成交价不是与当前中间价比、而是与**微价格**（给定失衡的长期预期价）比，即
$P_{+\infty}(t)=\lim_{\delta t\to+\infty}\mathbb E(P_{t+\delta t}\mid P_t,\mathrm{Imb}_t)$。
利用失衡的收益会被延迟侵蚀：把较慢的代理用限制其改单频率的延迟因子 $\tau$ 建模，延迟成本 $\mathrm{Latency}_{U_0,f}(\tau)=V_{U_0,f}-V_{U_0,f,\tau}$ 随 $\tau$ 迅速增大，且临近到期时最优做法是更积极地撤销与重挂。价差中的逆向选择成分因此不是常数：它是与更知情订单流成交的预期损失，也正是让延迟值得付费的东西。

## 本章依据的文献

- Avellaneda, M. and Stoikov, S. (2008). *High-frequency trading in a limit order book*. Quantitative Finance 8(3), 217–224.
- Guéant, O., Lehalle, C.-A. and Fernandez-Tapia, J. (2013). *Dealing with the inventory risk: a solution to the market making problem*. Mathematics and Financial Economics 7(4), 477–507.
- Guéant, O. (2017). *Optimal market making*. Applied Mathematical Finance 24(2), 112–154.
- Guilbaud, F. and Pham, H. (2013). *Optimal high frequency trading with limit and market orders*. Quantitative Finance.
- Cont, R., Stoikov, S. and Talreja, R. (2010). *A stochastic model for order book dynamics*. Operations Research 58(3), 549–563.
- Cartea, Á., Jaimungal, S. and Ricci, J. (2014). *Buy low, sell high: a high frequency trading perspective*. SIAM Journal on Financial Mathematics 5(1), 415–444.
- Lehalle, C.-A. and Neuman, E. (2019). *Incorporating signals into optimal trading*. Finance and Stochastics 23(2), 275–311.
- Cartea, Á. and Wang, Y. (2020). *Market making with alpha signals*. International Journal of Theoretical and Applied Finance 23(3), 2050016.
- Lehalle, C.-A. and Mounjid, O. (2016). *Limit order strategic placement with adverse selection risk and the role of latency*.
- Bergault, P., Evangelista, D., Guéant, O. and Vieira, D. (2021). *Closed-form approximations in multi-asset market making*. Applied Mathematical Finance.
- Drissi, F. (2024). *Lecture notes on market microstructure and algorithmic trading*, University of Oxford.

## 本章要点

- 做市问题是以库存、现金、中间价为状态，以两个报价距离为控制，以期末盯市财富的 CARA 或均值–方差效用为目标的随机控制问题。
- Avellaneda–Stoikov 的保留价格 $r(s,q,t)=s-q\gamma\sigma^2(T-t)$ 让报价逆持仓偏移，并随到期临近回落到中间价；其最优价差 $\delta^a+\delta^b=\gamma\sigma^2(T-t)+\tfrac2\gamma\ln(1+\gamma/k)$ 分为风险项与流动性项，且在指数强度下与库存无关。
- Guéant–Lehalle–Fernandez-Tapia 用硬库存上界取代展开，把 HJB 化为线性三对角常微分方程，其谱解给出渐近报价与对库存仿射、敏感度为 $\sqrt{\sigma^2\gamma/(2kA)}$ 的近似。
- Guéant–Royer 推广了强度函数，说明期末 CARA 准则与运行二次库存惩罚属于同一族方程，$\xi$ 区分价格风险与不成交风险厌恶，并把构造扩展到多个相关标的、归结为矩阵 Riccati 型约化。
- 一旦代理也能发市价单，动态规划方程就变成 HJB 拟变分不等式：限价单是正则控制、市价单是脉冲控制，Guilbaud–Pham 的问题因此用一个值函数同时覆盖报价与吃单。
- Cont–Stoikov–Talreja 给出微观基础，把订单簿建成生灭排队网络，由簿状态闭式计算成交概率与价格移动概率。
- Cartea–Jaimungal–Ricci 与 Cartea–Wang 说明 alpha 信号如何进入最优报价：它既修正逆向选择又给报价定向，信号足够大时把做市商变成吃单者。
- 做市与执行是同一个控制问题从相反两侧看的结果；做市商价差中的逆向选择附加费，与第 3 章 Glosten–Milgrom 模型中的信息成本是同一个量。
