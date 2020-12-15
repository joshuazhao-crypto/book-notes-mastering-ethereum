# Book notes - Mastering Ethererum

## Week 01/02 - Date: 15/12/2020

### Chapter 01 Intro

- what is Ethererum
  - `“the world computer”`
  - from CS:
    - `a deterministic but practically unbounded state machine, consisting of a globally accessible singleton state and a virtual machine that applies changes to that state.`
  - from practical:
    - `an open source, globally decentralized computing infrastructure that executes programs called smart contracts.`
  - enables(buzzwords):
    - `build Dapps with built-in economic functions. high availability, auditability, transparency, neutrality, reduces or eliminates censorship, reduces certain counterparty risks.`
  - components:
  - Turning complete
    - what:
    - how:
    - why:
- Compare to Bitcoin
  - brief touch on the difference between Ethererum and Bitcoin, blockchain gen 1.0 vs gen
    2.0
- what is a Blockchain
  - components(open public one):
    ```
    1. a network layer: p2p based "gossip" protocol
    2. messages, as transactions, represent state transitions
    3. a set of consensus rules
    4. a state machine process 2. based on 3. through 1.
    5. a chain of blocks(cryptographically secure), as journal of good states(accepted n verfied), db-ish
    6. a consensus algorithm, enforce 3. to every participates, govern
    7. a incentivization scheme(game-theoretically sound), economically secure,
       secure
    8. clients
    ```
  - `open, public, global, decentralized, neutral, and censorship-resistant`
  - from blockchain to Dapp
  - web3
- History of Ethererum and Stages of Development
- Eth Culture

### Chapter 02 Basic

- Ether
  - definition:
  - unit:
- a practical intro to Wallet, more on the topic when we finished Chapter 05 Wallet
  - simple explain what is a wallet
  - list of wallet on the market
  - control and responsibility with tips
  - hands on MetaMask
    - first encounter of mnemonic, address and other concept
    - network choices: Main net, assorted test nets, localhost 8545, custom RPC
    - how to send Ether
    - what is gas, more will come in detail at Chapter 06 Transaction
- Ethererum as the world computer
  - EVM Ethereum Virtual Machine
    - Solidity
  - EOA Externally Owned Account
  - Contract
  - Faucet
  - creating contract
  - interacting contract
