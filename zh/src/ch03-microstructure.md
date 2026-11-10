# 市场微观结构：执行成本从何而来

下单这个动作本身就已经付出了代价，与对价值的判断无关。执行成本不是交易附带的收费，而是交易所消耗的服务的价格，加上交易所泄露的信息的价格。产生它的机制有四个：价差、市场冲击、时机风险，以及显性费用。下面的均衡模型会说明，前两者是均衡的产物——取决于还有谁在交易——而不是券商随手设定的常数。Harris 的定义最可操作：**流动性**就是"想交易时就能交易"；Hasbrouck 把它拆成四个属性——**即时性、紧度、深度、弹性**，即速度、往返便宜、报价附近有量、价格效应会消退。Harris 还给出一笔冷峻的记账：交易在扣除成本前是零和博弈，扣除成本后是负和博弈，所以一个分析毫无信息量的管理者，仅仅因为要交易就会平均亏钱。

## 执行成本的四个来源

**买卖价差**。最优卖价高于最优买价，需要即时成交的交易者把差额付给愿意在另一边等待的人。Wyart、Bouchaud、Farmer 与 Lillo（2008）称之为"一股股票瞬时往返的成本"，并沿文献把价差拆成流动性提供者承担的三类成本：订单处理成本、逆向选择成本、库存风险。三者都会撑出价差，但没有一项是手续费。

**市场冲击**。大单吃掉最优价位的挂单后继续沿盘口走，价格随之移动。这部分移动一部分是信息性的、会留在价格里，一部分是流动性被机械地耗尽、随后会复原。这是第三节和第四节的对象，也是机构订单最主要的成本，第 5、6 章的所有模型都是为了控制它。

**时机风险**。订单不可能按决策时的价格成交；从决策到执行完成之间，价格在扩散。Ho 与 Stoll（1981）把它归为做市商承担的两类独立不确定性：持仓的收益不确定性，以及下一笔订单何时到达的交易不确定性——即他不知道自己要把头寸持有多久。同样地，Almgren 与 Chriss 把执行成本的方差写成 $V(x) = \sigma^2 \sum_k \tau x_k^2$：没执行完的部分都暴露在风险里。时机风险是"不交易"的成本，与冲击直接此消彼长。

**显性费用**。交易接入费、返佣、券商佣金、税费、清算费。这是唯一有明码标价的真实转移。在上面那些模型里它就是一个加在冲击项上的常数——Almgren 与 Chriss 把它写成 $h(n_k/\tau) = \epsilon\,\mathrm{sgn}(n_k) + (\eta/\tau)n_k$ 中的 $\epsilon$——对大单来说也是最小的一项。

对执行而言真正重要的不是这份清单，而是**钱付给了谁**：价差与冲击的机械部分付给流动性提供者；冲击的永久部分付给掌握更多信息的人；时机风险谁也不付，是纯粹的损失。

Harris 按动机而非信息给参与者分类。**功利交易者**为交易之外的目的而交易——把现金在时间上挪动、转移风险、兑换货币——并愿意为此承担交易成本；这份支付意愿正是所有利润动机交易者收入的最终来源。利润动机一方再分：知情者（价值交易者与新闻交易者）依据基本面价值交易，寄生交易者依据对他人订单的预测交易，而**流动性提供者**——做市商连接不同时刻到达的买卖双方，套利者跨市场连接同时到达的双方。一笔订单是吃单还是挂单，是一个取舍，即**挂单—吃单选择**：吃单付出价差但确定成交，挂单更便宜却可能永不成交。撮合先按价格、再按时间排序，显示订单优先于隐藏订单，而 Harris 提出的三类速度需求——**吃单、挂单、撤单**——使流动性供给变成一场只有相对速度才有价值的竞赛。

## 库存模型：做市商的定价问题

Ho 与 Stoll（1981）研究一个风险厌恶的做市商，他报出买入费 $b$ 与卖出费 $a$（即在其自身估值 $p$ 之下报买价 $p-b$、卖价 $p+a$），目标是最大化期末财富的期望效用 $E[U(W_T)]$。订单按泊松过程到达，强度随报价变宽而下降，因此报价越宽成交越少。问题是一个动态规划，其递推式是一个带两个跳变项的 Bellman 方程——一笔按强度 $\lambda_a$ 发生的卖出，一笔按强度 $\lambda_b$ 发生的买入：

$$\max_{a,b}\ \Big\{\, \lambda_a\big(J(F+pQ+aQ,\ I-pQ,\ Y) - J(F,I,Y)\big) + \lambda_b\big(J(F-pQ+bQ,\ I+pQ,\ Y)-J(F,I,Y)\big)\Big\} = 0 .$$

论文用价差 $s=a+b$ 与偏移量 $d=a-b$ 描述策略，两个结构性结论是：价差基本与库存无关；库存移动的是**价差中点**——取 $d\ne0$ 使两个报价同向移动，持多头时买价与卖价一起下调。价差本身写成风险中性的收益最大化价差 $\rho$（对即时性的需求越缺乏弹性，$\rho$ 越大）加上一个卖出风险溢价和一个买入风险溢价。一阶风险调整项与库存无关，只随三样东西增大：绝对风险厌恶系数 $-U''/U'$、交易规模 $Q$、股票的瞬时方差；而且是**总方差**而非系统性方差，因为做市商在报价这一笔上没有分散化。这就是价差的库存解释：即时性的价格，是替客户持有他不愿持有的风险所得到的补偿。

*(Ho–Stoll 的式 19、24、25 是多行展开公式，扫描件的 OCR 未能保真，故上文用文字描述而非逐字转录。)*

Roll（1984）把这个机制变成一种测量。若基础价格是随机游走、成交价在买卖价之间跳动，则相邻价格变动的一阶序列协方差恰为负的四分之一价差平方，方差为 $s^2/2$、自相关为 $-1/2$：

$$\operatorname{Cov}(\Delta p_t, \Delta p_{t-1}) = -\tfrac14 s^2, \qquad s = 2\sqrt{-\operatorname{Cov}(\Delta p_t,\Delta p_{t-1})}.$$

用百分比收益率并乘 200 换算成百分点，得 $s_{jt} = 200\sqrt{-\hat c_{jt}}$。在 1963–1982 年 CRSP 数据上，全样本隐含价差日均约 0.298%，周均约 1.74%，且与公司规模显著负相关。这个估计量是所有"从成交记录反推执行成本"方法的源头；它的失效情形——协方差为正、估计量无定义——恰好就是 Wyart 等人看到的纽交所专营商租金。

Hasbrouck 的成本框架进一步从执行数据里反推出另外两个价差。设 $m$ 为成交时的报价中间价、$m_5$ 为五分钟后的中间价，则可成交买单的**有效成本**为 $p-m$、**实现成本**为 $p-m_5$；有效价差是前者的两倍，若成交价优于近侧报价，差额即**价格改善**。两者之差度量成交后中间价移动了多远，是对这笔订单价格冲击的近似（之所以只是近似，是因为这段移动由区间内所有成交共同造成），并因此把流动性提供者的报酬（实现价差）与知情订单的信息分离开。它就是下文"永久 vs 暂时"的经验形式；来源本身还记下两点保留。O'Hara（2015）指出这一分解依赖时间尺度：在高速电子市场里实现价差经常为**负**，五分钟是一段"一生"；而"永久/暂时"本身也可疑，因为界定"暂时"的时间跨度并无定义。Harris 补充说，在 maker-taker 定价下报价差已不再是成交成本：吃单方的真实价差是报价差加两倍接入费，挂单方则是报价差减两倍返佣。

Amihud 与 Mendelson（1986）把价差放进均衡资产定价：当投资者持有期限不同时，市场观测到的期望收益率应当是相对价差的递增凹函数。流动性因此不只是交易成本，它本身被定价，持有期长的投资者能承担更多。*(该文付费未能获取，仅依据其公开摘要。)*

## 信息不对称：价差作为贝叶斯推断

Ho–Stoll 的做市商平均不亏给任何人。Glosten 与 Milgrom（1985）证明，即使专营商风险中性、面临竞争、被迫赚取恰好为零的期望利润，价差依然存在。原因在于：接受卖方报价的那位客户，可能正是因为**知道些什么**才来成交。专营商分不清知情者与流动性交易者，只能按"给定成交确实发生"的条件设定盈亏平衡的价格：

$$A_t = E[V \mid S_t,\ \text{客户买入}], \qquad B_t = E[V \mid S_t,\ \text{客户卖出}].$$

**命题 1** 由此给出 $A \ge E[V] \ge B$，只要逆向选择可能发生就严格成立。证明是一个条件期望论证——对买入事件 $C$ 有 $A = E[V\mid C]$，而 $E[V\mid F]$ 比 $E[V\mid F,C]$ 更粗糙——其含义是：价差是一笔纯信息租金，专营商不是在为服务收费，而是在提防一个无法分类的对手。

让模型真正可用的是另外三个结果。**命题 2**：成交价 $P_k = E[V\mid S_k]$ 是鞅，所以即使存在价差，价格仍然是半强式有效的。**命题 3**：若买入与卖出的到达概率都远离零，则前 $k$ 笔交易的平均均方根价差以 $1/\sqrt{k}$ 的速度消失，

$$\sqrt{\tfrac1k E[(A_k-B_k)^2]} \le \sqrt{\operatorname{Var}(V)/(\alpha\beta k)} .$$

**命题 5**：知情者信息更精细、知情与不知情到达比上升、或流动性动机的离散度下降时，价差变宽。最后这一条就是盘口版的 Akerlof 柠檬问题：把流动性交易者挤出太多，市场可能彻底关闭。

Glosten–Milgrom 把交易强度当作外生。Kyle（1985）把它变成策略性的。一个风险中性的内幕交易者观测到清算价值 $v \sim N(\mu_0,\Sigma_0)$；噪声交易者提交与 $v$ 独立的 $u \sim N(0,\sigma_u^2)$；竞争的、风险中性的做市商只能看到总订单流 $x+u$，并令 $p = E[v \mid x+u]$。内幕者知道自己的下单会推动价格，据此最大化期望利润。单次拍卖的均衡中双方策略都是线性的（定理 1）：

$$X(v) = \beta(v-\mu_0), \qquad P(x+u) = \mu_0 + \lambda(x+u),$$

$$\beta = \Big(\frac{\sigma_u^2}{\Sigma_0}\Big)^{1/2}, \qquad \lambda = \frac12\Big(\frac{\Sigma_0}{\sigma_u^2}\Big)^{1/2},$$

由内幕者一阶条件 $1/\beta = 2\lambda$ 与投影定理 $\lambda = \beta\Sigma_0/(\beta^2\Sigma_0+\sigma_u^2)$ 得到。$\lambda$ 即 **Kyle's lambda**：价格对订单流的斜率；倒数 $1/\lambda$ 是市场**深度**，即推动价格变动一单位所需的订单流，它正比于噪声交易量与尚未消化的私有信息之比。一轮拍卖之后只有一半私有信息进入价格，$\Sigma_1 = \tfrac12\Sigma_0$；内幕者的最大化利润为 $\Sigma_0/(4\lambda) = \tfrac12\sqrt{\Sigma_0\sigma_u^2}$。

序贯版本（定理 2）把这同一件事逐笔写出，$p_n - p_{n-1} = \lambda_n(\Delta x_n + \Delta u_n)$；连续极限（定理 3）收缩成一个值得记住的常数：

$$\lambda(t) = \Big(\frac{\Sigma_0}{\sigma_u^2}\Big)^{1/2}, \qquad \Sigma(t) = (1-t)\,\Sigma_0 .$$

深度恒定，价格是常数波动率的布朗运动，收盘时私有信息全部进入价格。Kyle 的 lambda 就是经典线性冲击系数，也正是第 5 章里永久冲击项 $g(\cdot)$ 取那个形式的理由。

## 永久冲击与暂时冲击

上面两种机制给出两种不同的冲击，把它们分开是后续所有执行模型的地基。泄露了信息的成交把价格推到一个新的水平，这个水平是真实的：信息已经公开，收不回来。耗尽流动性的成交之所以推动价格，只是因为盘口太薄，而盘口会重新填满。前者是**永久冲击**——一次公共信息事件，等于 $\lambda$ 乘以净订单流。后者是**暂时冲击**——付给流动性提供者的一笔租金，会衰减。

Guéant 的讲义把这一划分写成一般形式。设 $v_t$ 为交易速率、$V_t$ 为市场成交量，则**参与率**为 $\rho = v_t/V_t$。中间价满足

$$dS_t = \sigma\,dW_t - k\,v_t\,dt,$$

其中 $k$ 项即永久冲击；执行所付现金为 $V_t\,L(v_t/V_t)\,dt$，$L$ 称为**瞬时冲击函数**：严格凸、偶函数、在 $\mathbb R_+$ 上递增、$L(0)=0$、渐近超线性，实践中取 $L(\rho) = \eta|\rho|^{1+\varphi} + \psi|\rho|$。清算 $q_0$ 股的期末财富为

$$X_T = X_0 + q_0 S_0 - \tfrac{k}{2}q_0^2 + \sigma\int_0^T q_t\,dW_t - \int_0^T V_t\,L(v_t/V_t)\,dt .$$

要紧的一点是：永久冲击成本 $\tfrac{k}{2}q_0^2$ 与策略无关——无论慢慢做还是一笔打完都一样——所以 $k$ 只是换元，模型的自由度落在 $L$ 的形状与风险项上。这就是"只有暂时部分能靠放慢速度优化掉"的形式化版本。

Bouchaud（2010）把这个经验论点说得很锋利。如果冲击是永久且线性的，价格就会继承成交符号的自相关；而 $\epsilon_n$ 衰减得极慢——至少持续数天、数千笔交易——价格就会呈现趋势，与它近似鞅的行为矛盾。因此冲击必须是暂时的，由一个衰减核 $G(\ell)$ 承载：

$$p_T = p_{-\infty} + \lambda\sum_{n=-\infty}^{N-1} G(N-n)\,\epsilon_n\, v_n^{\psi}.$$

在成交量固定的情形下，若符号相关 $C(\ell)\sim \ell^{-\gamma}$（股票通常 $\gamma\approx0.5$），则要让价格变动保持白噪声，$G(\ell)$ 必须按 $\ell^{-\beta}$ 衰减，其中 $\beta = (1-\gamma)/2$。Gatheral 后来给出的条件 $\beta+\psi\ge1$ 正是该框架下排除价格操纵策略的界线。

那么冲击随规模呈什么形状？答案是**平方根律**。Tóth、Lempérière、Deremble、de Lataillade、Kockelkoren 与 Bouchaud（2011）把它写成：规模为 $Q$ 的母单，其首末成交之间的平均相对价格变动为

$$\Delta(Q) = Y\sigma\sqrt{\frac{Q}{V}},$$

其中 $\sigma$ 为日波动率、$V$ 为日成交量、$Y = O(1)$；更一般地 $\Delta(Q)\propto Q^\delta$，$\delta$ 落在 0.4–0.7 区间。他们对数据的解读是：Almgren 等人报告 $\delta\approx0.6$，Moro 等人在马德里报告 $\delta\approx0.5$、在伦敦报告 $0.7$；这条规律在股票、期货、外汇之间以及不同年代之间都近似成立。其机制是一个潜在订单簿，供需剖面呈 V 形并在现价附近消失——市场处于临界状态，线性响应失效，平方根由此出现。

但这并非定论。Zarinelli、Treccani、Farmer 与 Lillo（2015）使用 2007–2009 年美国股市数百万笔母单发现：平方根形式只在约两个数量级的规模范围内拟合良好，最优参数 $Y = 0.15 \pm 0.01$、$\delta = 0.47 \pm 0.01$，加权均方根误差 6.70；而对数形式 $a\log_{10}(1+b\pi)$（$\pi$ 为日参与率）能在近五个数量级上拟合，$\hat a = 0.028 \pm 0.001$、$\hat b = 465 \pm 33$，误差 2.80——"显著更好"。他们还引入由母单持续时间与参与率张成的冲击**曲面**，并发现冲击平均会回复到峰值的约**三分之二**。Cont、Kukanov 与 Stoikov（2014）提供了互补的分解：在短时间尺度上，价格变动主要由最优价位的订单流**失衡**驱动，斜率与市场深度成反比，再用一个标度论证把这一线性关系转成观测到的平方根。

只有"暂时"这一图景能同时解释：订单流是持续的，而冲击不是永久的。Bouchaud、Farmer 与 Lillo（2009）综述了证据：订单符号的自相关是长记忆过程，按幂律衰减且指数小于一；可见流动性极小（最优价位上的量比相邻一档的量小约 $(w/u^*)$ 倍，比日成交额小约一千倍），所以大单必须拆成数小时、数天乃至数周执行。他们的总量冲击计算给出

$$R(Q,N) = N\,\frac{s\Gamma_0}{I(1-\beta)}\left(\frac{Q}{N^{1-\gamma/2}}\right),$$

母单较小时冲击强烈凹、规模大时转为线性；价差本身则以 0.4–0.5 的幂律指数回复。这就是市场的"弹性"（resilience），也是暂时冲击可以复原的原因。

## 市场微观结构不变性

跨资产比较执行成本本来无从下手：USD 5 的股票和 USD 500 的股票，一股并不代表同样多的风险。Kyle 与 Obizhaeva（2016）主张，一旦改用**业务时间**——以"下注"（bet，指一个组合决策，而非一笔订单或一笔成交）的到达来度量的时间——比较就变得简单。他们的两条假设是：**下注不变性**（一笔下注在单位业务时间内转移的美元风险，其分布跨资产、跨时间相同）与**交易成本不变性**（执行一笔下注的美元成本，是其以该单位度量的规模的同一个函数）。记下注活跃度 $\bar W_{jt} = \sigma_{jt}P_{jt}\bar V_{jt}$、下注规模 $\tilde I_{jt} = P_{jt}\tilde Q_{jt}\sigma_{jt}\gamma_{jt}^{-1/2}$，下注不变性给出 $\bar W_{jt} = \gamma_{jt}^{3/2}E|\tilde I|$，于是

$$\gamma_{jt} = \bar W_{jt}^{2/3}\,\iota^2, \qquad E|\tilde Q_{jt}| = \bar W_{jt}^{1/3}\frac{1}{P_{jt}\bar\sigma_{jt}}\iota^{-2}.$$

下注笔数按活跃度的 2/3 次幂、平均下注规模按 1/3 次幂缩放。交易成本不变性给出一个非流动性指数，即收益率方差与美元成交额之比的立方根，

$$\frac{1}{L_{jt}} = \iota^2\bar C_B\Big(\frac{\sigma_{jt}^2}{P_{jt}V_{jt}}\Big)^{1/3},$$

以及百分比价差正比于业务时间里的波动率：$s_{jt}/P_{jt} = \kappa_0\sigma_{jt}\bar W_{jt}^{-1/3}$——波动率不变时，活跃度提高八倍，百分比价差减半。而"平方根"是唯一能让不变性导出"冲击只依赖于下注规模相对下注量之比、不依赖其他资产特征"的冲击函数：$\bar C_{jt}(Q) = \bar\sigma_{jt}\kappa_I[|Q|/\bar V_{jt}]^{1/2}$。Kyle 与 Obizhaeva 用 40 万余笔组合转换订单检验：下注规模与第一条假设相符（对数方差 2.53），交易成本在经济意义上也接近第二条，尽管统计检验通常会正式拒绝它。

## 订单流作为信号

以上所有内容从不同角度说的是同一件事：订单流携带信息，这才是它要收费的原因。在 Glosten–Milgrom 中，价差**就是**逆向选择租金；在 Kyle 中，价格变动**就是**市场的贝叶斯更新。这笔租金的大小可以测量。Wyart 等人（2008）证明：若要求做市与吃单两种策略最多只能获得边际利润，就会导出价差与市价单瞬时冲击之间的线性关系；进而导出价差正比于**每笔交易**的波动率——在巴黎交易所的股票上实证 $R^2$ 超过 0.9。他们的结论是：价差的主要决定因素是逆向选择，而绝大部分波动率来自交易冲击。对执行的直接含义是：订单流可被预测的策略，付出的代价低于订单流是意外的策略，因为只有意外才会推动价格。

Hasbrouck 把买卖报价与成交共同收敛到新价值的过程称为**价格发现**；"发现"一词正强调结果是未知的——大家可能都同意消息是利好，却没人知道它值多少，市场只能聚合彼此不同的信念。使这一聚合代价高昂的，是订单冲击相对订单本身的量级：在一家有一千万股的公司的股票上，一笔 1000 股的市价买单可能把价格推高一分钱，从而把公司市值重估到这笔订单价值的约五倍。盘口之所以不会在原价被重新填满，是因为成交这一事实本身、连同它的方向，已经进入公共信息集。O'Hara（2015）把这一点对准电子市场：那里市场信息的基本单位不是成交，而是底层订单。算法把一张母单拆成几十张子单，于是一张母买单在成交记录上表现为许多笔小卖单；而速度本身也变成一种信息——最先看到并对价格作出反应的人，短暂地就是知情者。

这也正是信息优势者的成本结构与他人不同的原因。对不知情的执行算法来说，自身的冲击是付给流动性提供者的成本，要尽量压低。对内幕交易者来说，冲击的永久部分根本不是成本，而是价格正朝着他要买的价值移动，并且在他累积的头寸上兑现。两人面对同一个 lambda，却应当有不同行为。第 8 章讲信息泄露时会回到这一点，而第 2 章的做市商站在它的另一侧。

## 成本地图

把各机制合起来读，就得到一张简明的"谁付钱、付给谁"账本。

| 机制 | 付钱方 | 收钱方 | 是否复原 |
|---|---|---|---|
| 价差 | 需要即时性的一方 | 提供即时性的一方（处理、逆向选择、库存） | 入场即付，出场只在报价中收回 |
| 暂时冲击 | 主动吃单方 | 吸收订单的流动性提供者 | 会——按 $G(\ell)$ 衰减，全部或部分 |
| 永久冲击 | 主动吃单方 | 持有反向头寸的人——知情者 | 不会——信息已进入价格 |
| 时机风险 | 未执行的剩余部分 | 无人（纯风险，非转移） | 不会 |
| 显性费用 | 交易者 | 交易所、券商、政府 | 不会 |

其中重塑了整个执行理论的是"永久 vs 暂时"这一行。两笔在事后成本报告里长得一模一样，都是冲击。但暂时的那部分，是靠安排交易节奏去谈判的租金；永久的那部分，是市场在发现这笔订单意味着什么。只有前者能靠放慢速度被优化掉。

## 本章依据的文献

- Ho, T. and Stoll, H. (1981). *Optimal Dealer Pricing under Transactions and Return Uncertainty*, Journal of Financial Economics 9(1), 47–73. [Wharton working paper](https://rodneywhitecenter.wharton.upenn.edu/wp-content/uploads/2014/03/7927.pdf)
- Roll, R. (1984). *A Simple Implicit Measure of the Effective Bid-Ask Spread in an Efficient Market*, Journal of Finance 39(4), 1127–1139. [pdf](https://www.bauer.uh.edu/rsusmel/phd/roll1984.pdf)
- Glosten, L. and Milgrom, P. (1985). *Bid, Ask and Transaction Prices in a Specialist Market with Heterogeneously Informed Traders*, Journal of Financial Economics 14(1), 71–100（工作论文版为 Northwestern CMSEMS Discussion Paper 570, 1983）. [pdf](https://www.econstor.eu/bitstream/10419/220930/1/cmsems-dp0570.pdf)
- Kyle, A. S. (1985). *Continuous Auctions and Insider Trading*, Econometrica 53(6), 1315–1336. [pdf](https://people.duke.edu/~qc2/BA532/1985%20EMA%20Kyle.pdf)
- Amihud, Y. and Mendelson, H. (1986). *Asset Pricing and the Bid-Ask Spread*, Journal of Financial Economics 17(2), 223–249（未获取全文）.
- Madhavan, A. (2000). *Market Microstructure: A Survey*, Journal of Financial Markets 3(3), 205–258.
- Wyart, M., Bouchaud, J.-P., Farmer, J. D. and Lillo, F. (2008). *Relation between Bid-Ask Spread, Impact and Volatility in Order-Driven Markets*.
- Bouchaud, J.-P., Farmer, J. D. and Lillo, F. (2009). *How Markets Slowly Digest Changes in Supply and Demand*. [arXiv:0809.0822](https://arxiv.org/abs/0809.0822)
- Bouchaud, J.-P. (2010). *Price Impact*, Encyclopedia of Quantitative Finance (Wiley). [arXiv:0903.2428](https://arxiv.org/abs/0903.2428)
- Tóth, B., Lempérière, Y., Deremble, C., de Lataillade, J., Kockelkoren, J. and Bouchaud, J.-P. (2011). *Anomalous Price Impact and the Critical Nature of Liquidity in Financial Markets*. [arXiv:1105.1694](https://arxiv.org/abs/1105.1694)
- Cont, R., Kukanov, A. and Stoikov, S. (2014). *The Price Impact of Order Book Events*. [arXiv:1011.6402](https://arxiv.org/abs/1011.6402)
- Zarinelli, E., Treccani, M., Farmer, J. D. and Lillo, F. (2015). *Beyond the Square Root: Evidence for Logarithmic Dependence of Market Impact on Size and Execution Kinetics*. [arXiv:1412.2152](https://arxiv.org/abs/1412.2152)
- Kyle, A. S. and Obizhaeva, A. A. (2016). *Market Microstructure Invariance: Empirical Hypotheses*, Econometrica 84(4), 1345–1404.
- Hasbrouck, J. (2025). *Securities Trading: Principles and Procedures*, v14c，纽约大学 Stern 商学院教学书稿（其《Empirical Market Microstructure》的接续之作）。[作者主页](https://pages.stern.nyu.edu/~jhasbrou/)
- O'Hara, M. (2015). *High Frequency Market Microstructure*, Journal of Financial Economics 116(2), 257–270.
- Harris, L. (2015). *Trading and Electronic Markets: What Investment Professionals Need to Know*, CFA Institute Research Foundation。[pdf](https://www.cfainstitute.org/-/media/documents/book/rf-publication/2015/rf-v2015-n4-1-pdf.pdf)
- Guéant, O. (2016). *The Mathematics of Optimal Execution*, CFM–Imperial Distinguished Lectures。[pdf](https://www.ogueant.com/doc/imperial_slides.pdf)

关于未获取的来源：O'Hara 的 *Market Microstructure Theory*（1995）、Hasbrouck 的 *Empirical Market Microstructure*（2007）、Harris 的 *Trading and Exchanges*（2003）三本书的正文，以及 Guéant 的 *The Financial Mathematics of Market Liquidity*（2016）印刷版正文，均未获取，本章没有任何陈述取自这些正文。本章依据的是作者本人与权利方自行发布的材料：Hasbrouck 的纽约大学 Stern 教学书稿 *Securities Trading: Principles and Procedures*（v14c, 2025，其 2007 年 OUP 一书的接续之作）提供流动性的四个属性、订单簿优先级规则、成本分解与私有信息分析；O'Hara 本人 2015 年的综述提供价差分解在高频环境下的重新评估，以及"信息单位是订单而非成交"的论点；Harris 的 *Trading and Electronic Markets*（CFA Institute Research Foundation, 2015，由该书作者本人免费发布的完整体量）提供交易者的动机分类、零和记账、三类速度需求以及 maker-taker 对报价差的修正；Guéant 的 CFM–Imperial 讲义（2016）覆盖其书第一、二部分的同一批最优执行内容，提供永久/暂时冲击的一般框架。仍然缺失的部分是：O'Hara 的九章正文、Hasbrouck 2007 年书中的计量章节 3–13、Harris 2003 年书的第 3–29 章，以及 Guéant 的印刷版正文。

## 本章要点

- 执行成本有四个来源——价差、市场冲击、时机风险、显性费用；只有最后一项是明码标价的转移，前三项都是取决于"还有谁在交易"的均衡对象。
- Ho–Stoll 说明库存风险如何进入做市商报价：价差基本与库存无关，可拆为风险中性收益最大化项加风险溢价（随风险厌恶、成交规模、总方差上升），而库存只移动价差中点。
- Roll 的估计量 $s = 2\sqrt{-\operatorname{Cov}(\Delta p_t,\Delta p_{t-1})}$ 仅凭价格序列就能反推有效价差，因为交易成本会造成负的序列相关；1963–1982 年 CRSP 日均约 0.298%，且随公司规模下降。
- Glosten–Milgrom 证明单是逆向选择就能撑出价差：零利润报价 $A = E[V\mid\text{买入}]$、$B = E[V\mid\text{卖出}]$ 满足 $A \ge E[V] \ge B$，价格仍是鞅，且价差在前 $k$ 笔交易中以 $1/\sqrt{k}$ 消失。
- Kyle 的 lambda 在单次拍卖中为 $\lambda = \frac12(\Sigma_0/\sigma_u^2)^{1/2}$、连续极限中为 $(\Sigma_0/\sigma_u^2)^{1/2}$，是价格对订单流的斜率；其倒数为市场深度，也是永久冲击的均衡微观基础。
- 永久冲击与暂时冲击必须分开：前者是信息释放、永不回收，后者是付给流动性提供者的租金、会衰减；冲击形状为 $\Delta \propto \sigma\sqrt{Q/V}$（平方根律），$\delta$ 在 0.4–0.7 之间，而 Zarinelli 等人发现对数形式比平方根拟合其数据更好。
- Kyle–Obizhaeva 的不变性指出：按单位业务时间度量，跨资产的下注规模与交易成本是常量，给出 $\gamma \propto \bar W^{2/3}$ 与非流动性指数 $\propto (\sigma^2/PV)^{1/3}$，这是跨市场比较的工具。
- 订单流是信号：价差是付给信息持有者的逆向选择租金，因此可预测的订单比意外的订单更便宜；而对知情者来说，永久冲击不是成本，而是价格正朝价值移动。
