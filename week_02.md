# ⛓ 📝 Book notes - Mastering Ethereum

## Week 02 - Date: 23/12/2020

### Chapter 03 Ethereum Clients 

#### what is a Blockchain
  - Ethereum Networks
    - A full node running on a live mainnet network is not necessary for Ethereum development.
  - different way of running:
    - live mainnet
    - a testnet
    - a remote client

  - Pros and Cons for different connection method:
    | Tables                | Pros                   | Cons            |
    | --------------------- |:----------------------| :---------------|
    | **Mainnet**          | - Supports the resilience and censorship resistance of Ethereum-based networks <br> - Authoritatively validates all transactions <br> - Can interact with any contract on the public blockchain without an intermediary <br> - Can directly deploy contracts into the public blockchain without an intermediary <br> - Can query (read-only) the blockchain status (accounts, contracts, etc.) offline <br> - Can query the blockchain without letting a third party know the information you’re reading      | -Requires significant and growing hardware and bandwidth resources <br> -May require several days to fully sync when first started <br> -Must be maintained, upgraded, and kept online to remain synced          |
    | **Public Testnet**          | -A testnet node needs to sync and store much less data<br> -sync quickly <br> -Deploying contracts or making transactions requires test ether, which has no value and can be acquired for free from several “faucets.” <br> -Testnets are public blockchains with many other users and contracts, running “live.”           |   -can’t use “real” money on a testnet; it runs on test ether.<br> -There are some aspects of a public blockchain that you cannot test realistically on a testnet. For example, transaction fees           |
    | **Simulation**    | -No syncing and almost no data on disk; you mine the first block yourself<br> -No need to obtain test ether; you “award” yourself mining rewards that you can use for testing <br> -No other users, just you <br> -No other contracts, just the ones you deploy after you launch it              |    -Having no other users means that it doesn’t behave the same as a public blockchain. There’s no competition for transaction space or sequencing of transactions.<br> -No miners other than you means that mining is more predictable; therefore, you can’t test some scenarios that occur on a public blockchain. <br> -Having no other contracts means you have to deploy everything that you want to test, including dependencies and contract libraries. <br> -You can’t recreate some of the public contracts and their addresses to test some scenarios (e.g., the DAO contract).           |
