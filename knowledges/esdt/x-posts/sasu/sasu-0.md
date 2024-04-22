SOURCE: https://x.com/SasuRobert/status/1757496948444016681

---
The true and complete way to fractionalised Ownership - a developer's guide

Let’s break free from ERCs and build it properly on #MultiversX
#

1⃣ $ESDT is the token standard which gives the users full ownership of the tokens. It is highly optimised, can have sophisticated mechanics, can be extended and it is seamlessly composable, backward and forward compatible. Nothing close to it exist.

There are already a set of primitives as 
@MultiversX
 improvement proposal presented in Agora related to NFTs and coded as Smart Contract modules:
🔹ComposableNFTs/SFTs: equipping. The goal is to equip NFTs/SFTs with other NFTs/SFTs. Make a merge of tokens which is still kept by the user, safe for everyone, forward and backward compatible.
🔹FractionalNFTs v1: a new contract through which ownership of one NFT or a merged/composed NFT can be separated into fractions. Actually creating a metaESDT with a quantity of X and the original NFT can be obtained back only if all quantities are sent to the contract and an unFractal endpoint is called.
🔹DynamicNFTs: it represents the idea that through special roles a set of attributes of the NFT/SFT/metaESDT can be changed at any time, without affecting the balances of the user.

Problem statement:
🔶But even if we fractionalise NFTs, you do not have one fungible token which can be easily traded on a DEX, you have one type of metaESDT which has quantity and nonce. So for a collection of 10.000 pieces which you fractionalise you have 10.000 different nonces for the created metaESDTs.

We can improve on this, while keeping all the ESDT primitives intact. Not like the overly hyped #ERC404. But why does this matter? What can one or more collections with the combination of a fungible token enhance volume, usage, gaming, ownership of collectibles and even RWAs.

Usecases:
▶️In games we tend to collect some points and we can exchange those points into some new elements. Also it is possible to exchange back any time the won pieces to a part of the initial points.
▶️You want to trade/own only a part of a big real estate, or an important piece of NFT. But the collection has countless NFTs and having one unified liquidity is always better for the price.
▶️One could create a basket of multiple DeFi positions, create a fungible token representing a share of that basket and trade freely the fungible token + integrate it in all the DeFi apps. Making it forward and backward compatible.
▶️One DAO could create a collection of rare and high quality art/NFTs from different collections  and the member could own a share of that through the whole collection being represented by the fungible tokens. Internally in the DAO the price could be set for each NFT in the launched fungible token.
▶️MetaVerse: this can be used to allow conglomerates/groups of individuals to come together to buy virtual lands or combined digital assets in the virtual worlds.

The idea is fractionalisation is great, and with great tech this can open up new markets and opportunities for both builders, investors and users.

Definitions:
🟡 fractalToken: represents the fungible ESDT. Through fractalTokens the user can claim from the basket of Goods an element if he has enough for that.
🟡 BasketOfGoods: represent one or more collections of NFTs/SFTs/MetaESDTs which can be exchanged for the fractalToken inside the Smart Contract.
 
Specifications of contract:
🔹It is actually pretty simple and we need only one SmartContract. No need for transferRoles, but we create a simple exchange between the fractalToken and the BasketOfGoods with an internal price written in fractalToken. As there is no other limitation, both the fractalToken and the elements from the BasketOfGoods can be freely traded in DEXs or in NFT marketplaces. Like the fractal token can be added to all DeFi protocols.

The setup phase:
🔹setInternalPriceFor@tokenID@nonce@value: this will create a map of storage where the internal price in the contract is set between the fractal token and the given basketOfGood. This means that users will be able to buy a specific NFT for the given price.

🔹setInternalPriceForCollection@tokenID@value: this will set the price in fractalToken for all the collection, same price for all the elements under the given collection.

🔹setFeeForFractionalisingNFTs@tokenID@nonce@value / setFeeForFactionalisingCollection@tokenID: through this the contract owner can set a burn fee for fractal tokens, when someone wants to claim an NFT, if he wants.

🔹setFeeForDepositBaskedOfGoods@percentage: sets a fee for the deposit basket goods if needed.

The endpoints:
🔹depositBasketOfGoods: this receives a list of MultiESDTNFTTransfer, validates each of the tokenIDs and registers those into the internal storage. According to the price set initially, the depositor will receive the fractalTokens. This endpoint can be further enhanced by sending the resulting fractal tokens to multiple users. A fee might be reserved and the resulting fractal tokens can be deposited into a separate small pool, later to be redistributed for holders.

🔹claimBasketOfGoods@List<tokenID@nonce@quantity> / claimBasketOfGoodRandom: The user will send the fractalTokens and claim a definite NFT or a random one, depending on how the contract is set up.

That’s it. It is simple, yet efficient. The resulting fractalToken can be treated as global liquidity and to be listed directly into any DEX, opening up interesting market dynamics. Forward and backward compatible. Composable. As any great standard and Smart Contract should be.

TransferRole version:
If you want to make NFTs to appear automagically for the user once it goes over the threshold of 10.000 pieces of fungible token, we can make the following:
🔹Set the transferRole for the fungible token to the SmartContract and ensure all transactions have to go through this contract. Register the balances of users in the contract.
🔹Every interaction for those tokens will have to go through the transferRole contract so we can precompute and resolve automatic mint/burn or deposit/send of NFTs according to a set of rules.
So when a user balance is >10K pieces of tokens we can send the user an NFT from the list of the remaining NFTs.
🔹When a user has an NFT and wants to share a part of it, it will send to the SC, the SC deposits it, creates the 10.000 tokens instead of the NFT, sell a required part of it in the DEX and register to balance the rest.
🔹When a user wants to list the NFT to a marketplace, he has to come through the transfer SC with both the NFT and the 10.000 tokens. The 10.000 tokens will be burnt and the NFT is listed to the marketplace.
🔹If a user sends tokens to another user and his balance gets <10.000 tokens, he needs to send his NFT as well in order for the call to go through. But you see you can choose which NFTs you want to send, Those you want to keep will remain in your account.

1- Let’s state some obvious facts: #ERC404 standard is not well thought out, it has multiple issues regarding user interactions, user safety, funds and NFT safety. It is also hardly composable, only new contracts can integrate it, while checking the inner details of the code to not make any mistake. Users can lose their precious rare NFTs, also they could re-mint until they get only the super rare NFTs. It is not intuitive for any frontend what will happen with users' funds or NFTs in case of an action.
No standard should be launched which has technical inaccuracies. 

2- Now there has been a technical improvement on this by a group of #ETH developers. @0xCygaar, @0xQuit
 tweeted mostly about this. The new standard is called #DN404 to combine both #ERC721 and #ERC20 functionalities into one, basic logic being separated into separate contracts and transferFrom redesigned in the ERC721 interface. Still 404 remains as more of a gimmick than anything else.

3- Overall we still see daily day phishing attacks, over 55M$ lost only in January, over 1Billion$ since 2017. When you keep your tokens in ERC, you are actually keeping your money in a safety deposit box, to which you added keys and approved others to open it. The approval function or increase allowance transaction means giving a key for the other contract/user to use your funds. This is not safe. Digital tokens have to be safer than cash, and also have to have all of its primitives. This is why ESDT exists. Tokens are in the user’s balance, there is no approval function, everything is transferAndExecute. 

As always #itistimetobuild , it is time to innovate.

The best place to innovate in #blockchain is #MultiversX
