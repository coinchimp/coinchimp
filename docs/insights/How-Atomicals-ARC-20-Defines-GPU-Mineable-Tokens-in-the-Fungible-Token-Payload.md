# How Atomicals ARC-20 Defines GPU Mineable Tokens in the Fungible Token Payload

Atomicals ARC-20 protocol introduces a unique and innovative way to define GPU mineable tokens directly in the fungible token payload. Here’s a breakdown of how it works and how you can leverage this technology to create proof of work (PoW) tokens.

## Understanding the Atomicals Fungible Token Payload

The payload of an Atomicals ARC-20 fungible token includes all the necessary information to define and manage GPU mineable tokens. Here’s how it’s structured:

**1. Defining Proof of Work Requirements:**
In the payload, you can specify the proof of work requirements that need to be met before a token is minted. This is done by encoding specific prefixes or messages into the transaction ID.

**Example Payload Structure:**
```json
{
  "tokenType": "ARC-20",
  "name": "ExampleToken",
  "symbol": "EXT",
  "initialSupply": 1000000,
  "decimals": 18,
  "proofOfWork": {
    "difficulty": "16^4",
    "prefix": "21e8",
    "energyBurn": 65000
  }
}
```

**2. Setting the Difficulty:**
The difficulty parameter determines how challenging it is to mine the token. In the example above, "16^4" represents a difficulty level requiring 65,000 hashes. This ensures that the mining process is feasible on regular desktop computers and smartphones.

**3. Specifying the Prefix:**
The prefix "21e8" is included in the proofOfWork object. This prefix must be present in the transaction ID of the mined token, proving that the required computational effort was made.

**4. Energy Burn Requirement:**
The energyBurn parameter specifies the number of hashes needed to mint the token. For example, 65,000 hashes are required in the example above. This helps throttle spammers and ensures only genuine transactions occur.

### Mining and Creating Tokens

To mine a token, you need to meet the proof of work requirements defined in the payload. Here’s how you can do it:

**Step-by-Step Mining Process:**
1. **Prepare the Payload:**
   Define your token with the desired name, symbol, initial supply, decimals, and proof of work requirements.

2. **Start Mining:**
   Use your GPU to start generating hashes until you find a transaction ID that meets the defined prefix and difficulty.

3. **Validate the Hash:**
   Ensure that the generated transaction ID includes the specified prefix (e.g., "21e8") and meets the difficulty level.

4. **Submit the Transaction:**
   Once the proof of work requirements are met, submit the transaction to the Atomicals network to mint the token.

**Example of Proof of Work Payload:**
```json
{
  "tokenType": "ARC-20",
  "name": "PowToken",
  "symbol": "PWT",
  "initialSupply": 500000,
  "decimals": 18,
  "proofOfWork": {
    "difficulty": "16^6",
    "prefix": "68656C6C6F",
    "energyBurn": 1100000000000
  }
}
```
In this example, the token "PowToken" requires a transaction ID with the prefix "68656C6C6F" (which represents "hello" in hex), and a higher difficulty level requiring approximately 1.1 trillion hashes.

### Advantages of GPU Mineable Tokens

1. **Enhanced Security:**
   The proof of work mechanism ensures that only legitimate transactions are processed, reducing the risk of spam and fraud.

2. **True Decentralization:**
   By requiring computational effort, the system promotes decentralization and prevents manipulation by wealthy entities.

3. **Unique and Rare Tokens:**
   The effort required to mint tokens ensures their rarity and value, making them genuinely unique and hard to replicate.

### Explore the Future of Token Mining

With Atomicals ARC-20, you have the power to create GPU mineable tokens that are secure, decentralized, and unique. By defining proof of work requirements in the token payload, you can ensure that each token represents genuine computational effort and energy burn.

Start mining and creating your own tokens today, and be part of the future of decentralized minting with Atomicals.

## why the use of prefixes is important?

Sure, let's break it down a little more and use our `21e8` we had in the example above.

### "The prefix "21e8" is included in the proofOfWork object."

#### What is a Prefix?
A prefix in this context refers to a specific sequence of characters that must appear at the beginning of a transaction ID. The transaction ID is a unique identifier for a transaction on the blockchain.

#### The ProofOfWork Object
The `proofOfWork` object in the token payload is a section that defines the conditions under which a token can be mined. It includes parameters like difficulty, prefix, and energy burn. Here’s an example:
```json
{
  "proofOfWork": {
    "difficulty": "16^4",
    "prefix": "21e8",
    "energyBurn": 65000
  }
}
```

In this object:
- **difficulty** specifies how challenging it is to find a valid transaction ID.
- **prefix** defines the exact sequence of characters (in this case, "21e8") that must appear at the start of the transaction ID.
- **energyBurn** indicates the number of hashes required to meet the difficulty.

### "This prefix must be present in the transaction ID of the mined token, proving that the required computational effort was made."

#### Why the Prefix Matters
The prefix "21e8" acts as a proof of computational work. Here’s how:

1. **Generating the Transaction ID:**
   When you attempt to mine a token, you need to generate a transaction ID. This is done by hashing data repeatedly until you find a hash that starts with the prefix "21e8".

2. **Hashing Process:**
   Hashing is a process that takes an input (or 'message') and returns a fixed-size string of bytes. Each hash is unique to the input data. In the context of mining, you keep changing the input slightly and rehashing until the output hash starts with the desired prefix.

3. **Proving Computational Effort:**
   Finding a hash that starts with "21e8" is not easy. It involves generating and checking potentially thousands or millions of hashes. The difficulty parameter (e.g., "16^4") indicates how hard this task is. The higher the difficulty, the more hashes you need to generate to find a valid one.

4. **Ensuring Validity:**
   Once you find a transaction ID that starts with "21e8", it proves that you have done the required amount of computational work. This prefix in the transaction ID is a clear indicator that you have met the proof of work criteria specified in the token payload.

#### Example of the Process
Let's say you are mining a token with the following proof of work requirements:
```json
{
  "proofOfWork": {
    "difficulty": "16^4",
    "prefix": "21e8",
    "energyBurn": 65000
  }
}
```

To mine this token:
1. **Start Hashing:**
   You begin by hashing different pieces of data.

2. **Check Each Hash:**
   For each hash you generate, you check if it starts with "21e8".

3. **Continue Until Success:**
   You keep repeating this process, generating new hashes, until you find one that matches the prefix "21e8".

4. **Submit the Transaction:**
   Once you have a valid hash (transaction ID), you submit this transaction to the blockchain. The presence of the prefix "21e8" in the transaction ID proves that you’ve done the required work.

### Importance of the Prefix

- **Spam Prevention:**
  Requiring a specific prefix ensures that spammers cannot easily flood the network with low-effort transactions. They must perform significant computational work, making spamming impractical.

- **Rarity and Value:**
  The difficulty in finding a hash that matches the prefix ensures that mined tokens are rare and valuable, as they represent a tangible amount of computational effort.

- **Security:**
  This mechanism adds a layer of security, as it’s computationally expensive to produce a valid transaction, reducing the likelihood of malicious actors manipulating the system.

## Why would I need the difficulty value for that, Isn't it redundant to give this value with the prefix?

The difficulty parameter and the prefix requirement serve complementary purposes in ensuring the security and integrity of the proof-of-work (PoW) process. Let's break down why both are necessary and how they work together:

### Purpose of the Prefix
The prefix (e.g., "21e8") is a specific sequence of characters that must appear at the beginning of a valid transaction ID. This requirement ensures that any valid transaction ID meets a minimum level of computational effort. Finding a hash with a specific prefix is inherently difficult because it requires generating numerous hashes until one matches the prefix.

### Purpose of the Difficulty Parameter
The difficulty parameter further quantifies the level of computational effort required to find a valid hash. It allows for fine-tuning the mining process by setting an explicit target for the number of leading zeroes (or any other specific condition) in the hash, beyond just matching a prefix.

### How They Work Together
1. **Prefix Requirement:**
   - Ensures that the hash begins with a specific sequence of characters.
   - Acts as a simple yet effective way to enforce a minimum level of work.
   - Makes the mining process transparent and verifiable, as anyone can check if a hash starts with the required prefix.

2. **Difficulty Parameter:**
   - Provides a more granular control over the mining process.
   - Allows the protocol to adjust the mining difficulty dynamically based on network conditions or other factors.
   - Ensures that even if the prefix requirement becomes too easy or predictable, the overall computational effort remains substantial.
   - Facilitates scalability by enabling adjustments to the required effort as needed.

### Example Scenario
Let's say you are mining a token with the following proof-of-work requirements:
```json
{
  "proofOfWork": {
    "difficulty": "16^6",
    "prefix": "21e8",
    "energyBurn": 65000
  }
}
```

- **Prefix ("21e8"):**
  - Requires the first four characters of the hash to be "21e8".
  - This ensures a base level of difficulty, as finding a hash that starts with "21e8" requires generating and checking many hashes.

- **Difficulty ("16^6"):**
  - Specifies an additional requirement that the hash must meet a higher difficulty level, which typically involves having a certain number of leading zeroes.
  - This makes the task exponentially harder, as the miner needs to find a hash that meets both the prefix and the difficulty conditions.

### Why Both Are Needed
1. **Granularity and Control:**
   - The prefix sets a straightforward baseline requirement.
   - The difficulty parameter allows fine-tuning and scaling the computational effort required, providing flexibility and control over the mining process.

2. **Preventing Exploits:**
   - Just relying on a prefix might not be sufficient if miners find a way to shortcut the process.
   - The difficulty parameter ensures that the mining effort remains substantial and cannot be easily gamed.

3. **Adjustability:**
   - Network conditions can change, and the difficulty parameter allows the protocol to adjust the required effort dynamically.
   - This ensures that the mining process remains challenging and secure over time.

## What if I trick the system and I make my miner to skip the difficulty value to find the transaction with the prefix before than others? how this is controlled to not happen?

To ensure that miners cannot bypass the difficulty requirement and simply find a transaction with the prefix, the proof-of-work (PoW) system in protocols like Atomicals ARC-20 includes mechanisms to verify that both the prefix and difficulty requirements are met. Here's how this is controlled:

### Integrated Verification of Prefix and Difficulty

1. **Combined Requirements:**
   - The system requires that a valid hash meets both the prefix and difficulty conditions. A hash that only meets the prefix condition but not the difficulty condition is not considered valid.
   
2. **Hash Verification Process:**
   - After a miner finds a hash with the required prefix, the system checks if this hash also satisfies the difficulty requirement. This involves verifying that the hash has the requisite number of leading zeroes or any other specific condition defined by the difficulty parameter.

3. **Transaction Validation:**
   - When a transaction is submitted, the blockchain nodes perform a full validation, ensuring that the transaction ID meets both the prefix and the difficulty requirements. Only transactions that pass this validation are accepted and added to the blockchain.

### Example of the Validation Process
Let's consider an example where the proof-of-work requirements are as follows:
```json
{
  "proofOfWork": {
    "difficulty": "16^6",
    "prefix": "21e8",
    "energyBurn": 65000
  }
}
```

- **Prefix Requirement ("21e8"):**
  - The hash must start with "21e8".
  
- **Difficulty Requirement ("16^6"):**
  - The hash must also have a certain number of leading zeroes, making it more difficult to find.

When a miner generates a hash:
1. **Hash Generation:**
   - The miner generates hashes until one starts with "21e8".

2. **Difficulty Check:**
   - The system then checks if this hash also meets the difficulty condition (e.g., having six leading zeroes).

3. **Validation:**
   - If both conditions are met, the hash is considered valid.
   - If only the prefix condition is met, the hash is rejected, and the miner must continue searching.

### Preventing Bypass of Difficulty Requirement
1. **Cryptographic Hash Functions:**
   - The use of cryptographic hash functions (like SHA-256) ensures that finding a valid hash is a random process. Miners cannot predict or shortcut this process.
   
2. **Full Node Verification:**
   - Each node in the blockchain network independently verifies the proof-of-work for each transaction. This decentralized verification ensures that no single miner can submit an invalid transaction.
   
3. **Consensus Mechanism:**
   - The consensus mechanism of the blockchain (e.g., Proof of Work) relies on the majority of nodes agreeing on the validity of transactions. This collective validation process prevents any single miner from manipulating the system.

To trick the system, a miner would need to generate a hash that meets both the prefix and difficulty requirements. The difficulty value is integral to the proof-of-work mechanism, and the blockchain nodes rigorously enforce it. This ensures that miners cannot bypass the difficulty requirement, maintaining the integrity and security of the mining process.

## Additional information

[**Bitwork Mining:** Proof of Work CPU/GPU Mineable Tokens](https://github.com/atomicals-community/atomicals-guide/blob/main/bitwork-mining.md)
