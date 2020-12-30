# ⛓ 📝 Book notes - Mastering Ethereum

## Week 03 - Date: 30/12/2020

### Chapter 04 Cryptography(secret writing)

- all communications with the Ethereum platform and between nodes (including transaction data) are unencrypted and can (necessarily) be read by anyone. This is so everyone can verify the correctness of state updates and consensus can be reached. 
- be used to prove knowledge of a secret without revealing that secret (e.g., with a digital signature), or to prove the authenticity of data (e.g., with digital fingerprints, also known as “hashes”)
- account addresses are derived directly from private keys: a private key uniquely determines a single Ethereum address, also known as an account.
- only account addresses and digital signatures are ever transmitted and stored on the Ethereum system, not the private key
- Ethereum transactions require a valid digital signature to be included in the
  blockchain, to prove owner of the funds

#### Public key cryptography(asymmetric cryptography)
- 1970s by Martin Hellman, Whitfield Diffie, and Ralph Merkl
- based on mathematical functions that have a special property: it is easy to calculate them, but hard to calculate their inverse
- trapdoor functions: very difficult to invert unless you are given a piece of secret information that can be used as a shortcut
- advanced cryptography is based on arithmetic operations on an elliptic curve
- discrete logarithm problem: In elliptic curve arithmetic, multiplication
  modulo a prime is simple but division (the inverse) is practically impossible, currently no known trapdoors
- private key -> create digital signatures -> sign transactions to spend any funds in the account/authenticate owners or users of contracts
- The verification of signature doesn’t involve the private key, anyone can do
  it

#### Private Keys
- Ownership and control of the private key is the root of user control(fund,
  contracts)
- private key must remain secret at all times
- private key must also be backed up and protected from accidental loss.
- a private key can be any nonzero number up to a very large number slightly less than 2^256
- 256-bit number
- usually achieved by feeding an even larger string of random bits (collected from a cryptographically secure source of randomness) into a 256-bit hash algorithm such as Keccak-256 or SHA-256, both of which will conveniently produce a 256-bit number. If the result is within the valid range, we have a suitable private key.
- there are almost enough private keys to give every atom in the universe an Ethereum account. If you pick a private key randomly, there is no conceivable way anyone will ever guess it or pick it themselves.

#### Public Keys
- An Ethereum public key is a point on an elliptic curve, meaning it is a set of x and y coordinates that satisfy the elliptic curve equation.
- an Ethereum public key is two numbers, joined together. These numbers are produced from the private key by a calculation that can only go one way. That means that it is trivial to calculate a public key if you have the private key, but you cannot calculate the private key from the public key.
- Elliptic curve multiplication is a type of function that cryptographers call a “one-way” function: it is easy to do in one direction (multiplication) and impossible to do in the reverse direction (division). -> private key can easily create the public key and then share it with the world, knowing that no one can reverse the function and calculate the private key from the public key -> the basis for unforgeable and secure digital signatures that prove ownership of Ethereum funds and control of contracts.
- Elliptic Curve Cryptography: discrete logarithm problem as expressed by addition and multiplication on the points of an elliptic curve
- secp256k1 standard
- Starting with a private key in the form of a randomly generated number k, we multiply it by a predetermined point on the curve called the generator point G to produce another point somewhere else on the curve, which is the corresponding public key K
- ETH only uses uncompressed public keys; therefore the only prefix is (hex) 04.
- `04 + x-coordinate (32 bytes/64 hex) + y-coordinate (32 bytes/64 hex)`

#### Cryptographic Hash Functions
- hash functions are part of the transformation of Ethereum public keys into addresses
- can also be used to create digital fingerprints, which aid in the verification of data
- hash function: any function that can be used to map data of arbitrary size to data of fixed size.
- input: pre-image, the message, or simply the input data
- output: hash
- Cryptographic hash functions subset to hash functions: a one-way hash function that maps data of arbitrary size to a fixed-size string of bits
- hash functions are “many-to-one” functions
- Finding two sets of input data that hash to the same output is called finding a hash collision
- the better the hash function, the rarer hash collisions are. For Ethereum, they are effectively impossible
- Properties of Cryptographic Hash Functions: Determinism, Verifiability,
  Noncorrelation, Irreversibility, Collision protection(particularly important for avoiding digital signature forgery in Ethereum)

#### Keccak-256
- Ethereum uses the Keccak-256 cryptographic hash function
- Keccak-256 was designed as a candidate for the SHA-3 Cryptographic Hash Function
- Edward Snowden, NIST, NSA, weaken the Dual_EC_DRBG random-number generator
  standard, backdoor, SHA-3 not trust worthy
- use test vector empty input to determines which hash standard

#### Ethereum Addresses(from public key use Keccak-256)
- Ethereum addresses are hexadecimal numbers, identifiers derived from the last 20 bytes of the Keccak-256 hash of the public key.
- private key -> public key(x and y coordinates of private key concatenated shown as hex) -> Keccak-256 hashed public key -> Ethereum address(keep only the last 20 bytes, least significant bytes) -> often with the prefix 0x that indicates they are hexadecimal-encoded
- Ethereum addresses are presented as raw hexadecimal without any checksum.

#### Ethereum Address Formats(a issue in reality)
- Inter exchange Client Address Protocol (ICAP) partly compatible with the International Bank Account Number (IBAN) encoding
- An IBAN consists of a string of up to 34 alphanumeric characters (case- insensitive) comprising a country code, checksum, and bank account identifier (which is country-specific).
- ICAP uses the same structure by introducing a nonstandard country code, “XE,” that stands for “Ethereum,” followed by a two-character checksum and three possible variations of an account identifier
- three possible variations of an account identifier: Direct(it only works for Ethereum addresses that start with one or more zero bytes, compatible with IBAN, in terms of the field length and checksum), Basic(31 characters long, This allows it to encode any Ethereum address, but makes it incompatible with IBAN field validation), Indirect
- At this time, ICAP is unfortunately only supported by a few wallets.
- Hex Encoding with Checksum in Capitalization (EIP-55)
- Wallets that do not support EIP- 55 checksums simply ignore the fact that the address contains mixed capitalization, but those that do support it can validate it and detect errors with a 99.986% accuracy.
- Keccak-256 hash of the lowercase hexadecimal address -> Capitalize each
  alphabetic address character if the corresponding hex digit of the hash is
  greater than or equal to 0x8 -> when use converts it to lowercase, and calculates the checksum hash
