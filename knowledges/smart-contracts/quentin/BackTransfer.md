SOURCE: https://x.com/gfusee33/status/1758154330115293262

---
Explaining BackTransfers with a concrete use case at 
@autoscale_
 

BackTransfers is a simple, yet highly underrated feature on #MultiversX. It significantly levels up the composability 🛠️

I will explain to you how we used it at 
@autoscale_
 to integrate 
@HatomProtocol
 🧵

Plan of the article

1️⃣ Our objectives at autoscale
2️⃣ The challenge we encountered
3️⃣ The solution provided by the BackTransfers feature

I will talk about smart contract development, but I will do my best to explain everything in comprehensive words 🙏

If you enjoy this type of technical content, please throw me a RT, a like and a follow, it supports me a lot 🫶

1️⃣ Our objectives at autoscale

As a yield optimizer, we aim to provide our users with the best yields by offering them multiple automated strategies. We recently developed a strategy based on Hatom, a well-known lending protocol. The strategy is straightforward: we lend assets within their protocol to earn rewards, which are then immediately re-deposited to accumulate more rewards

When you lend assets on Hatom, you earn rewards in three different ways:

- The accrued interests paid by borrowers. These rewards are automatically compounded and, therefore, are not directly claimable

- The incentive rewards Hatom pays to lenders

- The boosted rewards Hatom pays to lenders, depending on the amount of HTM they have staked in their "booster" smart contract

The simplified workflow of the strategy is as follows:

a) We deposit users' assets into the lending pools

b) After some time, we claim both the incentive and boosted rewards

c) We re-deposit those rewards as assets to enhance the users' lending positions

d) The process repeats from step b

Important point: the steps b and c are done by the strategy in the same transaction

While this workflow appears simple, the implementation of the second point introduces some technical challenges

2️⃣ The challenge we encountered

Before continuing, I need to clarify how incentive and boosted rewards are distributed

Hatom has chosen to distribute rewards in batches. A batch of rewards consists of three parameters:

- A specific token (for example, USDC)

- A period over which the rewards are distributed (for example, from 5th March to 25th August)

- The total amount to be distributed during that period

There can be one, two, or even thousands of parallel rewards within the same period. Consequently, when you claim the rewards, you cannot predict in advance either the type of token you will receive or the associated amounts

You might receive 100 USDC & 1 EGLD, or 150 USDC & 10 HTM, or 50 MEX, etc. Furthermore, when you claim rewards from the incentive & booster contracts, they do not provide any feedback. In this scenario, how can you determine which tokens to deposit to enhance the users' lending positions?

A naive approach would be to assume that you will receive USDC rewards. Then, you compare your balance for this token before and after claiming the rewards. For instance, if you had 1000 USDC just before claiming and 1500 USDC immediately afterward, you can deduce that you received 500 USDC in rewards

However, this method relies on a strong assumption: that the rewards are solely in USDC and will never vary. Is there any guarantee of this? Absolutely not!

Calculating this for all existing tokens is impractical; it would require a tremendous amount of gas, likely exceeding what the blockchain allows. So, how do we address this issue? There are two possible solutions:

- Request that the Hatom team modify their contracts to include the claimed rewards as part of the execution result

- Find a way to track all the ESDT transfers from the start of the transaction

The first option is less than ideal, as any modification to the Hatom contract requires not only time but also money, given that these contracts are audited to ensure the safety of the Hatom protocol

Therefore, our only viable solution seems to be the second option

3️⃣ The solution provided by the BackTransfers feature

Let's discuss smart contracts briefly. In simple terms, smart contracts are pieces of code deployed on the blockchain. Like any wallet, they have an address, a balance, and can send or receive tokens, among other functionalities

A smart contract can also make a call to another smart contract. While a single smart contract is incredibly useful, the real potential is unlocked when multiple smart contracts are combined

'Composability' refers to the ease with which a smart contract can interact with other smart contracts. Higher composability enables the creation of a richer ecosystem

BackTransfers is a straightforward and native feature of the MultiversX Virtual Machine, introduced a few months ago. It allows a smart contract to retrieve all the payments it received during its last call to another contract

For example, consider a smart contract that aims to swap 100 USDC for USDT. It needs to make a call to a DEX's smart contract, which then executes the swap and returns the USDT. Afterward, the original smart contract can request the BackTransfers from the Virtual Machine, which will inform it, 'During the last contract call, you received 99 USDT'

This feature addresses the issue mentioned earlier! When the autoscale strategy claims rewards, it simply needs to inquire the BackTransfers from the Virtual Machine to know precisely what rewards were received. The Virtual Machine might respond with, 'During the last call, you received 100 USDC and 1 EGLD.'

Conclusion

BackTransfers exemplify the type of features that are straightforward yet significantly enhance composability

They also make contracts safer by eliminating the need for any 'synchronous callback' function, which can lead to reentrancy attacks
