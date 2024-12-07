# Node

A node is a server running the Bitcoin program that sends, receives, or processs data connected to a network.

It connects to other nodes on the network to share information about transactions and blocks.

## Job

1. Keep a copy of the blockchain.
2. Validate and reply new transactions and blocks.

## Full Node

A full node is a node that can keep up with the blockchain and validate the blocks and transactions it receives. It receives a complete copy of blockchain.

- Archival Node: keeps a full copy of the blockchain. It can replicate the entire blockchain to any new node joining the network.
- Pruned Node: does not keep a full copy of the blockchain, instead, it receive a complete copy of blockchain but it deletes older blocks further down the chain as it goes to save on disk space.

> 存档节点保留区块链的完整副本。 它可以将整个区块链复制到加入网络的任何新节点。
> 修剪后的节点会收到区块链的完整副本，但它会删除链上较旧的区块，以节省磁盘空间。

## Lightweight Node

A lightweight node is a node that can keep up with the blockchain, but it cannot validate the blocks and transactions it receives.

Instead, it can verify that a block or transaction exists in the bloclchain, but it cannot comfirm that they are valid.

A common type of lightweight node is SPV wallet which only receives the block headers of the blockchain.

It can request proof from a full node to comfirm whether a specific transaction is in a specific block

# Requirements

1. **Disk Space**: to store the blockchain. The blockchain also grows at a rate of around 100 GB/year.
2. **RAM**: to store the latest trasactions in the mempool, as well as for storing UTXOs to help speed up the validation of new transactions and blocks.
3. **Bandwidth**: a node is constantly sending and receiving transactions and blocks to and from other nodes on the network, so you will need enough bandwidth to cover this.
memory pool: a waiting area for new transactions.
# Communication

A node communitates other nodes by sending lots of individual messages. These messages are sent via TCP.

## Network Protocol
1. TCP: a common way for two computers on a network to communicate with each other.
2. Bitcoin protocol: a set of rules on the structure and order of messages sent between nodes.

## Magic Bytes

Magic bytes are used as a way to **identify the separate messages** sent between nodes on the Bitcoin network.

Mainnet Network Magic Bytes: `f9beb4d9`
Testnet3 Network Magic Bytes: `0b110907`
Regtest Network Magic Bytes: `fabfb5da`
> 主网、测试网、注册测试使用不同的魔术字节在消息的开头；

This is a raw Tx message on the Mainnet network
```
f9beb4d9747800000000000000000000e00000006a86deb701000000015ac5ae0a2ba96622c9b79de2c339084c8b1d30f63bb55a315f354db4d9a6abcf010000006b4830450221009ad52459e1e8bd5e758399cc0be963c75726c5089499465d9aa79ffb304ecd3802207d73ea58047f4d1f857b400cbff725ef562b7ada1c26e763c5a1aa6d29d2fdf401210234b7b614fcc0e4d926747d491992d8cc133f076bd79095eddf60c34b0e3fef4affffffff02390205000000000017a914ea3b6d7e92e05370bc8a61d3f05dbfdc90bb1d9587d1df3000000000001976a91425f0800454530549ed93747a6449aefe2618203988ac00000000
```

why do we use magic bytes:

there's nothing actually _magical_ about magic bytes, they're just used to help with delimiting a stream of data.
>魔术字节仅仅是为了界定消息边界