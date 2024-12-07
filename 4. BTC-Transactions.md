# Block Body

# Transaction




# UTXOs
UTXO: Unspend Transaction Outputs

Every bitcoin transaction creates outputs that can be consumed as inputs in future transactions.
UTXOs are simply the transaction outputs that have not been consumed yet and can still be used for spending.

## Validating transactions

When your node receives a new transaction from the network, it needs to validate that all of its inputs are referencing outputs that have **not already been spent**.

If the transaction's inputs are all unspent outputs (UTXOs), then the transaction is valid.

However, if the transaction is trying to spend an output that has already been spent in a previous transaction, then the transaction is invalid and will be rejected


## Storage

UTXOs is a separate databse that gets stored in memory (RAM), which makes it faster to access than having to trawl through the raw blockchain files to check if an output has been spent or not.
