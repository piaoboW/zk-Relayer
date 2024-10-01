# 从zk-Relayer到跨链

## 1. Introduction
2024年，区块链技术正在迅速发展和变革，其中跨链技术越来越受到市场青睐。目前，诞生了无数个独立的区块链网络，每个网络都拥有独特的特性和优势，一些主流链如下表：

<table border="2" align="center">
	<tr>
		<th align="center" width="10%">名称</th>
		<th align="center" width="20%">特点</th>
        <th align="center" width="35%">优势</th>
        <th align="center" width="35%">缺点</th>
        </th>
	</tr>
		<tr>
		<th align="center" width="10%">Bitcoin</th>
		<th align="center" width="20%">去中心化，安全可靠，不可篡改，基于工作量证明 (PoW) 共识机制。</th>
        <th align="center" width="35%">安全性高: PoW机制确保了比特币网络的安全性，抵御攻击的能力强。<br>
        去中心化: 没有单一机构控制网络，避免了中心化风险。<br>        
        全球性: 比特币是全球通用的数字货币，不受地域限制。</th>
        <th align="center" width="35%">交易速度慢: 比特币的交易确认时间较长，每秒交易量有限。<br>能耗高: PoW 机制需要消耗大量能源，对环境有负面影响。<br>可扩展性差: 比特币网络难以扩展，难以处理大量交易。</th>
        </th>
        <tr>
		<th align="center" width="10%">Ethereum</th>
		<th align="center" width="20%">智能合约平台，支持去中心化应用 (DApps) 开发，基于 PoW 共识机制 (2022年升级为 PoS)。
        </th>
        <th align="center" width="35%">可编程性: 支持智能合约，可实现多种复杂的应用场景。<br>活跃的社区: 以太坊拥有庞大的开发者社区，不断开发新的应用和工具。<br>丰富的生态系统: 以太坊上有大量的 DeFi 应用、NFT 项目等，形成了丰富的生态系统。<br>去中心化: 没有单一机构控制网络，避免了中心化风险。</th>
        <th align="center" width="35%">交易费用高: 以太坊网络拥堵，导致交易费用高昂。<br>可扩展性差: 以太坊网络难以扩展，难以处理大量交易。<br>安全性问题: 智能合约存在安全漏洞的风险。</th>
        </th>
        <tr>
		<th align="center" width="10%">Solana</th>
		<th align="center" width="20%">高性能区块链，支持每秒数千笔交易，基于 PoH 共识机制。</th>
        <th align="center" width="35%">高吞吐量: Solana 的交易速度快，每秒可以处理数千笔交易。<br> 低交易费用: Solana 的交易费用较低，吸引了大量用户。<br> 开发友好: Solana 提供了友好的开发工具，方便开发者构建 DApps。</th>
        <th align="center" width="35%">中心化程度高: Solana 的验证节点数量较少，存在中心化的风险。<br>生态系统尚未完善: Solana 的生态系统仍在发展中，与以太坊相比还较为薄弱。<br>网络稳定性: Solana 网络曾出现过多次停机事故，稳定性有待提高。</th>
        </th>
        <tr>
		<th align="center" width="10%">Polkadot</th>
		<th align="center" width="20%">跨链平台，旨在连接不同的区块链网络，基于 PoS 共识机制。</th>
        <th align="center" width="35%">跨链互操作性: Polkadot 可以连接不同的区块链网络，实现跨链通信和资产转移。<br>高可扩展性: Polkadot 支持分片技术，可以提高网络的吞吐量。<br>可定制化: Polkadot 允许开发者创建自定义的区块链网络，满足不同的应用场景需求。</th>
        <th align="center" width="35%">技术复杂: Polkadot 的技术架构较为复杂，对于开发者来说有一定难度。<br>生态系统发展缓慢: Polkadot 的生态系统仍在发展中，与以太坊相比还较为薄弱。<br>安全问题: Polkadot 网络的安全性还有待验证。</th>
        <tr>
		<th align="center" width="10%"> Cosmos</th>
		<th align="center" width="20%"> 跨链平台，旨在连接不同的区块链网络，基于 PoS 共识机</th>
        <th align="center" width="35%">跨链互操作性: Cosmos 可以连接不同的区块链网络，实现跨链通信和资产转移。<br>灵活的架构: Cosmos 支持多种不同的区块链网络，具有灵活的架构。<br>社区活跃: Cosmos 拥有活跃的开发者社区，不断开发新的应用和工具。</th>
        <th align="center" width="35%">安全性: Cosmos 网络的安全性还有待验证。<br>可扩展性: Cosmos 网络的吞吐量有限。<br>生态系统发展缓慢: Cosmos 的生态系统仍在发展中，与以太坊相比还较为薄弱。</th>
        </th>
</table>

独立的区块链网络越来越多，这些网络之间的割裂也导致了数据和资产流动性的不足，阻碍了区块链生态的进一步发展。因此，跨链技术应运而生，旨在打破区块链之间的壁垒，实现互操作性，构建一个更加开放、互联的数字世界。

## 2. 跨链技术
跨链技术的发展主要分为三个阶段：**单链扩张阶段**、**中继跨链阶段**和**互联互通阶段**。

单链扩张是针对加密数字资产的跨链交易，使用的技术包括公证人、哈希锁定和侧链。

中继跨链提供可重复使用的数据结构以及网络、共识、激励和智能合约等技术的框架，用于创建彼此之间可以相互通信的定制化区块链，主要的框架有Cosmos和Polkadot。

互联互通阶段是中继跨链阶段的技术深化，就是提供一个抽象层，能够通过公开一组统一的操作接口，允许DApp和不同的链交互时可以使用一套统一的API，前期主要技术有可信中继Trusted Relays、Blockchain-Agnostic Protocols、Blockchain Migrators，现阶段主要以跨链桥为代表。

从跨链技术的发展来看，早期的跨链技术更多关注的是资产转移，现有项目更多关注的是链状态的转移。

### 2.1 跨链分类

跨链交互根据所跨越的区块链底层技术平台的不同可以分为同构链跨链和异构链跨链：同构链之间安全机制、共识算法、网络拓扑、区块生成验证逻辑都一致，它们之间的跨链交互相对简单。而异构链的跨链交互相对复杂，比如比特币采用PoW算法而联盟链Fabric采用传统确定性共识算法，其区块的组成形式和确定性保证机制均有很大不同，直接跨链交互机制不易设计。异构链之间的跨链交互一般需要第三方辅助服务辅助跨链交互。

### 2.2 几种跨链技术

#### 2.2.1 公正人技术

公证人是加密数字资产跨链交易中最简单的。例如，Alice将A数字资产打到公证人的地址，然后在公证人服务器中挂单，接下来Bod将B数字资产打到公证人地址，在公证人挂买单，最后公证人完成撮合后，将A资产打到Bob的A资产地址中，将B资产打到Alice的B资产地址。公证人机制的缺点是存在中心化风险。

![alt text](<fig_Relayer/deal.png>)
<p align="center"><font face="黑体" size=3.>Figure 1 Notary Public Mechanism</font></p>


#### 2.2.2 哈希锁定技术
交易者会首先在各自的链上使用哈希锁对资产进行锁定，并且使用时间锁来保证在公布哈希锁的原项前资产不会被取走。哈希时间锁定并不仅仅针对公有链，在联盟链中也可以集成。但是哈希时间锁定依然有缺陷，例如会造成交易的不公平，后加锁的一方或者先解锁的一方可以根据加密数字资产的价格来决定是否进行交易。

![alt text](<fig_Relayer/Hash locked.png>)
<p align="center"><font face="黑体" size=3.>Figure 2 Hash locking technology</font></p>

#### 2.2.3 侧链

侧链技术能够验证和解析主链中的区块数据和交易数据。侧链实现的核心技术是双向锚定(Two-way Peg)，通过双向锚定技术可以将数字资产在主链上进行锁定，同时将等价的资产在侧链中释放。反过来也可，当侧链中相关资产进行锁定时，主链上锚定的等价资产也可以被释放。主要包括以下几个步骤：

* （1）发送锁定交易：把比特币锁定在主链上。由比特币持有者操作，发送一个特殊交易，把比特币锁定在区块链上。

* （2）等待一个确认期：确认期的作用是等待锁定交易被更多区块确认，可防止假冒锁定交易和拒绝服务攻击。

* （3）在侧链上赎回：比特币确认期结束后，用户在侧链上创建一个交易花掉锁定交易的输出，并且提供一个SPV工作量证明，输出到自己在侧链上的地址中去。该交易称为赎回交易，SPV工作量证明是指赎回交易所在区块的工作量证明。

* （4）等待一个竞争期：竞争期的作用是防止双花。在此期间①赎回交易不会被打包到区块，②新传输到侧链的比特币不能使用，③如果有工作量更大的工作证明出现，即该赎回交易包括了比特币主链更大难度的SPV证明，则上一个赎回交易将被替换。竞争期结束后，该赎回交易将被打包到区块中，用户可以使用他的比特币。

侧链是以锚定某种原链上的代币为基础的新型区块链。比如，以太坊可以成为比特币的侧链，比特币作为以太坊的主链。但是主链不知道侧链的存在，侧链知道主链的存在，即侧链能读懂主链。侧链的缺点是随着交易量的增多，智能合约内部的数据存储存在膨胀问题，可能会造成交易处理速度慢，甚至出现交易堵塞的情况。

侧链通过特定的双向锚定机制与主链建立联系，使得代币可以在主链和侧链之间流动。侧链可以解决主链扩容性和功能性的问题，提高区块链的灵活性和可扩展性。通过侧链，可以实现去中心化金融（DeFi）应用、跨链互操作性等复杂功能。

#### 2.2.4 中继链

中继链是提供可重复使用的数据结构以及网络、共识、激励和智能合约等技术的框架，用于创建彼此之间可以相互通信的定制化区块链。当前主要的框架有Cosmos、Polkadot和以太坊的Beacon Chain。Cosmos是一个多链并行的区块链网络，每条链被称为Zone，所有的Zone采用相同的共识算法(Tendermint)，Zone可以通过Hub将信息传输到其他Zone，同时Hub可以最大限度的减少Zone之间的链接数量而且可以防止双花攻击。例如A Zone可以将自己Zone的数字资产转移到B Zone或从B Zone接收数字资产。在这个过程中A仅需要信任Hub和B Zone就可以。这样的设计还有一个好处就是可以简化区块链的开发，例如Cosmos将区块链的开发抽象为三层，即网络层、共识层和应用层。Tendermint实现了网络和共识层，然后通过一个内部的ABCI(Application Blockchain Interface)协议连接到应用层。同时Cosmos提供了SDK来让开发者进行应用层的开发。

中继链的发展仍处于早期阶段，但其潜力巨大，未来有望成为区块链领域的重要发展方向之一。

## 3. 互联互通阶段的跨链桥
随着当下区块链技术行业的多链生态模式的进一步发展，很多不同链上资产，也有很多个DAPP。不同的DAPP建立在不同的公链上，彼此无法顺畅交互，链上资产也不能快捷地实现迁移与价值交换.因此，跨链已的第三阶段（互通互联阶段）随之而来，
典型的代表是跨链桥。跨链桥是连接独立区块链网络的基础设施协议，允许数字资产从一个区块链无缝转移到另一个区块链，从而增强互操作性。

### 3.1 互联互通的形式

跨链的表面虽然是资产在跨链，但背后实际上就只是“讯息”在跨链而已。这些讯息像是“在A链上把资产锁住/烧毁了”或“在B链上把资产解锁/铸造出来了”，讯息的接收方就按照讯息内容来执行相对应的处理。

由于跨链实际上是信息的交互，因此可以有不同的实现方式。例如下面两图分别是A cross-chain form of independent interconnection and Cross-chain form of using connectors。

![alt text](<fig_Relayer/LIN1.png>)
<p align="center"><font face="黑体" size=3.>Figure 3 Cross-chain form of independent interconnection</font></p>


![alt text](<fig_Relayer/LIN2.png>)
<p align="center"><font face="黑体" size=3.>Figure 4 Cross-chain form of using connectors</font></p>

以上两种方式在使用中都有各自的优缺点。我们关注的是互联互通过程中“链不知道彼此存在”这个事实，因此它需要“相信某个人来传递讯息”。所以技术的主要挑战就在于要怎么验证一个送来的讯息的有效性。

### 3.2 跨链桥

跨链桥是实现区块孤岛互联互通的重要途经。跨链桥接通常通过智能合约锁定或销毁原链上的加密资产，并在新链上解锁或铸造加密资产。

主要的的几种跨链桥有Trusted Relayers、Optimistic Verification、Light client。

#### 3.2.1 Trusted Relayers

Trusted Relayers就是信任讯息传递者。基本上相信讯息传递者后，也不需要再验证讯息有效性了，只要是信任的讯息传递者带来的讯息都相信是有效的。而自然地当有了信任及中心化的假设，架构就会简单许多，而且成本会很低。

Trusted Relayers的安全假设是Honest Majority，也就是过半数的讯息传递者必须是诚实的好人。如果超过一半的讯息传递者是坏人或被骇客入侵，则这个跨链桥的就不再安全。

#### 3.2.2 Optimistic Verification

此技术是先乐观地接受传递过来的讯息，但会验证讯息的有效性，如果发现是无效的则发起挑战。优点是绝大多数的时间系统都会是正常运作（因为作恶会被挑战并惩罚），讯息都是有效所以不需发起挑战，所以成本非常低，因为不需要在链上去验证讯息。缺点是必须要有一段挑战期（Optimistic Window），让负责验证的人有足够多时间验证并发起挑战。

一个典型的Optimistic Verification有三个角色：Updater、Relayer及Watcher。

Updater：抵押担保，并负责为讯息签名做担保；

Relayer：负责把讯息及Updater的签名送到目标链上；

Watcher：负责监督Updater，并在Updater作恶时反应。

#### 3.2.3 Light client

这种方式是让目标链成为来源链的轻节点，可以是链下运行一个轻节点，也可以是用链上合约模拟一个轻节点：合约里记录来源链每个区块并验证每个区块的标头档（Block Header）。除了验证标头档的内容是有效的之外，还需要验证共识，也就是PoW的验算或PoS的投票结果。

由于各个链的区块内容和共识机制不一样，因此Light client验证成本会比其他跨链桥高许多。它的优点是非常安全，它不需要相信Relayer，它会自己验证区块和共识。

#### 3.2.4 Cost and Security

以上三种跨链桥技术，Trusted Relayers成本最低，因为不需什么复杂的验证，但是Relayer带来的资讯都直接相信，所以安全性最低。
Optimistic Verification：需要验证Merkle Proof，并且假设至少有一个Watcher是诚实的 cost和Security都一般。
Light Client：要验证最多东西，包含共识、区块标头档及交易或状态的证明，cost较高的同时带来非常高的安全性。

## 4. zk-Relayer

现有的跨链桥技术都会涉及一个重要角色：Relayer。它的作用就是负责把讯息及数据送到目标链上，完成消息传递工作。在Trusted Relayers中我们相信Relayer是可信的，不需要验证它带来信息的有效性，在其他的一些跨链桥技术中一般会对Relayer传递的信息进行简单复杂的验证，以保证安全性。

### 4.1 Relayer工作原理

上面分析我们知道一个可信的Relayer是实现跨链的关键。我们希望有一种方法在花费极小代价的基础上来保证跨链信息的安全性。这种方法就是零知识证明(zk).

零知识证明是一种密码学技术，允许一方（证明方）向另一方（验证方）证明某个陈述是真实的，而无需透露除该陈述真实性以外的任何信息。比如在ZKRollup（Rollup的一种实现方式）中，零知识证明用于验证链下交易的有效性，同时保持交易数据的隐私性。

Relayer使用零知识证明技术为每一批次交易生成一个简洁的证明。这个证明被提交到链上的智能合约进行验证。智能合约只需验证证明的有效性，而无需检查每一笔交易的细节，从而大大提高了跨链的执行效率，并且具有极高的安全性。

### 4.2 zk-Relyer技术分析
我门先来看一个使用zk-Relayer的例子：zk-rollup. zkRollup项目是 Layer2 二层扩容技术，一般会包含2个基本角色：

Transactor：可以理解为用户, 用户构建转账交易并用私钥签名，然后将交易发送给relayer。

Relayer：负责收集并验证用户的交易，之后将交易批量打包压缩，并生成zk-SNARK的Proof。最后relayer将用户交易中的核心数据+Proof+全量用户新状态的Merkle Root提交到链上的Layer2智能合约。Layer2 智能合约验证Proof通过后将新状态的Merle Root更新至新区块的stateRoot。

整个zk-rollup一般至少有两个角色：

* Transactor：可以理解为用户，用户构建转账交易并用私钥签名，然后将交易发送给 relayer。
* Relayer：负责收集并验证用户的交易，之后将交易批量打包压缩，并生成 zk-SNARK 的 Proof。最后relayer将用户交易中的核心数据 + Proof + 全量用户新状态的 Merkle Root 提交到链上的 Layer2 智能合约。 Layer2 智能合约验证 Proof 通过后将新状态的 Merkle Root 更新至新区块的stateRoot。所以也可称为zk-Relayer。

zkRollup 的本质是将原本在链上的用户状态变更，转移到链下进行，同时通过 zk-SNARK 的Proof 来保证链下用户状态变更过程和结果的正确性。

下图是zk-Relayer的执行过程。
![alt text](<fig_Relayer/zk-relayer.png>)
<p align="center"><font face="黑体" size=3.>Figure 5 zk-Relayer</font></p>

input：Transactor用私钥签名过的若干transaction和区块链上某个区块的Merkle Tree Root。
output：zk-proof和交易前后的Merkle Tree Root：r1 and r2。

生成的zk-proof可以保证：

* 交易前后状态变化过程没有问题，即重新提交的Merkle Tree Root r1 and r2 没有问题。
* 每笔交易的数据没有问题。
* 用户签名没有问题。

链上的智能合约通过简单验证原先的状态根与提交的r1是否相等和提交的zk-proof来确定是否接收这批交易。

### 4.3 zk-Relyer跨链
上节的zk-rollup的例子分析了zk-Relayer的工作原理。起初zk-Relayer是用来对以太坊进行扩容。随着跨链技术的发展，zk-Relayer的技术特性使其在构建无需信任的全链互操作性协议方面具有无与伦比的优势。一个典型的例子就是LayerZero，它
是一个区块链互操作性解决方案，本质是一种链上Light client，由LayerZero Labs开发，旨在打破不同区块链之间的信息孤岛，实现价值和数据的无缝流动。前面提到的Light Client跨链桥虽然安全，但成本很高，通过使用zk-Relayer可以有效降低这个成本。

LayerZero使用的是Fig.4的跨链结构，在每个支持网络（以太坊、Solana、Polygon等）上部署了一套智能合约，用户必须通过LayerZero合约与各链的合约互动。

它的核心是一个称为“中立的消息传递层”（主要技术是zk-Relayer）。它允许不同的区块链系统之间进行直接的、无需信任的交互，而不依赖于中心化的第三方或桥接器。这种设计减少了攻击面，提高了跨链资产转移的安全性。

![alt text](<fig_Relayer/layerzero.png>)
<p align="center"><font face="黑体" size=3.>Figure 5 LayerZero</font></p>

LayerZero 将他们在轻量级链上的客户端称为“LayerZero Endpoints”，它们由智能合约组成。

LayerZero依赖于Oracle和Relayer在链上端点之间传输消息。Oracle的功能是获取区块头，Relayer功能是获取交易事件信息。

Oracle可作为第三方服务，提供一种独立于其他 LayerZero 的机制组件，能从一个链中读取一个块头并将其发送到另一个链上，这样能在目标链上验证源链上交易的有效性。

Relayer是一种链下服务，但它并不获取块头，而是获取指定交易的信息并提交proof。

Oracle提交的区块头将与Relayer提交的交易证明进行交叉验证，二者不形成任何共识，只传输消息，通过简单的验证即可完成跨链通信。

### 4.3 zk-Relyer主要优势

* 隐私保护： zk-Relayer 通过 ZKP 保护了交易信息，防止交易内容被泄露。
* 安全性： ZKP 可以确保交易的合法性，防止恶意攻击和欺诈行为。
* 可扩展性： zk-Relayer 可以处理大量的跨链交易，提高跨链效率。

## 5 展望

如果说共识机制是区块链的灵魂核心，那么对于区块链特别是联盟链及私链来看，跨链技术就是实现价值网络的关键，它是把联盟链从分散单独的孤岛中拯救出来的良药，是区块链向外拓展和连接的桥梁。

Relayer这种方式是未来跨链网络架构的主要形式，因为不管是中继链还是中继平台，都可以作为一个可信的消息验证平台，减轻区块链平台的信息验证负担。随着 ZKP 技术的发展，zk-Relayer 将成为跨链技术的重要组成部分,它通过 ZKP 保护了交易信息，实现了隐私保护的跨链交互。未来，zk-Relayer可能支持更广泛的区块链网络, 实现更广泛的跨链互操作性，为构建更加安全、高效、隐私保护的跨链生态系统做出贡献。

## References：

<p name = "ref1">[1]Dinh, T. T. A., Wang, H., Chen, S., Vo, B., Nguyen, Z. T., & Thai, M. T.. Blockchain Interoperability: A Comprehensive Survey. IEEE Access, 9, 38904-38940. 2021.https://doi.org/10.1109/ACCESS.2021.3063461

<p name = "ref1">[2]Shahsavari, S., Ravi, A., Sharif, K., Moniruzzaman, M., & Singhal, M.. A Survey on Cross-Chain Interoperability Solutions.2021. arXiv preprint arXiv:2106.09491.

<p name = "ref1">[3]Buchman, E., Kwon, J., & Jae Kwon, Z.. Cosmos: A Network of Distributed Ledgers.2017. https://cosmos.network/cosmos-hub-whitepaper.pdf

<p name = "ref1">[4]Buterin, V..Cross-Chain DeFi: Bridging the Gap Between Blockchains. Vitalik.ca.2021. https://vitalik.ca/general/2021/01/26/crosschain.html

<p name = "ref1">[5]StarkWare. StarkNet: A permissionless decentralized ZK-Rollup. 2023. https://docs.starknet.io/

<p name = "ref1">[6]https://docs.layerzero.network/v2/home/getting-started/send-message

<p name = "ref1">[7]https://arxiv.org/pdf/2210.00264
