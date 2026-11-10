# 加密货币市场的执行

第 3 章的微观结构假设只有一个合并订单簿、有开盘与收盘的交易时段、以及一个小的常数费率。这些前提在加密货币里全部失效。同一资产同时挂在几十家彼此独立的中心化交易所（CEX）和链上资金池上交易；市场没有开盘也没有收盘；一笔交易的成本是一个按比例收取的场所费，外加一笔按笔固定收取的 gas；而承载大部分成交量的场所是没有到期日的永续合约，其价格靠一笔叫资金费率的现金流钉在现货上。本章说明这些差异对执行问题意味着什么，并把文献真正测到的东西与只能算判断的东西分开。

## 关键的制度差异

**7×24 连续交易、无熔断。** 加密 CEX 全天候运行，没有集合竞价，也没有交易所层面的停牌，因此既没有收盘价可以作 VWAP 调度的基准，也没有统一的开盘价可以锚定到达价；到达价只能相对某一选定的参考场所来定义，而不是相对行情带。

**场所碎片化且无统一行情带。** 同一资产在互不共享订单簿的多家交易所报价，价差、深度和费率档位相差若干数量级，于是路由（第 9 章）成为核心问题，而不是一个细节。

**分层 maker–taker 费率。** CEX 费率是成交额的一个比例，并按过去 30 天的成交量分档。以撰写时的 Binance 现货费率为例：普通用户（30 天成交量低于 USD 1,000,000）为 maker 0.1000%、taker 0.1000%；到 VIP 9（30 天成交量不低于 USD 4,000,000,000）降为 maker 0.0110%、taker 0.0230%；若用交易所自有代币支付费用，还可再减 25%。每一档的 maker 费率都低于 taker，做市商还可协商到零。因为档位取决于已经发生的成交量，边际费率是依赖历史的：在零售端，同样的名义金额"吃单"的成本约为"挂单"的十倍。

**资金费率。** 永续合约没有到期日，交易所因此周期性向多空双方支付现金流，把合约价格维持在现货附近；Binance 的结算周期是 8 小时，费率是期货对现货溢价的函数。

**提现费用与结算延迟。** 把资产提离交易所要付一笔按币种计的费用，早期调研中为 10 到 50 个基点；转账只有在区块链确认后才完成，跨所套利者因而要承担确认窗口内的价格漂移。

**永续合约与 AMM 并存。** 大部分成交量在永续合约而非现货上；同时存在一套去中心化场所——恒定函数做市商（CFMM）——它按一条确定性曲线而非订单簿定价，其执行成本结构与前者本质不同。

## 碎片化与套利

Makarov 与 Schoar（2020）是跨场所错价的基准测量。他们用约 80 家交易所 2017—2018 年的逐笔数据，发现单日潜在套利利润常超过 USD 75 million，2017 年 12 月至 2018 年 2 月合计至少 USD 2 billion。价差又大又带地域性：美韩价格比平均超过 15%，有几天达到 40%（即"泡菜溢价"），日美平均约 10%，美欧约 3%，而同一国家内交易所之间的偏离通常低于 1%。单因子分解显示，约 80% 的比特币收益来自一个共同成分，跨所价差则来自各场所的异质成分。可能弥合价差的成本相比之下都很小：买卖价差平均 1 到 10 个基点；交易所费率为成交额的 0.25% 至 0.1%，按成交量分档，多数场所对 maker 收费为零；提现费 10 到 50 个基点——因此作者认为大资金往返总成本在 50 到 75 个基点，远低于价差。把共同收益成分对净买量回归，得到系数 $8.8\times10^{-6}$，$t=80$、$R^2=54\%$，作者把它换算为"比特币价格为 USD 10,000 时，一笔 USD 1 million 交易的价格冲击为 9 个基点"。

Hautsch、Scheuch 与 Voigt（2018/2023）给出了价差得以持续存在的机制：结算延迟。区块链用共识协议取代中央对手方，跨所转账要花掉若干个区块的时间，其间套利者暴露于价格风险。他们的模型给出一个随延迟和波动率放大的"无交易带"，并用比特币网络与订单簿数据证明：大的跨所价差与高结算延迟同期出现；资产流追逐套利机会；违约风险低的交易所之间价差更小。改在目标场所持有库存可以消除延迟，但换来了交易所违约风险。因此跨所路由本质上是一个关于结算时间的控制问题，而不只是费率比较。

Barbon 与 Ranaldo（2021/2024）把两层成本栈形式化。CEX 的有效成本是报价半价差加交易所费；DEX 的有效成本是报价半价差加协议费加 gas。因为 gas 是按笔固定的，它在总成本中的占比随名义金额上升而下降。他们的样本覆盖 CEX 成交量的 70% 和 DEX 成交量的约 60%（2021 年 3 月至 2023 年 2 月），结论是一个规模阈值：小额与中等金额在 CEX 交易更便宜，DEX 在约 USD 10,000 以上才变得划算。DEX 相对无套利条件的偏离比 CEX 大一个数量级；他们借 2021 年 5 月 Uniswap v3 上线与 gas 价格变动做因果识别，估计 gas 价格上升一个标准差，会使价格偏离幅度增加 18% 至 25%。

Wu、Sui、Thiery 与 Pai（2025）测量了 CEX–DEX 套利实际提取到的价值。根据链上 swap，他们估计 19 个主要套利者在 7,203,560 笔交易中合计提取 USD 233.8 million，其中前三名拿走约四分之三的成交量与价值。盈利能力取决于与区块构建者的整合程度：把流量分散给多个构建者的"中性"套利者利润率更高，而那些只把流量送给关联构建者的"专属"套利者要把大部分价值分出去，"有时甚至以净亏损运营"。因此路由本身——走哪个构建者或套利者通道、公开 mempool 还是私有通道——就是一个执行决策，要与 gas 和打包概率一起定价。

## 加密市场中的价格冲击

Donier 与 Bonart（2014/2015）重建了 MtGox 比特币/美元市场上超过一百万笔元订单。成交量为 $Q$ 的元订单、对执行速率 $\mu$ 取平均后的峰值冲击满足

$$\langle I(Q,\mu)\rangle_\mu \approx Y_e\, Q^{\delta},$$

其中 $\delta\approx0.5$、$Y_e\approx4.5\times10^{-2}$，从最小尺度起跨越四个数量级都成立。用平均波动率与日成交量归一后得到 Y-ratio 约 0.9，接近成熟股票与期货市场上的取值。该规律不只在最终价格上成立，在元订单执行过程中也成立；且流量中的非知情成分会造成冲击几乎完全地长期衰减。MtGox 每笔交易收费 0.6%，约 60 个基点的价差常常超过典型元订单的峰值冲击——在这个场所，成本是被费率主导的，与股票市场的直觉相反。

加密市场的冲击规律与第 3 章是**同一条**平方根律，而不是一个变体。指数 $\delta\approx1/2$，归一系数 $Y\approx0.9$ 落在股票与期货的区间内，尽管该场所几乎没有高频交易与统计套利。这是对平方根律普适性的强样本外支持；不同的只是它外面的成本栈——小额交易被固定费率和宽价差主导。Cho、Norman 与 Nunes（2023）用传播子（propagator）刻画了瞬时那一侧，$M(t)=\int_0^t f(\dot x_s)\,G(t-s)\,ds+\int\sigma\,dZ_s$，其衰减核表现为初期冲击很大、随后以幂律衰减地回归。

Barone 与 Lillo（2026）在 Hyperliquid——一个全链上、且原生 TWAP 订单自始至终公开可查的永续合约限价订单簿——上检验了透明度的取舍。他们从地址级数据重建出约 430 万笔隐藏元订单和约 46.5 万笔公开 TWAP 执行。隐藏元订单呈现前重后轻的 U 形调度，与瞬时冲击最优执行一致；原生 TWAP 则几乎均匀成交。在中位波动率下，公开 TWAP 的临时冲击比隐藏元订单低约 9 个基点，执行结束后留下的价格位移少约 5 个基点；而那些顺着已经可见的同向 TWAP 流量交易的隐藏元订单，承受更高的位移，同向可见 TWAP 占比每增加 10 个百分点，位移增加约 0.8 至 0.9 个基点。预先公开大额订单降低了自己的成本，却把冲击转移给了未公开者。

## 永续合约与资金费率

永续合约承载了加密市场的大部分流动性，其资金费率机制改变了持仓成本，从而改变了最优执行的时间窗。He、Manela、Ross 与 von Wachter（2022/2026）给出了 Binance 使用的锚定规则：

$$\Phi\!\left(\frac{F-S}{F}\right) = \kappa\,\frac{F-S}{F} + \operatorname{clamp}\!\left(\iota - \kappa\,\frac{F-S}{F},\; -\gamma,\; \gamma\right), \qquad \operatorname{clamp}(x,l,h)=\min(\max(x,l),h),$$

当前 Binance 参数为 $\kappa=1$、$\gamma=0.05\%$、$\iota=0.01\%$。当期货与现货接近时，费率被钉在很小的利率成分 $\iota$ 附近；两者背离时，费率随溢价线性增长。无套利价格是"随机到期"价格——现货在随机时刻的风险中性期望，对应永续合约随机的有效到期时间。

这篇论文也是把**完整费率表**纳入执行模型的最清楚例子。套利者面对一个往返成本，无套利带随成本上升而变宽。成本取自 Binance 官方费率表，按 30 天成交量分档："高"（现货高于 USD 1 million、期货高于 USD 15 million，零售个人）、"中"（现货高于 USD 150 million、期货高于 USD 1 billion，小型基金）、"低"（现货高于 USD 2 billion、期货高于 USD 12.5 billion，大型基金），做市商可协商到零费率；因为机构通常下限价单，用的是 maker 而非 taker 费率。高档的单边 maker 费率为现货 0.0675%、期货 0.0144%，低档为 0.0225% 与 0.0018%，对应的年化无套利带分别为 $\pm179.4\%$ 与 $\pm53.2\%$。随机到期套利策略在零售成本下 Sharpe 比率为 3.35，对免费率做市商最高可达 11.65；计入有效买卖价差后降为 3.27 与 10.46——作者估计这些间接成本低于 0.35 个基点，远低于直接费率。偏离很大：摘要报告各币种年化平均绝对偏离约 52% 至 90%，正文为 60% 至 90%，2022 年后缩小近 80%。

Ackerer、Hugonnier 与 Jermann（2023/2024）把定价做严格。当资金支付与期货—现货价差线性相关时，$f_t=\mathbb{E}^{\mathbb{Q}}_t[x_\theta]$，其中 $\theta$ 服从均值为 $1/\kappa$ 的指数分布，$\kappa$ 是锚定强度；当 $\kappa\to\infty$ 时期货与现货价格重合。可见基差不是纯粹信号，而是资金机制的人为产物。Kim 与 Park（2025）说明机制设计本身是可选的变量：他们用路径依赖的无限期倒向随机微分方程，推导出让永续价格锚定目标的资金费率规则，给出发行方的复制组合，并刻画了路径依赖规则与标准马尔可夫规则的差异——因此可执行的基差信号取决于场所选择的规则，而不是一个普适常数。

Donnelly、Lin 与 Lorig（2026）把 Almgren–Chriss 清算问题搬进永续合约。代理要同时管理交易成本、库存风险、永久冲击，以及剩余头寸上**持续发生的资金支付**。在恒等支付 $\psi(s)=s$ 下，最优交易速度有闭式解

$$\nu^*(t,q,p,s) = \frac{1}{4k}\left[\left(\xi(t)+\pi(t)\right)q + \left(\xi(t)-\pi(t)\right)\frac{p-s}{b}\right],$$

其中按比例缩放的资金费率 $z=p-s$ 的系数在所有 $t<T$ 上为负：代理加快交易，以降低多头支付资金费的成本，或锁定空头收取资金费的收益。非线性支付下，当资金参数或交易区间很小时有近似解，前者是 Almgren–Chriss 策略的扰动。由于资金费在**剩余头寸**上累积，相当于持仓的一种 carry，永续头寸的最优时间窗与现货不同。

## AMM 上的执行与路由

CFMM 按规则而非订单簿定价。Angeris、Agrawal、Evans、Chitra 与 Boyd（2021）证明，一笔交易 $\Delta$ 被接受，当且仅当交易函数在其上不下降，$\varphi(R-\Delta)\ge\varphi(R)$（$R$ 为储备）；并证明在所有实际情形中（Uniswap、SushiSwap、Balancer、Curve、mStable），交易函数是凹的。这一凹性使多资产交易变得可解：选择交易、检测套利、读取价格预言机、跨多个池执行组合，都可以写成凸优化问题并精确求解，其乘子具有边际价格的含义。Angeris、Chitra、Evans 与 Boyd（2021/2022）区分了对实际路由有决定意义的两种情形：无固定成本时，跨 CFMM 网络的最优路由是凸问题；一旦有按场所的固定成本——gas——它就变成混合整数凸规划，只能靠慢的全局方法求全局解，或用基于凸性的启发式求近似解。gas 是按池固定的成本，因此路由是组合式的，是股票市场"固定场所费"问题的加密对应物。

Diamandis、Resnick、Chitra 与 Angeris（2023）用对偶分解把它做到能在机器人里实时运行：放松唯一的耦合约束后，问题变成各 CFMM 独立可解的子问题，用一次对偶更新迭代，并可容纳 Uniswap v3 这类集中流动性聚合 CFMM，以及跨多个费率档的路由。他们报告的目标值"常常提高超过 50%"，指的是求解器在测试实例上相对 Mosek 基线的目标值，而非交易成本节省。

Xi 与 Moallemi（2026）审计了以太坊主网上 2,980,000 笔 WETH–USDC swap 的真实路由。相对一个考虑 gas 的全场所最优，实际路由平均每笔损失 2.02 个基点，在约 USD 120.42 billion 的成交量上合计 USD 24.20 million。相对"只在已使用池上最优"的基准，损失只有 USD 6.58 million（均值 0.05467 个基点），可见大部分缺口来自没有启用更多资金池。若用滞后一个区块的状态而非执行时刻状态做优化，在无 gas 最优下再增 1.29 个基点（约 USD 15.5 million），在含 gas 最优下增 1.78 个基点（约 USD 21.4 million），说明次优性很大一部分是时点错配而非拆分不当。损失是重尾的，被三明治的交易在均值和离散度上都明显上移。

Cartea、Drissi 与 Monga（2023/2025）给出了 AMM 版的 Almgren–Chriss。CFMM 的执行成本就是其交易函数的曲率：对深度为 $\kappa$、边际率为 $Z$ 的恒定乘积池，水平函数满足

$$\varphi''(q^Y)=\frac{2\kappa^2}{(q^Y)^3}=\frac{2Z^{3/2}}{\kappa},$$

规模为 $y$ 的交易，其凸性成本约为 $\tfrac12\varphi''(q^Y_O)\,y^2$。单位成本因此**随交易规模线性增长**、**与池深度成反比**——这与限价订单簿不同，后者的单位成本由订单簿给出。凸性成本是随机的（LP 随时增减流动性），作者在三种情形下求解吃单方"汇率风险与执行成本"的权衡——参考汇率形成于竞争的 CEX、形成于池内、或两者兼有——并给出可实时部署的闭式近似。Lehar、Parlour 与 Zoican（2023/2024）解释了为什么路由器必须跨费率档拆分而非总打最便宜的池：面对固定的公共 gas 成本，高费率池吸引了 58% 的流动性供给，却只执行 21% 的成交量；大 LP 主导低费率池，小 LP 则聚向高费率池以缓解逆向选择，于是流动性被内生地停放在不同费率档里。他们的研究把 Uniswap 的费率档设定为 1、5、30、100 个基点，对应最小变动单位 1、10、60、200 个基点。

与订单簿的本质差异在于：AMM 对一条连续曲线定价，而订单簿报的是离散价位。给定储备，AMM 的成本是确定的、交易前已知，因此滑点可以被算出来而不是被发现；但镜像的损失落在流动性提供者身上。Cartea、Drissi 与 Monga（2023/2024）把 LP 的财富分解为手续费收入、池内持仓的盯市、以及 gas 再平衡成本，并分离出**可预测损失**——池机械地卖出升值资产、买入贬值资产带来的不可对冲损失，它随集中流动性区间收窄而上升。这是集中流动性情形下的 loss-versus-rebalancing（LVR）对应物，也是 AMM 执行成本的下界：吃单方的成本不可能低于手续费加上这笔交易造成的可预测损失。

## MEV 与交易排序

链上执行共享一种资源——区块内的交易顺序——由此产生股票市场没有的成本。Kulkarni、Diamandis 与 Chitra（2022/2023）把它形式化为两个博弈。路由 MEV：套利者的插入与重排会改变最优路由和自利路由；若三明治攻击的影响在适当意义下是局部的，则"无政府代价"是常数，与网络规模无关。重排 MEV：对 $n$ 笔用户交易，三明治造成的最大价格冲击相对平均冲击为 $O(\log n)$——有界但真实存在，随区块内交易数增长。Canidio 与 Fritsch（2023/2025）给出设计层面的回应：一个在成交前把交易批量化的"函数最大化 AMM"，在套利者竞争下以单一清算价成交，从而消除套利利润（LVR）和三明治攻击。用 Binance 数据对 11 个交易对模拟，他们得到的流动性提供收益下界大体上略高于对应的 Uniswap v3 历史收益；他们假设该批量 AMM 获得 Uniswap v3 成交量的 30% 作为噪音交易，并指出在其研究的一个池里约 60% 的成交量是噪音交易。对执行而言，MEV 防护是一笔有界但真实的成本，而路由——公开 mempool、私有通道、批量场所——要与 gas 和费率一起定价。

## 一个无法核实的说法

有一个被广泛重复的说法称：针对**完整费率表**做执行优化可以带来"超过 60% 的潜在节省"。本章无法核实它。所有已获取的文献都被转成文本并检索了关于节省与成本削减的表述；没有任何一段报告过由费率表优化带来的超过 60% 的节省。相邻的那些 60% 数字含义都不同：He 等文中的 52%–90%（或 60%–90%）是永续价格对其无套利基准的年化偏离；Canidio 与 Fritsch 的 60% 是噪音交易占成交量的比例；Barbon 与 Ranaldo 的 60% 是其样本覆盖的 DEX 成交量比例；Wu 等文中的 60% 与 89% 是两个套利者送给同一构建者的成交量占比。真正把完整费率表纳入模型的那些文献——He 等的分档费率与协商零费做市合同、Binance 费率表本身、Lehar 等的 1/5/30/100 基点费率档——报告的是 Sharpe 比率、无套利带和流动性份额，而不是一个百分比成本节省。最接近的**实测**执行节省数字远低于 60%：Xi 与 Moallemi 测得的 2.02 个基点路由次优，其中"池内切换"部分只有 0.05467 个基点。Diamandis 等的"超过 50%"是求解器在测试实例上相对 Mosek 基线的目标值，不是成本节省，也不是 60%。在有人给出具体出处之前，这个数字应被视为无来源；本章不把它当作事实重复。

相关的缺口：Aleti 与 Mizrach（2021）未能获取全文。其出版商摘要报告 BTC 现货—期货市场日成交约 USD 146–160 million，现货交易规模中位数低于 USD 1,300，而 CME 上超过 USD 18,000；平均买卖价差 0.0298%（约 2.98 个基点）；超过 USD 1 million 的交易使价格移动不到一个基点。这些只是摘要层面的数字，未对全文核实。

## 对加密执行者的含义

对**小额、无信息优势**的交易者，约束性成本是按比例的费用加固定的 gas。在 CEX 上交易、并在费率允许时挂单而非吃单，是文献支持的默认做法，因为小额名义下 gas 主导 DEX 成本（Barbon 与 Ranaldo）；把成交量集中到一家以提高费率档、在打折时用场所代币付费，直接来自费率表按 30 天成交量分档这一事实。

对**大额**交易者，成本由深度和冲击主导。平方根律以相同的指数、接近股票的归一系数成立（Donier 与 Bonart），因此调度可用第 5 章的工具标定；但场所选择会改变，因为 DEX 在约 USD 10,000 以上变得有成本竞争力，并可跨费率档拆分（Lehar 等）。链上路由应把 gas 纳入考虑、使用执行时刻的池状态——滞后一个区块的代价在 1.3 至 1.8 个基点量级（Xi 与 Moallemi）——并在流量暴露于三明治时使用 MEV 防护。

对**跨所或基差**交易者，约束性因素是结算延迟与资金费率择时。跨所偏离又大又持久，是因为资金被困住，而不是因为成本高（Makarov 与 Schoar；Hautsch 等），因此设计问题是：以何种库存持有方式、承担多大违约风险，把一个价差变成已实现利润。永续基差交易必须对照由**实际**费率档与资金规则给出的无套利带来评估（He 等；Ackerer 等），并把持有期设在"预期资金收益超过往返成本"的位置（Donnelly 等）。

上述结论中有两条属于本章判断而非某篇文献的结论。第一，"费率档与资金规则共同决定永续头寸的合适时间窗"是由两个独立结果（He 等的无套利带、Donnelly 等的受资金驱动的调度）推出的推论；本章所查文献中没有哪篇提出并求解这一联合优化。第二，"链上执行应优先保证执行时刻的池状态、而非复杂的拆分"是对 Xi 与 Moallemi 分解结果的解读——他们的美元缺口主要来自池启用、时间损失主要来自状态陈旧——并非这些作者关于策略设计的主张。

## 本章依据的文献

- Igor Makarov and Antoinette Schoar, "Trading and Arbitrage in Cryptocurrency Markets", *Journal of Financial Economics* 135(2), 293–319 (2020)，第 4.4、7.2、8.2 节。本地 PDF：`materials/papers/cluster-h/2020-MakarovSchoar-trading-arbitrage-cryptocurrency.pdf`。
- Nikolaus Hautsch, Christoph Scheuch and Stefan Voigt, "Building Trust Takes Time: Limits to Arbitrage for Blockchain-Based Assets", arXiv:1812.00595（v4, 2023）。本地 PDF：`materials/papers/cluster-h/2018-Building-trust-limits-to-arbitrage-blockchain.pdf`。
- Andrea Barbon and Angelo Ranaldo, "On the Quality of Cryptocurrency Markets: Centralized Versus Decentralized Exchanges", arXiv:2112.07386（v7, 2024）。本地 PDF：`materials/papers/cluster-h/2021-BarbonRanaldo-quality-crypto-markets-cex-vs-dex.pdf`。
- Fei Wu, Danning Sui, Thomas Thiery and Mallesh Pai, "Measuring CEX-DEX Extracted Value and Searcher Profitability", arXiv:2507.13023（2025）。本地 PDF：`materials/papers/cluster-h/2025-CEX-DEX-extracted-value-mev.pdf`。
- J. Donier and J. Bonart, "A Million Metaorder Analysis of Market Impact on the Bitcoin", arXiv:1412.4503（v3, 2015），第 4 节。本地 PDF：`materials/papers/cluster-h/2014-DonierBouchaud-million-metaorder-bitcoin-impact.pdf`。
- Christopher J. Cho, Timothy J. Norman and Manuel Nunes, "PRIME: A Price-Reverting Impact Model of a Cryptocurrency Exchange", arXiv:2305.07559（2023）。本地 PDF：`materials/papers/cluster-h/2023-PRIME-price-reverting-impact-model.pdf`。
- Davide Barone and Fabrizio Lillo, "Trading in the Sunshine or in the Shade: Market Impact and Adverse Selection on Hyperliquid", arXiv:2606.15715（2026）。本地 PDF：`materials/papers/cluster-h/2026-Hyperliquid-market-impact-adverse-selection.pdf`。
- Songrun He, Asaf Manela, Omri Ross and Victor von Wachter, "Fundamentals of Perpetual Futures", arXiv:2212.06888（2026），式 (7)、推论 2、表 3。本地 PDF：`materials/papers/cluster-h/2022-He-fundamentals-perpetual-futures.pdf`。
- Damien Ackerer, Julien Hugonnier and Urban Jermann, "Perpetual Futures Pricing", arXiv:2310.11771（v2, 2024）。本地 PDF：`materials/papers/cluster-h/2023-Ackerer-perpetual-futures-pricing.pdf`。
- Jaehyun Kim and Hyungbin Park, "Designing Funding Rates for Perpetual Futures in Cryptocurrency Markets", arXiv:2506.08573（2025）。本地 PDF：`materials/papers/cluster-h/2025-KimPark-designing-funding-rates-perpetuals.pdf`。
- Ryan Donnelly, Junhan Lin and Matthew Lorig, "Optimal Liquidation of Perpetual Contracts", arXiv:2601.10812（2026），定理 2、定理 6。本地 PDF：`materials/papers/cluster-h/2026-optimal-liquidation-perpetual-contracts.pdf`。
- Guillermo Angeris, Akshay Agrawal, Alex Evans, Tarun Chitra and Stephen Boyd, "Constant Function Market Makers: Multi-Asset Trades via Convex Optimization", *Proceedings of the IEEE*（2022）。本地 PDF：`materials/papers/cluster-h/2021-Angeris-cfmm-multiasset-convex-optimization.pdf`。
- Guillermo Angeris, Tarun Chitra, Alex Evans and Stephen Boyd, "Optimal Routing for Constant Function Market Makers", WWW 2022。本地 PDF：`materials/papers/cluster-h/2022-AngerisChitra-optimal-routing-cfmm.pdf`。
- Theo Diamandis, Max Resnick, Tarun Chitra and Guillermo Angeris, "An Efficient Algorithm for Optimal Routing Through Constant Function Market Makers", Financial Cryptography 2023。本地 PDF：`materials/papers/cluster-h/2023-diamandis-efficient-algorithm-routing-cfmm.pdf`。
- Weiye Xi and Ciamac C. Moallemi, "Quantifying Sub-Optimality in Routing for Automated Market Makers", arXiv:2607.20762（2026）。本地 PDF：`materials/papers/cluster-h/2026-quantifying-suboptimality-routing-amm.pdf`。
- Álvaro Cartea, Fayçal Drissi and Marcello Monga, "Decentralised Finance and Automated Market Making: Execution and Speculation", arXiv:2307.03499（v3, 2025），第 2.2–2.3 节。本地 PDF：`materials/papers/cluster-h/2023-CarteaDrissiMonga-execution-speculation-amm.pdf`。
- Álvaro Cartea, Fayçal Drissi and Marcello Monga, "Decentralised Finance and Automated Market Making: Predictable Loss and Optimal Liquidity Provision", arXiv:2309.08431（v3, 2024）。本地 PDF：`materials/papers/cluster-h/2023-CarteaDrissiMonga-predictable-loss-optimal-liquidity.pdf`。
- Alfred Lehar, Christine A. Parlour and Marius Zoican, "Fragmentation and Optimal Liquidity Supply on Decentralized Exchanges", arXiv:2307.13772（v7, 2024）。本地 PDF：`materials/papers/cluster-h/2023-Fragmentation-optimal-liquidity-supply-dex.pdf`。
- Kshitij Kulkarni, Theo Diamandis and Tarun Chitra, "Towards a Theory of Maximal Extractable Value I: Constant Function Market Makers", arXiv:2207.11835（v2, 2023）。本地 PDF：`materials/papers/cluster-h/2022-Capponi-theory-mev-cfmm.pdf`。
- Andrea Canidio and Robin Fritsch, "Arbitrageurs' Profits, LVR, and Sandwich Attacks: Batch Trading as an AMM Design Response", arXiv:2307.02074（v6, 2025）。本地 PDF：`materials/papers/cluster-h/2023-Arbitrageurs-LVR-sandwich-batch-trading.pdf`。
- Binance, "Fees & Transactions Overview, Spot & Margin fee tiers"，访问于 2026-11-10（`official-doc`）。`https://www.binance.com/en/fee/schedule`。
- Saketh Aleti and Bruce Mizrach, "Bitcoin Spot and Futures Market Microstructure", *Journal of Futures Markets* 41(2), 194–225（2021）——**未获取全文**；所引数字来自出版商摘要，未对全文核实。

## 本章要点

- 加密执行与股票执行的差别有六条结构性来源：7×24 无熔断交易、无统一行情带的场所碎片化、分层 maker–taker 费率、永续合约每 8 小时一次的资金费率、提现费与结算延迟、以及中心化订单簿与 AMM 并存。
- Binance 现货费率按 30 天成交量从零售端的 0.1000% maker/taker 分档到 VIP 9 的 0.0110%/0.0230%，每一档 maker 都低于 taker、做市商可协商至零，因此执行模型要纳入的是 maker–taker 价差与成交量分档，而不是单一比例费率。
- 跨所偏离又大又持久——2017 年 12 月至 2018 年 2 月潜在套利利润至少 USD 2 billion、美韩溢价一度达 40%——原因是资金被困且结算缓慢，而价差（1–10 个基点）与费率（0.1%–0.25%）远小于它。
- 加密市场的价格冲击规律与股票市场是**同一条**平方根律：$\langle I(Q)\rangle\propto Q^{0.5}$、归一 Y-ratio 约 0.9，在一个几乎没有高频交易的场所上跨越四个数量级成立，不需要加密专属的指数。
- AMM 的执行成本是交易函数的曲率，单位成本随交易规模线性增长、与池深度成反比，与订单簿的离散价位流动性本质不同；可预测损失/LVR 是吃单成本的下界。
- 最接近的实测路由节省数字很小——平均 2.02 个基点、USD 120 billion 成交量上合计 USD 24 million，其中池内切换部分只有 0.05467 个基点——因此 MEV 防护与及时的池状态比复杂的拆分更重要。
- 资金费率是期货—现货溢价的截断（clamp）函数（Binance 上 $\kappa=1$、$\gamma=0.05\%$、$\iota=0.01\%$），它使永续合约的有效到期时间随机化，因此是执行时间窗必须计入的成本，而不是免费信号。
- "针对完整费率表优化可带来超过 60% 的潜在节省"这一说法在本章所查文献中找不到依据；相邻的 60% 数字分别是年化基差偏离、噪音交易占比和样本覆盖率，而最接近的实测节省以基点计。
