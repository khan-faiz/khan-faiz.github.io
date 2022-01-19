
I would just like to briefly discuss three major projects I have been following for awhile now, introduce them and give information about what kinds of projects are possible with these platforms, and hopefully open a broader discussion. My hope here is to give a broad idea of possible funding avenues as well.

All three projects are blockchain-based smart-contract platforms with various differences. But what is a smart contract platform? You are most likely familiar with the smart contract platform Bitcoin, which is a peer-to-peer protocol to mint, transfer and store cryptographically secure bits on what is commonly known as a **blockchain**. 

### Overview of Bitcoin (BTC)
If you aren't familiar with that term, in essence, the core of the Bitcoin protocol is the description of a method that allows a group of users ("miners") to validate certain actions made by other users within the protocol, essentially, coming to an agreement on what actions are valid and how they happened, within a certain security model. 

In a more concrete sense, the first application of the Bitcoin protocol was the creation ("minting") of certain numbers ("bitcoins") which could be sent around the network to other users, giving them balances that would be accurate even when users decided to disconnect from the network. This persistent balance sheet of the network ("the ledger") must be maintained by the network, and the mechanism by which the ledger is secured is through the process of mining blocks. The specifics of the mining algorithm are not truly relevant for our discussion, but I link an article here which does a great job, if anyone is interested in the details ([How does Bitcoin mining work?](https://www.investopedia.com/tech/how-does-bitcoin-mining-work/))

The blockchain then, is the state of the ledger, or balances, and is the prime piece of data that must be protected from attackers or adversaries. The security of this data is ensured by the mining algorithm, which is a part of the Bitcoin protocol. 

But what if the Bitcoin protocol had more functionality than just sending and receiving these secured bits, the bitcoins? In fact, the Bitcoin protocol originally had additional functionality beyond this, but was removed early in the development cycle as it was found to increase the attack surface area by a large margin. It would be several years before a project called Ethereum would launch in order to add back functionality removed from the original Bitcoin protocol.

![](/assets/how_the_bitcoin_blockchain_works.jpg)

### Ethereum (ETH)

Launched in 2014, Ethereum is a blockchain based on similar technologies as Bitcoin with full, Turing complete smart-contract functionality. Essentially, we can view Ethereum as Bitcoin with full smart contract functionality added. 

Specifically, the Ethereum Virtual Machine, or EVM, which is embedded in every Ethereum client, is what validates transactions as they go out on the network. Users craft a certain kind of promise ("Send X to Alice when she interacts with Bob") and the Ethereum network validates these steps and executes them. *In this sense, a smart contract is a much more general attempt at describing a programmable transaction than the send/receive functionality we see in Bitcoin.* In fact, the wide variety of possible transactions makes it a target for hacking, with millions lost from programming mistakes as of 2021 (See: [DAO hack](https://www.coindesk.com/understanding-dao-hack-journalists) ).

While I won't go in depth here, I would be willing to elaborate on finer aspects of the Ethereum protocol in the discussion. It suffices to analogize Ethereum and other smart contract platforms like it as trying to solve the problem of *programmable money* in a safe, secure and scalable way. The most popular smart contracts platform in use today, it has an enormous number of decentralized applications running on top of its chain. The programming language used to write its smart contracts is known as Solidity, a language specific to smart contract creation that resembles Python.

![](/assets/blockchain_fundamentals_screenshot_ethereum-100761608-orig.jpg)

### Cardano (ADA)

Cardano is a relative newcomer, launched in 2017, it aims to introduce *formal verification methods* as one of the linchpins of its future success. While similar in scope to Ethereum, in execution the two platforms are vastly different. Formal verification methods (otherwise known as proof checking programs) are used to verify the security of the Cardano security model, which is based on **proof of stake**, a vastly different mining algorithm to **proof of work**, which Bitcoin uses. The proof of stake algorithm that Cardano uses is provably secure, which essentially implies it has been formalized, written up as a proof, and subsequently has been peer reviewed. 

Cardano aims to launch its platform in stages, and its smart contract functionality *is not yet fully up and running*, however, it aims to complete this by the end of the year. The Plutus smart contract platform is designed and written in Haskell.

A rough idea of the Cardano roadmap is given in the image below (currently Goguen will launch this summer, to give a general idea):
![](/assets/Cardano-Roadmap.png)

### Polkadot (DOT)

The newest of the smart contract competitors, Polkadot's allure is its commitment to scalability. The network launched in May of 2020, it aims to be a base layer which other blockchains connect into. The description can get technical, but essentially Polkadot is a "meta-chain" which allows for secondary blockchains ("relay chains") to lease computation time and thus security from the main network. It is a "chain of chains" in that sense. All governance decisions are made by holders of the DOT token, and the main chain is secured by a variation of proof of stake. Developers who wish to make their own applications then create their own blockchain and plug into the main network as needed. Toolchains for Polkadot are written in Rust. (Further reading: https://polkadot.network/an-updated-overview-of-polkadot/)

![](/assets/polkadot-chain.jpg) 

**I've also included some 'deep reading' links for those who are looking for more info, below.**





### Deep Reading
* Bitcoin
[Bitcoin: A peer to peer electronic cash system](https://bitcoin.org/bitcoin.pdf)
* Ethereum
[What is Ethereum](https://ethereum.org/en/what-is-ethereum/)
* Cardano
[Cardano Roadmap](https://roadmap.cardano.org/)
[Ouroboros: A Provably Secure Proof-of-Stake Blockchain Protocol](https://eprint.iacr.org/2016/889.pdf)
* Polkadot
[Overview of Polkadot and its Design Considerations ](https://eprint.iacr.org/2020/641.pdf)
[Nominated Proof of Stake](https://arxiv.org/abs/2004.12990)
