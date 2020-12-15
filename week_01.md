# Book notes - Mastering Ethereum

## Week 01/02 - Date: 15/12/2020

### Chapter 01 Intro

#### what is a Blockchain
  - components(open public one):
    ```
    1. a network layer: p2p based "gossip" protocol
    2. messages, as transactions, represent state transitions
    3. a set of consensus rules
    4. a state machine process 2. based on 3. through 1.
    5. a chain of blocks(cryptographically secure), as journal of good states(accepted n verfied),
       db-ish
    6. a consensus algorithm, enforce 3. to every participates, govern
    7. a incentivization scheme(game-theoretically sound), economically secure,
       secure
    8. clients
    ```
  - buzzwords looking for in a good blockchain:
    - `open, public, global, decentralized, neutral, and censorship-resistant`
  - from blockchain to Dapp, web3

#### what is Ethereum
  - `“the world computer”`
  - from CS:
    - `a deterministic but practically unbounded state machine, consisting of a globally accessible singleton state and a virtual machine that applies changes to that state.`
  - from practical:
    - `an open source, globally decentralized computing infrastructure that executes programs called smart contracts.`
  - enables(buzzwords):
    - `build Dapps with built-in economic functions. high availability, auditability, transparency, neutrality, reduces or eliminates censorship, reduces certain counterparty risks.`
  - components(try to compare this with a open public blockchain listed below):
    ```
    1. a network layer: p2p, Ethereum main network, TCP port 30303, ÐΞVp2p
       protocol
    2. transactions, messages, include (among other things) a sender, recipient, value, and data payload
    3. a set of consensus rules, check the Yellow Paper
    4. a state machine, processed by the Ethereum Virtual Machine (EVM)
       db-ish
    5. data structures: Merkle Patricia Tree, stored in local node, hashed and
       serialized
    6. a consensus algorithm: Nakamoto Consensus, same to Bitcoin, at the time
       weighted in importance by PoW, moving to PoS weighted voting, Casper
    7. a PoW algorithm called Ethash, will to PoS
    8. clients, like Geth(Golang)/Parity(Rust)
    ```
  - PoW: ¯\\\_(ツ)_/¯
  - PoS: ¯\\\_(ツ)_/¯
  - Turing complete for Ethereum(back to cs101, or just watched [The Imitation Game](https://www.imdb.com/title/tt2084970/) lol)

    - definition: Turing complete if it can be used to simulate any Turing machine.
    - halting problem
      - given an arbitrary program and its input, to determine whether the program will eventually stop running?(No~)
      - or just watch [Halt and Catch Fire](https://www.imdb.com/title/tt2543312/) lmao
    - **what:** Ethereum can compute any algorithm that can be computed by any Turing machine, given the limitations of finite memory.
    - **how:** EVM with reading and writing to RAMish thingy(state transitions in kv
      pair)
    - **why:**
      ```
      1. combine the general-purpose computing architecture of a stored-program computer with a
      decentralized blockchain, thereby creating a distributed single-state (singleton) world computer
      2. run anywhere
      3. universal state
      4. secure by consensus
      ```
    - Issues:

      - flexibility may bring security issues: halting problem, cannot predict the path of a program without running it
      - Ethereum: DoS attack, a smart contract can be created such that it runs forever when a node attempts to validate it
      - solution: gas, metering mechanism, upper limit of what can be consumed
        when running a smart contract
      - ***gas*** can only be purchased as part of a transaction, and can only be bought with ether.

#### Ethereum compares to Bitcoin:
  - blockchain gen 1.0 vs gen 2.0
  - not primarily to be a digital currency payment network that tracking only the state of currency ownership(Bitcoin), Ethereum tracks the state transitions of a general-purpose data store(in kv pair, similar to RAM)
  - general purpose(since Ethereum is Turing complete), with EVM

#### Ethereum compares to conventional general purpose computers:
  ```
  1. state changes are governed by the rules of consensus
  2. the state is distributed globally
  ```

#### Dapps and web3
  - Decentralized Applications
  - A DApp is, at the very least, a smart contract and a web user interface.
  - More broadly, a DApp is a web application that is built on top of open, decentralized, peer-to-peer
  - A decentralized (P2P) storage protocol and platform(optional), Swarm for
    Ethereum
  - A decentralized (P2P) messaging protocol and platform(optional), Whisper for
    Ethereum Ethereum
  - Ethereum web3.js JavaScript library
  - web3 coined by Dr. Gavin Wood

#### History of Ethereum and Stages of Development
  ```
  - started about 2013
  - July 30, 2015, the first Ethereum block was mined.
  - creators n main contributors: Vitalik Buterin, Andreas M. Antonopoulos
  - 4 main stages:
    - Frontier, Homestead, Metropolis, and Serenity
  - intermediate hard forks:
    - Ice Age, DAO, Tangerine Whistle, Spurious Dragon, Byzantium, and Constantinople
  ```

#### Ethereum Culture
  ```
  move fast and break things
  ```
  - remain flexible and be prepared to rebuild your infrastructure as some of the underlying assumptions change.
  - prepared to deploy new ones, migrate users, apps, and funds, and start over.

### Chapter 02 Basic

#### Ether
  - what:
  - unit: wei and such
#### a practical intro to Wallet, more on the topic when we finished Chapter 05 Wallet
  - simple explain what is a wallet
  - list of wallet on the market
  - control and responsibility with tips
  - hands on MetaMask
    - first encounter of mnemonic, address and other concept
    - network choices: Main net, assorted test nets, localhost 8545, custom RPC
    - how to send Ether
    - what is gas, more will come in detail at Chapter 06 Transaction
#### Ethereum as the world computer
  - EVM Ethereum Virtual Machine
    - Solidity
  - EOA Externally Owned Account
    ```
    have a private key; having the private key means control over access to funds or contracts.
    ```
  - smart contract: `the software program recorded on the Ethereum blockchain at the contract account’s creation and executed by the EVM.`
  - Contract Account
    ```
    1. has smart contract code, which a simple EOA can’t have.
    2. does not have a private key, cannot initiate a transaction.
    3. owned (and controlled) by the logic of its smart contract code:
    4. Only EOAs can initiate transactions, but contracts can react to transactions by
      calling other contracts, building complex execution paths.
    ```
  - typical usage:
    ```
    an EOA sending a request transaction to a multisignature smart contract wallet to send some ETH on to another address.
    ```
  - an typical Dapp pattern: 
    ```
    Contract A calling Contract B in order to maintain a shared state across users of Contract A
    ```
  - Faucet
  - creating contract
  - interacting contract
  - good summarization:
    ```
    Ethereum contracts are programs that control money, which run inside a virtual machine called the
    EVM. They are created by a special transaction that submits their bytecode to be recorded on the
    blockchain. Once they are created on the blockchain, they have an Ethereum address, just like
    wallets. Anytime someone sends a transaction to a contract address it causes the contract to run
    in the EVM, with the transaction as its input. Transactions sent to contract addresses may have
    ether or data or both. If they contain ether, it is “deposited” to the contract balance. If they
    contain data, the data can specify a named function in the contract and call it, passing arguments
    to the function.
    ```
