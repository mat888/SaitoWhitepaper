Some editor notes:

-----------------------------------------------------

There may be technical inaccuracies. My strategy in editing was to fully understand as I read and change content to better enable the understanding from a newbie's POV. I tried not to change content without feeling I understood it, but someone more experienced will have to be the final auditors.

Whole paper changed to third person, as most of it was in third person. That means replacing use of 'we, us, etc.' with more omniscient diction.

Use of quotes for invented terms is only used now before the term is defined. Once a term has been defined its future occurrences in the paper are un-quoted.

Structure remains mostly the same. Most edits are within single sentences and keep the meaning but elaborate.

*characters between stars indicate italics*

content in funny brackets []{} are notes not intended to be in the final paper, they are usually questions about if an edit is accurate or acceptable

footnotes were not looked at in this edit

-----------------------------------------------------

Saito fixes the collective action problems that impede scaling in proof-of-work and proof-of-stake blockchains by coupling a finite ledger to a consensus mechanism that incentivizes the collection and sharing of transaction requests and the fees associated. The resulting network pays not just for (mining and staking) [Saito does in fact mine and stake, or is this a only reference to the status quo?], but for all activities that contribute economic value to the network. In the process, Saito fully eliminates majoritarian as well as other attacks.

Saito pays user-serving infrastructure nodes out of an open consensus mechanism. This incentive gives economic motivation for the network to scale with new users and can be used to build secure decentralized versions of many data heavy services, such as un-astroturfable data exchanges, authentication and monetization applications, distributed key registries, payment channels and much more.

In economic terms, Saito can be understood as a solution for inducing a free market to deliver a public good, much like the vision of Bitcoin. Unlike classical crypto-currencies, Saito's design corrects for collective action problems inherent in proof-of-work and proof-of-stake consensus; the competitive nature is shifted from work/stake so that profit-seeking actors instead strive to bring as many transactions into the system as they can, enabling natural and robust scalability. Indeed, estimates indicate that the practical limit for a Saito blockchain today is in the order of 100TB of data per day, and advances in routing capacity will it to the petabyte level within a decade - all thanks to the fact that Saito is bottle-necked by its hardware, rather than its consensus.

The next section describes briefly the economic problems that need to be solves in order to build a scalable blockchain. The following sections outline how Saito solves these problems and describes an implementation of these methods.

1. THE PROBLEM

The problem with blockchain scaling is not at the network technology layer: at the time of writing, data centers around the world are implementing 400Gbps network switches while 100Gbps connections are becoming standard even in lower-tier colocation facilities. Given the resources to pay for the necessary equipment there are only constraints from current consensus mechanisms which prevent a blockchain from being as decentralized, open and efficient as the public Internet backbone.

What limits network growth is the challenge of the network paying for itself. In the past, non-economists have waves this limitation, claiming that as long as *someone* is earning money from the network they will, or must be paying the costs or performing the tasks necessary to support it. But this is not the case - proof-of-work and proof-of-stake are afflicted by two market failures: a tragedy-of-the-commons problem that leads to blockchain bloat and eventual collapse in addition to a free-rider problem that leads to an under-provision of user-serving infrastructure and an over-provision of paid activities like staking and mining. Neither problem is crippling at small scales, but they grow incapacitating as bandwidth and storage costs rise with the growth of the network. 

Are there alternatives? Faced with the need to pay for uncompensated network infrastructure, computer scientists throw the problem to the market. As economists have known since the 1960s [reference?], asking the private sector to fund non-excludable infrastructure requires closure somewhere in the economic model. In layman's terms, if profit seeking actors receive their earnings from only one aspect of a system, they will over-optimize that aspect while keeping other facets of the system merely on life support. Actors which make earnings from fee collection will naturally attempt to keep restricted access to fees which they are working on unless they are incentivized to do otherwise. In proof-of-work and proof-of-stake the only incentives come from mining and staking - hoarding transactions therefore is the most competitive strategy (as it gives competing miners or stakers less potential reward at the same cost), undermining the openness of the consensus layer.

The only viable solution is to eliminate these market failures on the incentive level. Before the solution can be understood, the issues must be seen clearly. The major issues are as follows:

The tragedy-of-the-commons issue is created by the existence of a permanent ledger, which encourages nodes to accept payment now for storage costs that can be offloaded to others in the future. This incentive leads to bloated blockchains, and more subtly to transaction mis-pricing, as users can pay fees that do not reflect the cost of their transaction to network across time. The fact that this is a fundamental this is a fundamental problem is self-evident from the way Satoshi's solution is "not to care," an approach that stops being viable in networks that operate at serious economic scale.

Eliminating the tragedy-of-the-commons problem requires all nodes that add transactions to the blockchain to bear the cost of processing those transactions for as long as they remain on the blockchain. In practice, this requires a market mechanism for accurately determining the price of on-chain data storage. It also requires eliminating blockchain creep or deferring fee collection so that payments are meted-out over time as the node continues to do work for payment. One solution to this problem is described in section 2.

The free-rider problem is more insidious; It arises in blockchains where payments are made for one specific type of work, like mining or staking, at the expense of other necessary activities. This mismatch incentivizes participants to maximize their resources on paid work and minimize them on anything else. In the blockchain space, this results in miners and stakers "free-riding" on those who do the unpaid work of collecting transactions, developing applications, or otherwise supporting the user-facing network. The problem gets worse as the network scales, and evolutionary pressures make the trap inescapable: any Bitcoin miner who spends a smaller percentage of her revenue on hashing than her peers will lose market share and eventually capitulate.

In economics, the typical solution to the free-rider problem is to eliminate the property of "non-excludability" associated with any good or service: restricting its benefits to those who pay the cost of provision. This is impossible in the blockchain space without destroying the openness of the network, as it would require all users to run nodes in order to use the network. Computer scientists often address the problem by adding protective middle-ware, such as wrapping consensus payments in closed voting rings which themselves remain susceptible to the same economic attacks. The *jiggery-pokery,* whack-a-mole style approach can never solve these economic problems; markets are powerful enough to undermine these structures exactly when it is profitable to do so.

Without a solution to this problem the compromise resides between a network which cannot scale because it can only pay for a limited amount of network operations, or a network which scales but loses the open, decentralized, trustless qualities that make blockchain a useful invention in the first place. Neither approach is useful for building a genuinely open blockchain at a massive scale.

The theoretical solution to the free-rider problem requires eliminating the possibility for free-riding by fixing the underlying incentive structure so that participants are paid for providing what the network needs in a holistic sense. Because the blockchain requires a quantifiable cost-of-attack, it is necessary to eliminate "mining" and "staking" as they are and shift to a different form of work that accounts and compensates nodes in proportion to the *value* they provide to the network rather than the amount of hashing or staking they do.

The Saito network uses a new approach to measuring the value of work done and pays nodes in proportion to this value. This requires deriving the measure "work" from the transaction fees that users pay. The work of routing transactions into the network is the value that the Saito network bases rewards off of. Honest nodes can be induced to do this work by expectation of a share of the fees collected. The challenge then shifts to ensuring this mechanism preserves the cost-of-attack properties, such that attackers cannot spend their own money to attack the network and harvest it back in a perpetual loop. The security mechanism described in Section 3 outlines a technical method of accomplishing this.

2. FIXING THE TRAGEDY OF THE COMMONS

Saito solves blockchain creep by allowing the nodes in the network to delete the oldest blocks in the ledger when a new block is accepted. Epoch length is specified in the consensus code and determines how many blocks are kept by nodes at one time. Different epoch window lengths may be suited to different applications: in an extreme case, a blockchain designed to handle global traffic for distributed key-exchange applications may have an epoch as short as 24 hours.

Saito specifies that once a block falls out of the current epoch, its unspent transactions outputs (UTXOs) are no longer spendable. Buy any UTXO from that set which contains enough tokens to pay a rebroadcasting fee must be rebroadcast in the very next block. Block producers do this by creating "automatic transaction rebroadcasting" (ATR) transactions which include the original transaction data, but have entirely fresh and newly-spendable UTXOs which not need be rebroadcast for a whole 'nother epoch. After two epochs block producers may delete all block data from that first epoch, although the 32-byte header hash be be retained to prove the connection with the original genesis block. 

The ATR mechanism fixes the tragedy of the commons problem completely, making it impossible for the blockchain to grow so large that it collapses. They key is ensuring the rebroadcasting fee payed for by ATR transactions is a positive multiple of the average fee paid by new transactions over the previous epoch - in other words, the cost to rebroadcast can not be cheaper than the cost to add data to the chain, within an epoch. As the blockchain expands and there is less space for new transactions available, market competition pushes up the fees paid by new transactions. This in turn forces up the rebroadcast fees and increases the amount of data pruned from the blockchain. The market reaches equilibrium when old data is removed at the same rate that new data is added. 

Market discovery of the true cost of blockchain processing is a side-effect of this incentive structure, which works by removing the incentive block producers have to add unprofitable data to the chain. This incentive mechanism replaces hard-coded economic variables which cannot meaningfully adapt to changing conditions in the market or network and prevents pernicious forms of free-riding present in other chains, like deletion of on-chain data or refusing to participate in historical block storage despite earning network rewards. All such forms of cheating disappear as nodes which do not store the whole blockchain are incapable of producing new blocks, as they do not know which payments must be rebroadcast and thus will fall out of consensus with nodes which are correctly rebroadcasting.

While this avoids the problem of the blockchain growing too large for network nodes to store, and ensures space on the blockchain can be priced accurately as hardware advances, fixing the tragedy-of-the-commons problem does not address the issue of compensating nodes in the peer-to-peer network which pay for the auxiliary functions necessary to keep the network operating. To solve this problem, a new consensus mechanism is required.

3. ELIMINATING FREE-RIDING
[Figures in this section should be titled: "Routing Work Required"

In Saito any node can create a block at any time provided is has enough "routing work" accumulated for its proposed block, i.e. in its mempool. The amount of "routing work" required to produce a block depends on how quickly the proposed block follows the most recent one: consensus rules dictate that the work required starts high and linearly decreases with time until it reaches zero. Since block producers will issue blocks as soon as it becomes profitable, the pace of block production is a function of the overall amount of "routing work" fulfilled by the network.

Figure 1: The Burn Fee Curve

Saito derives routing work from the transaction fee embedded in every transaction. Using this measure of work to produce blocks makes attacking the network expensive, since making claims about time cost money. It can be seen from Figure 2 that it is impossible for attackers to produce blocks at a faster rate than the main chain unless they have access to a larger pool of transaction fees. To secure this mechanism, Saito has routing nodes cryptographically sign transactions as they propagate through the network. Consensus rules specify that the amount of routing work a transaction provides any node drops with the number of hops in its routing path, and that transactions provide no usable routing work to nodes that are not in their routing path. The work used to produce blocks now becomes the efficient collection and sharing of inbound network fees.

Figure 2: Good Actor Burn Fee Costs...

As long as there is no payment for block production, this system offers comparable security to Bitcoin: cost-of-attack can always be quantified and attackers must spend their own money to attack the chain. This allows users to wait however many block confirmations are needed to meet their security requirements. As a bonus, the network can increase the amount of routing work needed for block production to keep blocktime constant as transaction volume grows, so that security scales with fee-volume. 

The major problem with this approach lies in the consequences of requiring the network to burn fees to produce routing work:

Figure 3: Deflation of Burn Fee Over Time

Avoiding a deflationary crash requires the network to conserve its token supply, but fees from routing work cannot be given directly to block producers, as that would allow attackers to use the income from one block to generate their routing work needed to produce the next. Dividing up the payment between different nodes is preferable, but as long as block-producers have any influence over go gets paid a savvy attacker can sybil the network or conduct grinding attacks that target the token-issuing mechanism.

Solving this problem requires inverting the classic proof-of-work solution. In Bitcoin, consensus rules make it expensive to produce blocks and fees are then handed to the block producer. This is meant to ensure block production is expensive but in reality guarantees there are always conditions under which attacks are profitable. In Saito the solution is the opposite. The first order-problem becomes securing the payment mechanism i.e. ensuring that payments are proportional to work regardless of who produced the block. The cost-of-attack a mechanism with this property is what becomes the cost of block production.

The trick is to ensure that ensures there is always a quantifiable cost of attacking the system. The mechanism used to achieve this is dubbed the "golden ticket" solution. This mechanism pays honest nodes for collecting fees regardless of who produces blocks. The solution is returning the transaction fees to the network through a process that cannot be gamed by any of the players in the system without them spending far more money on the attack than they stand to gain via rewards. Implementation details are described in the next section.

4. THE GOLDEN TICKET

Whenever a node produces a block, it may collect the difference between the amount of routing work included in its mempool and the amount of routing work required for block production. No other payments are made.

Unlocking those payments requires the network to solve a computational puzzle, the "golden ticket." This puzzle requires knowledge of the block hash to solve and cannot be calculated in advance. Miners on the network listen for blocks as they are produced and begin hashing in search of a solution. Should they find a transaction instead, they propagate it back into the network as a normal fee-paying transaction.

Only one solution may be included in any block, and that solution  must be included in the very next block to be considered valid. If these conditions are violated or a golden ticket is not solved, the funds that were not paid out in the previous block are simply not allocated. They fall backwards into the blockchain and, reaching their epoch's end, are recollected by the consensus layer and redistributed as part of a future block reward.

Should a golden ticket be solved in time, the unallocated fees are released to the network, split between the miner that found the solution and a random node in the peer-to-peer routing network. The lucky routing node is selected using a random variable sourced from the miner solution with each routing node's chance of winning normalized to be proportional to the amount of routing work it contributed to the solved block.

As transactions are routed into the network, it can be seen that the total claim on transaction fees (sum of routing work accumulated among all nodes in the routing path) grow while the amount of remaining work available shrinks. Attackers who use honest transactions to produce blocks must use their own tokens to exceed the routing work of honest nodes - and while this may increase their chance of winning the reward, they would have to spend proportionally *more* than their increased odds are worth, making attacks based on receiving one's own transactions a losing strategy.

The division of rewards between miners and routers, *the paysplit*, is by default set to 0.5 (half to miners, half to routers) but can be made adjustable as described in the section below. The golden ticket system can be visualized as follows:

Figure 4: The Golden Ticket System

This system has several major advantages over proof-of-work and proof-of-stake mechanisms. The most important is that Saito explicitly distributes fees to the nodes that service users, both by collect transactions and producing blocks, and does so in proportion to the amount of value actors provide to the overall network. network nodes compete for access to lucrative inbound transaction flow, and will happily fund whatever development activities are needed to get users on the network. Of note, the services provided by edge nodes to attract Saito usage can include public-facing infrastructure needed by other blockchains.

This is a fundamental shift. Where other blockchains explicitly define which activities have value, Saito lets the users signal what services provide value through fee-pricing, while the network infers who deserves payment. Saito also directly incentivizes the efficient delivery of value to users. {A sentence here elaborating a bit on the last would be helpful} By paying for all of what users value rather than a subset of network activities Saito provides the only way to guarantee that a self-sufficient network can remain open and economically independent at scale.

The Saito consensus mechanism is also "twice as secure" as its proof-of-work and proof-of-stake counterparts. Honest nodes route transactions to block producers and earn fees in exchange. But attackers are thrown into a catch-22: they must not only spend the same amount of fees as the honest network to produce a competitive chain, but must also match 100 percent of the mining output to find enough golden to ticket solutions to recapture their funds. For attackers to successfully launch fee-recycling attacks, they must out-mine the rest of the network to scrape their profits - adding enough cost of attacking the network to outweigh the potential earnings of attack.

The basic version of the Saito system achieves 100 percent fee-security, meaning zero profit potential for consistent attackers, thus eliminating the fifty-one percent attack completely. Section 5 describes a modification to this mechanism that pushes security above 100 percent and guarantees that attackers will lose money in all circumstances. Regardless of which implementation is used, the economic problems created by mechanisms that rely on external supply-curves vanish: mining serves as a pure cost function instead of a difficulty function and the blockchain remains secure even if the supply curve for hashpower becomes perfectly flat.

