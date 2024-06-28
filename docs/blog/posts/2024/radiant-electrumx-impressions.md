---
date: 2024-05-22
categories:
  - radiant
tags:
  - crypto
  - radiant
  - python
  - docker
  - containers
  - electrumx
authors:
  - coinchimp
---

# Exploring the Exciting World of Radiant

I have been a big fan of projects like Kaspa. These chains offer incredible potential, and I have seen a lot of hype surrounding KRC20, which is still in development. People are excited about the possibility of creating meme coins, but I hope it will also attract other projects with more utility.

**However, did you know there are other highly scalable projects?** 

## The Technical Excellence of Radiant Community
Although it has a small community yet, it is very focused on technical excellence. Radiant is very close to completing the [Photonic Wallet](https://github.com/1razoo/photonic-wallet). This wallet will enable the creation of PoW mineable coins, promoting decentralization and fair launches.

![PoW fungible tokens with Radiant Photonic Wallet](/assets/blog/radiant-fungible-pow-tokens-atomicals-photonic-wallet.png)

## Why PoW Fungible Tokens Are Superior?

Proof-of-Work (PoW) fungible tokens offer significant advantages over tokens minted on platforms like Ethereum and Solana. If you're aiming for a project that is everlasting, trustless, fair-launched, and fully decentralized, PoW tokens are the way to go - even for a meme!

1. **Trustless**: With PoW, transactions and token creation are verified by miners who follow strict cryptographic rules. This eliminates the need for trust in a central authority, making the system more secure and reliable.

2. **Fair-Launched**: PoW tokens are distributed through mining, meaning anyone with the right hardware can participate from the start. This contrasts with pre-mined tokens on other platforms, where early insiders often have a significant advantage by managing vesting schedules.

3. **Community-Driven**: The PoW mechanism relies on a network of independent miners spread across the globe. This decentralization reduces the risk of a single point of failure or control, ensuring the network is more resilient and censorship-resistant.

PoW fungible tokens provide a more robust foundation for projects that prioritize security, fairness, and decentralization. They stand out as the best choice for creating a truly democratic and enduring project.

## How it works

Radiant stands out as the only fork or layer 2 project related to Bitcoin that can create this type of coin using atomicals. It also offers the unique Radiant feature of Induction Proofs, allowing tokens to be tracked back to the genesis block without using external indexers, making it fully trustless.

If you want to give it a try, you will need to install your own ElectrumX instance. You can use the publicly available [Docker image](https://hub.docker.com/r/radiantcommunity/electrumx_radiant_node) by running the following command:

```bash
sudo docker run -d -v /home/pinrojas/electrumx_radiant:/data -p 8000:8000 radiantcommunity/electrumx_radiant_node:latest
```

After that, you can clone the same project. I did a fork to add an extra switch to the command [electrumx_rpc](https://github.com/coinchimp/electrumx). Run:

```bash
electrumx_rpc getinfo
```

You should see something like this:

```json
{
    "coin": "Radiant",
    "daemon": "localhost:7332/",
    "daemon height": 197575,
    "db height": 187407,
    "db_flush_count": 21,
    "groups": 1,
    "history cache": "0 lookups 0 hits 0 entries",
    "merkle cache": "0 lookups 0 hits 0 entries"
}
```

If you are not familiar with UTXO, you need to wait for the `daemon height` and `db height` to be the same to ensure your server is in sync. 

Then, clone [Photonic Wallet](https://github.com/1razoo/photonic-wallet) and start playing with your new pizza or pepe-rxd tokens (I am on that phase right now, I will give you more insigths as soon as I can)

Check out [Radiant Blockchain](https://radiantblockchain.org/) for more details.

Cheers!