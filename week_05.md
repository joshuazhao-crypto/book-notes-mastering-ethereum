# ⛓ 📝 Book notes - Mastering Ethereum

## Week 05 - Date: 13/01/2021

### Chapter 06 Transactions

- Transactions are signed messages originated by an externally owned account, trans‐ mitted by the Ethereum network, and recorded on the Ethereum blockchain.
- the only things that can trigger a change of state, or cause a contract to execute in the EVM
- Contracts don’t run on their own. Ethereum doesn’t run autonomously. Everything starts with a transaction

#### Structure
- Each client and application that receives a serialized transaction will store it in-memory using its own internal data structure
- The network-serialization is the only standard form of a transaction.
- transaction is a serialized binary message that contains the following data:
  Nonce, Gas price, Gas limit, Recipient, Value, Data, {v,r,s}
- The transaction message’s structure is serialized using the Recursive Length Prefix (RLP) encoding scheme
- simple, byte-perfect data serialization in Ethereum
- All numbers in Ethereum are encoded as big-endian inte‐ gers, of lengths that are multiples of 8 bits
- RLP’s length prefix is used to identify the length of each field. Anything beyond the defined length belongs to the next field in the structure.
- most internal representa‐ tions and user interface visualizations embellish this with additional information, derived from the transaction or from the blockchain.

#### Nonce
- A scalar value equal to the number of transactions sent from this address or, in the case of accounts with associated code, the number of contract-creations made by this account.
- nonce is an attribute of the originating address
- the nonce is not stored explicitly as part of an account’s state on the blockchain. Instead, it is calculated dynamically, by counting the number of confirmed transactions that have originated from an address.
- There are two scenarios of Nonce important:
  1. the usability feature of transactions being included in the order of creation
  2. the vital feature of transaction duplication protection
- use of the nonce is actually vital for an account-based protocol, in contrast to the “Unspent Transaction Output” (UTXO) mechanism of the Bitcoin protocol.
- nonce is an up-to-date count of the number of confirmed (i.e., on-chain) transactions that have originated from an account.
- The nonce is a zero-based counter, meaning the first transaction has nonce 0.
- But in the interim, while there is more than one transaction pending, it might not help us.
- Thereafter, keep track of the nonce in your application until each transaction confirms.
- tracking nonce is important multiple independent processes simultaneously.
- nonce will be stored in the mempool, while the Ethereum network waits for the missing nonce to appear.
- concurrency is when you have simultaneous computation by multiple independent systems.
- Ethereum, by definition, is a system that allows concurrency of operations (nodes, clients, DApps) but enforces a singleton state through consensus.
- In the end, these concurrency problems, on top of the difficulty of tracking account balances and transaction confirmations in independent processes, force most imple‐ mentations toward avoiding concurrency and creating bottlenecks such as a single process handling all withdrawal transactions in an exchange, or setting up multiple hot wallets that can work completely independently for withdrawals and only need to be intermittently rebalanced.

#### Gas
- Gas is the fuel of Ethereum. Gas is not ether
- it’s a separate virtual currency with its own exchange rate against ether
- avoid denial-of-service attacks or inadvertently resource-devouring transactions.
- Gas is separate from ether in order to protect the system from the volatility that might arise along with rapid changes in the value of ether, and also as a way to man‐ age the important and sensitive ratios between the costs of the various resources that gas pays for (namely, computation, memory, and storage).
- he minimum value that gasPrice can be set to is zero, which means a fee-free transaction.
- In simple terms, gasLimit gives the maximum number of units of gas the transaction originator is willing to buy in order to complete the transaction
- If your transaction’s destination address is a contract, then the amount of gas needed can be estimated but cannot be determined with accuracy.
- like a credit account for a gas station company, where you pay after the trip is completed, based on how much gas you actually used.

#### Recipient
- contains a 20-byte Ethereum address. The address can be an EOA or a contract address.
- Ethereum does no further validation of this field
- Ethereum has no way of knowing whether an address was correctly derived from a public key (and therefore from a private key) in existence.
- Sending a transaction to the wrong address will probably burn the ether sent, render‐ ing it forever inaccessible
- EIP-55 Hex Encoding with Checksum
- there are a number of valid reasons for burning ether—for example, as a disincentive to cheating in pay‐ ment channels and other smart contracts—and since the amount of ether is finite, burning ether effectively distributes the value burned to all ether holders (in propor‐ tion to the amount of ether they hold).

#### Value and Data
- The main “payload” of a transaction is contained in two fields: value and data. Transactions can have both value and data, only value, only data, or neither value nor data. All four combinations are valid.
- A transaction with only value is a payment.
- A transaction with only data is an invocation.
- an Ethereum transaction that contains a value, it is the equiva‐ lent of a payment.
- Such transactions behave differently depending on whether the destination address is a contract or not.
- For EOA addresses, or rather for any address that isn’t flagged as a contract on the blockchain, Ethereum will record a state change, adding the value you sent to the bal‐ ance of the address.
- If the destination address (to) is a contract, then the EVM will execute the contract and will attempt to call the function named in the data payload of your transaction
- no data in your transaction, the EVM will call a fallback function
- no data in your transaction, the EVM will call a fallback function
- A contract can reject incoming payments by throwing an exception immediately when a function is called, or as determined by conditions coded in a function
- When your transaction contains data, it is most likely addressed to a contract address.
- the critical difference is that any interpretation of the data pay‐ load by an EOA is not subject to Ethereum’s consensus rules, unlike a contract execution.
- Most contracts use this data more specifically as a function invocation, calling the named function and passing any encoded arguments to the function.
- data payload sent to an ABI-compatible contract
- a hex-serialized encoding of A function selector, The function arguments

#### Contract Creation
- transaction that creates a new contract on the blockchain, deploying it for
  future use. zero address 0x0
- This address represents neither an EOA (there is no corresponding private–public key pair) nor a contract. It can never spend ether or initiate a transaction. It is only used as a destination, with the special mean‐ ing “create this contract.”
- sometimes receives payments from various addresses
- an intentional ether burn, you should make your intention clear to the network
  and use the specially designated burn address instead: `0x000000000000000000000000000000000000dEaD`
- A contract creation transaction need only contain a data payload that contains the compiled bytecode which will create the contract.
- You can include an ether amount in the value field if you want to set the new contract up with a starting balance

#### Digital Signatures
- used to present proof of ownership of a private key without revealing that private key.
- The digital signature algorithm used in Ethereum is the Elliptic Curve Digital Signa‐ ture Algorithm (ECDSA)
- three purposes in Ethereum:
  1. proves that the owner of the private key, who is by implication the owner of an Ethereum account, has authorized the spending of ether, or execution of a contract
  2. guarantees non-repudiation: the proof of authorization is undeniable
  3. proves that the transaction data has not been and cannot be modified by anyone after the transaction has been signed.
- Creating a digital signature
- Verifying the Signature
- one must have the signature (r and s), the serialized transaction, and the public key that corresponds to the private key used to create the signature
- If the x coordinate of the calculated point Q is equal to r, then the verifier can con‐ clude that the signature is valid.
- in verifying the signature, the private key is neither known nor revealed
- sign the Keccak-256 hash of the RLP-serialized transaction data(sign
  transaction)
- To sign a transaction in Ethereum, the originator must:
  ```
    1. Create a transaction data structure, containing nine fields: nonce, gasPrice, gas
    Limit, to, value, data, chainID, 0, 0.
    2. Produce an RLP-encoded serialized message of the transaction data structure.
    3. Compute the Keccak-256 hash of this serialized message.
    4. Compute the ECDSA signature, signing the hash with the originating EOA’s pri‐ vate key.
    5. Append the ECDSA signature’s computed v, r, and s values to the transaction.
  ```
- The special signature variable v indicates two things: the chain ID and the recovery identifier to help the ECDSArecover function check the signature. It is calculated as either one of 27 or 28, or as the chain ID doubled plus 35 or 36.
- At block #2,675,000 Ethereum implemented the “Spurious Dragon” hard fork, which, among other changes, introduced a new signing scheme that includes transaction replay protection (preventing transactions meant for one network being replayed on others) EIP-155
- Raw Transaction Creation with EIP-155 Simple Replay Attack Protection
- a replay-attack- protected transaction encoding, which includes a chain identifier inside the transac‐ tion data, prior to signing
- ensures that transactions created for one blockchain (e.g., the Ethereum main network) are invalid on another blockchain (e.g., Ethereum Classic or the Ropsten test network)
- Therefore, transactions broadcast on one net‐ work cannot be replayed on another, hence the name of the standard.
- EIP-155 adds three fields to the main six fields of the transaction data structure, namely the chain identifier, 0, and 0.
- three fields are added to the transaction data before it is encoded and hashed.

#### Public Key Recovery
- The process of recovering the signer’s public key is called public key recovery.
- To make things more efficient, the transaction signature includes a prefix value v, which tells us which of the two possible R values is the ephemeral public key. If v is even, then R is the correct value. If v is odd, then it is R'. That way, we need to calcu‐ late only one value for R and only one value for K.

#### transmit
- the three steps of creating, signing, and broadcasting a transaction normally happen as a single operation
- can separate
- The most common reason is security.
- The computer that signs a transaction must have unlocked private keys loaded in memory.
- Separating the functions of signing and transmitting and performing them on different machines (on an offline and an online device, respec‐ tively) is called offline signing and is a common security practice.
- air-gapped
- ZeroMQ (0MQ)

#### Transaction Propagation
- Ethereum network uses a “flood routing” protocol.
- Each Ethereum client acts as a node in a peer-to-peer (P2P) network, which (ideally) forms a mesh network. No network node is special: they all act as equal peers.
- The transaction is validated and then transmitted to all the other Ethereum nodes that are directly connected to the origi‐ nating node.
- On average, each Ethereum node maintains connections to at least 13 other nodes, called its neighbors.
- Each neighbor node validates the transaction as soon as they receive it. If they agree that it is valid, they store a copy and propagate it to all their neighbors (except the one it came from)
- flooding across the network, until all nodes in the network have a copy of the transaction. Nodes can filter the messages they propagate, but the default is to propagate all valid transaction messages they receive.
- Within just a few seconds, an Ethereum transaction propagates to all the Ethereum nodes around the globe. From the perspective of each node, it is not possible to discern the origin of the transaction. The neighbor that sent it to the node may be the originator of the transaction or may have received it from one of its neighbors. 
- To be able to track the origins of transactions, or interfere with propagation, an attacker would have to control a significant percentage of all nodes. This is part of the security and privacy design of P2P networks, especially as applied to blockchain networks.

#### Recording on the Blockchain
- some of them are operated by min‐ ers and are feeding transactions and blocks to mining farms, which are computers with high-performance graphics processing units (GPUs)
- The mining computers add transactions to a candidate block and attempt to find a proof of work that makes the candidate block valid
- valid transactions will eventually be included in a block of transactions and, thus, recorded in the Ethereum blockchain
- Once mined into a block, transactions also modify the state of the Ethereum singleton, either by modifying the balance of an account (in the case of a simple payment) or by invoking contracts that change their internal state
- These changes are recorded alongside the transaction, in the form of a transaction receipt, which may also include events.
- A transaction that has completed its journey from creation through signing by an EOA, propagation, and finally mining has changed the state of the singleton and left an indelible mark on the blockchain.

#### Multiple-Signature (Multisig) Transactions
- Ethereum’s basic EOA value transactions have no provisions for multiple signatures
- arbitrary signing restrictions can be enforced by smart contracts with any conditions you can think of, to handle the transfer of ether and tokens alike
- ether has to be transferred to a “wallet contract” that is programmed with the spending rules desired, such as multisignature require‐ ments or spending limits (or combinations of the two)
- The ability to implement multisignature transactions as a smart contract demon‐ strates the flexiblity of Ethereum.
- flexibility can lead to bugs that undermine the security of multisignature schemes.
- There are, in fact, a number of proposals to create a multisignature command in the EVM that removes the need for smart contracts, at least for the simple M-of-N multi‐ signature schemes. This would be equivalent to Bitcoin’s multisignature system, which is part of the core consensus rules and has proven to be robust and secure.

