---
date: 2024-07-17
categories:
  - kaspa
  - krc20
tags:
  - crypto
  - krc20
  - code
  - typescript
  - kaspa
authors:
  - coinchimp
---

# Understanding KRC20 Minting with TypeScript Real Examples: A Step-by-Step Guide

Minting a KRC20 token can seem daunting, especially for newcomers in the crypto world. In this post, we'll walk you through the minting process using TypeScript code pieces as an example. Those code pieces are using [rusty-kaspa WASM](https://kaspa.aspectron.org/nightly/downloads/) recommended by [aspect](https://www.linkedin.com/in/anton-yemelyanov/). We'll also dive into the composition of transactions and discuss some updates on KIP-9, addressing current challenges and solutions.

Special thanks to [KaffinPX](https://x.com/KaffinPX) for sharing this code in the dev channel in[Kaspa Discord server](http://discord.gg/kaspa).

## What is KRC20?

KRC20 is a standard for tokens on the Kaspa network, similar to the BRC20 standard for Bitcoin. It defines a set of rules for tokens, enabling seamless interaction within the Kaspa ecosystem.

## Minting KRC20 Tokens: The TypeScript Example

Our TypeScript app leverages the Kaspa WASM SDK to mint KRC20 tokens. Here's a simplified explanation of the process:

### Step 1: Setup and Create the Script

We create a script that includes the minting data for the KRC20 token:

```typescript
import { ScriptBuilder, Opcodes, PrivateKey, addressFromScriptPublicKey } from "./wasm/kaspa"
import config from "./config.json"

const privateKey = new PrivateKey(config.treasury.privateKey)
const publicKey = privateKey.toPublicKey()
const address = publicKey.toAddress('testnet-11')

const data = { "p": "krc-20", "op": "mint", "tick": "TNACHO" }

const script = new ScriptBuilder()
  .addData(publicKey.toXOnlyPublicKey().toString())
  .addOp(Opcodes.OpCheckSig)
  .addOp(Opcodes.OpFalse)
  .addOp(Opcodes.OpIf)
  .addData(Buffer.from("kasplex"))
  .addI64(0n)
  .addData(Buffer.from(JSON.stringify(data, null, 0)))
  .addOp(Opcodes.OpEndIf)

const P2SHAddress = addressFromScriptPublicKey(script.createPayToScriptHashScript(), 'testnet-11')!
```

### Step 2: Commit Transaction

The commit transaction is the first step in the minting process. This transaction locks funds into a Pay-to-Script-Hash (P2SH) address. The P2SH address is used to ensure that the script must be satisfied before the funds can be spent. This provides an additional layer of security and ensures that the minting operation can only be completed if certain conditions are met. Specifically, it means the funds are locked under a script that dictates how and when they can be spent, protecting against unauthorized or premature usage.

```typescript
const { entries } = await RPC.getUtxosByAddresses({ addresses: [address.toString()] });
const { transactions } = await createTransactions({
  priorityEntries: [],
  entries,
  outputs: [{
    address: P2SHAddress.toString(),
    amount: kaspaToSompi("1")!
  }],
  changeAddress: address.toString(),
  priorityFee: kaspaToSompi("0.1")!,
  networkId: 'testnet-11'
});

for (const transaction of transactions) {
  transaction.sign([privateKey]);
  const hash = await transaction.submit(RPC);
}
```

In this step, we create and sign a transaction that sends 1 Kaspa to the P2SH address. This transaction is submitted to the network, effectively committing to the minting operation.

### Step 3: Reveal Transaction

After the commit transaction is confirmed, we proceed with the reveal transaction. This transaction reveals the minting operation by spending the UTXO created in the commit transaction. The script is included in the reveal process to prove that the conditions for minting the KRC20 token are met.

The `priorityEntries` parameter includes the UTXO from the commit transaction as the highest priority for spending. This ensures that this specific UTXO is used first in the reveal transaction, which is essential because it contains the locked funds that are governed by the script we created.

```typescript
setTimeout(async () => {
  try {
    const { entries } = await RPC.getUtxosByAddresses({ addresses: [address.toString()] });
    const revealUTXOs = await RPC.getUtxosByAddresses({ addresses: [P2SHAddress.toString()] });

    const { transactions } = await createTransactions({
      priorityEntries: [revealUTXOs.entries[0]],
      entries,
      outputs: [],
      changeAddress: address.toString(),
      priorityFee: kaspaToSompi("0.1")!,
      networkId: 'testnet-11'
    });

    for (const transaction of transactions) {
      transaction.sign([privateKey], false);

      const ourOutput = transaction.transaction.inputs.findIndex((input) => input.signatureScript === '');

      if (ourOutput !== -1) {
        const signature = transaction.signInput(ourOutput, privateKey);
        transaction.fillInput(ourOutput, script.encodePayToScriptHashSignatureScript(signature));
      }

      const revealHash = await transaction.submit(RPC);
    }
  } catch (revealError) {
    console.error('Reveal transaction error:', revealError);
  }
}, 20000); // Wait for 20 seconds before attempting to reveal
```

In the reveal transaction, we:
1. Identify the UTXO from the commit transaction.
2. Create a new transaction that spends this UTXO.
3. Include the script to verify that the conditions are met.
4. Sign and submit the transaction.

## Why Use P2SH and Scripts?

Using a P2SH address allows us to lock the funds with a script that defines specific conditions for spending. This provides flexibility and security, ensuring that the minting process can only be completed if the defined conditions are met. The script execution happens when the reveal transaction is processed, verifying the minting conditions before allowing the UTXO to be spent.

## Update on KIP-9 and Ongoing Challenges

[KIP-9](https://github.com/kaspanet/kips/blob/master/kip-0009.md) introduces a mechanism to regulate the growth of the UTXO set, addressing state bloat in the chain. Our TypeScript app utilizes the `createTransactions()` function from Kaspa WASM SDK, which incorporates KIP-9 fee calculations and ensure your app is compatible with the extended mass formula proposed in KIP-9 to regulate the creation and lifecycle of UTXOs. However, two critical features are still in progress:

1. **Dynamic Network Fee Estimation**: This will allow for more accurate priority fee calculations based on current network conditions.
2. **Replace-By-Fee (RBF)**: This feature will enable transaction replacement in the mempool with higher fees, ensuring timely confirmation.

These updates are expected to be available in the coming weeks, enhancing the efficiency and scalability of KRC20 token minting. Additionally, KIP-9 helps to make better block utilization by regulating the growth rate of the UTXO set and optimizing transaction fees, thus reducing state bloat and enhancing overall network performance.