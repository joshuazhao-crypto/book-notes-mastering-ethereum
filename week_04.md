# ⛓ 📝 Book notes - Mastering Ethereum

## Week 04 - Date: 06/01/2021

### Chapter 05 Wallet

- At a high level, a wallet is a software application that serves as the primary user inter‐ face to Ethereum
- The wallet controls access to a user’s money, managing keys and addresses, tracking the balance, and creating and signing transactions. In addition, some Ethereum wallets can also interact with contracts
- Every wallet has a key-management component
- in fact, very strictly speaking, the wallet holds only keys. The ether or other tokens are recorded on the Ethereum blockchain.
- In a sense, an Ether‐ eum wallet is a keychain
- Ethereum wallets contain keys, not ether or tokens. Wallets are like keychains containing pairs of private and public keys. Users sign transactions with the private keys, thereby proving they own the ether. The ether is stored on the blockchain.

####  Two primary types of wallets
- a nondeterministic wallet
  - where each key is independently generated from a different random number. The keys are not related to each other. This type of wallet is also known as a JBOK wallet, from the phrase “Just a Bunch of Keys.”
- second type of wallet is a deterministic wallet
  - all the keys are derived from a single master key, known as the seed. All the keys in this type of wallet are related to each other and can be generated again if one has the original seed
  - The most com‐ monly used derivation method uses a tree-like structure, as described in “Hierarchical Deterministic Wallets (BIP-32/BIP-44)
- mnemonic code words: To make deterministic wallets slightly more secure against data-loss accidents, such as having your phone stolen or dropping it in the toilet, the seeds are often encoded as a list of words
- The “type 0” nondeterministic wallets are the hardest to deal with, because they create a new wallet file for every new address in a “just in time” manner.
- many Ethereum clients (including geth) use a keystore file, which is a JSON-encoded file that contains a single (randomly generated) private key, encrypted by a passphrase for extra security
- The keystore format uses a key derivation function (KDF)
- In simple terms, the private key is not encrypted by the passphrase directly. Instead, the passphrase is stretched, by repeatedly hashing it. The hashing function is repeated for 262,144 rounds, which can be seen in the keystore JSON as the parameter crypto.kdfparams.n. An attacker trying to brute-force the passphrase would have to apply 262,144 rounds of hashing for every attempted passphrase, which slows down the attack sufficiently to make it infeasible for passphrases of suffi‐ cient complexity and length.
- industry-standard–based HD wallet with a mnemonic seed
- In a deterministic wallet, the seed is sufficient to recover all the derived keys, and therefore a single backup, at creation time, is sufficient to secure all the funds and smart contracts in the wallet.
- The seed is also sufficient for a wallet export or import, allowing for easy migration of all the keys between different wallet implementations.

#### Hierarchical Deterministic Wallets (BIP-32/BIP-44)
- Currently, the most advanced form of deterministic wallet is the hierarchical deterministic (HD) wallet defined by Bitcoin’s BIP-32 standard. 
- Advantages:
  - First, the tree structure can be used to express additional organizational meaning
  - The second advantage of HD wallets is that users can create a sequence of public keys without having access to the corresponding private keys. This allows HD wallets to be used on an insecure server or in a watch-only or receive-only capacity, where the wal‐ let doesn’t have the private keys that can spend the funds.
- Seeds and Mnemonic Codes (BIP-39)
- mnemonic, and the approach has been standardized by BIP-39 encode private key
- the use of a recovery word list to encode the seed for an HD wallet makes for the easiest way to safely export, transcribe, record on paper, read without error, and import a private key set into another wallet.

#### Wallet Best Practices
- BIP-39 has now achieved broad industry support across dozens of interoperable implementations and should be considered the de facto industry standard. Furthermore, BIP-39 can be used to produce multicurrency wal‐ lets supporting Ethereum, whereas Electrum seeds cannot.
- The wallet starts from a source of entropy, adds a check‐ sum, and then maps the entropy to a word list:
- The mnemonic words represent entropy with a length of 128 to 256 bits. The entropy is then used to derive a longer (512-bit) seed through the use of the key-stretching function PBKDF2. The seed produced is used to build a deterministic wallet and derive its keys.
- the mnemonic and a salt.
- The purpose of a salt in a key-stretching function is to make it difficult to build a lookup table enabling a brute-force attack. 
- In the BIP-39 standard, the salt has another purpose: it allows the introduction of a passphrase that serves as an additional security factor
- form of plausible deniability or “duress wallet,” where a chosen passphrase leads to a wallet with a small amount of funds, used to distract an attacker from the “real” wallet that contains the majority of funds.
- If the wallet owner is incapacitated or dead and no one else knows the pass‐ phrase, the seed is useless and all the funds stored in the wallet are lost forever.
- Conversely, if the owner backs up the passphrase in the same place as the seed, it defeats the purpose of a second factor.

#### HD Wallets (BIP-32) and Paths (BIP-43/44)
- A very useful characteristic of HD wallets is the ability to derive child public keys from parent public keys, without having the private keys. This gives us two ways to derive a child public key: either directly from the child private key, or from the parent public key.
- This shortcut can be used to create very secure public key–only deployments
- That kind of deployment can produce an infinite number of public keys and Ethereum addresses, but cannot spend any of the money sent to those addresses. Meanwhile, on another, more secure server, the extended private key can derive all the corresponding private keys to sign transactions and spend the money.
- The web server can use the public key derivation function to create a new Ethereum address for every transaction
- Another common application of this solution is for cold-storage or hardware wallets.
- Hardened child key derivation
- extended public key, or xpub
- The hardened derivation function uses the parent private key to derive the child chain code, instead of the parent public key. 
- if you want to use the convenience of an xpub to derive branches of public keys without exposing yourself to the risk of a leaked chain code, you should derive it from a hardened parent, rather than a normal parent. Best practice is to have the level-1 children of the master keys always derived by hardened derivation, to pre‐ vent compromise of the master keys.
- the tree can be as deep as you want, with a potentially infinite number of generations. With all that potential, it can become quite difficult to navigate these very large trees.
- BIP-44 specifies the structure as consisting of five predefined tree levels:
- m / purpose' / coin_type' / account' / change / address_index
- Only the “receive” path is used in Ethereum, as there is no necessity for a change address like there is in Bitcoin. 
