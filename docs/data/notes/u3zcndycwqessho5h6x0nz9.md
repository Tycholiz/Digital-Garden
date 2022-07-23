
A coin is a chain of transaction records, where each record represents each time an owner transferred the coin to a new owner as payment. 
- The latest transaction record in the chain shows the coin's current owner.

Each coin owner has a [[public/private key|crypt.public-key]] pair which the network uses to verify the integrity of transactions.

When the current owner of a coin wants to transfer the coin to a new owner, they create a transaction record which contains:
- The public key of the coin's new owner.
- A hash of the previous transaction record in the chain.
- A signature of the above hash signed with the current owner's private key.

This information allows the new owner to verify that it received the coin from the right owner.

## The Bitcoin Blockchain
Each block of the [[blockchain|blockchain]] is identified with a [[hash|crypt.hashing]] and contains:
- The hash of the previous block in the chain.
- A set of transactions.
- A nonce

When you make a payment, the payee won't accept it until the transaction is in the blockchain. 
- since all transactions are in the blockchain, the payee will find out if the coin has been spent before.

### Adding a block to the blockchain (ie. mining)
When a peer receives new transactions, it collects them into a block. Before it can add the block to the blockchain, it needs to do actual CPU work (called *proof-of-work*).

The process of finding the *nonce* is calling *mining*

It takes 10 minutes on average for a peer to mine a new block, which means that the parties involved in a transaction have to wait for about 10 minutes before it appears on the blockchain.
- A peer broadcasts a block to all peers after finding its proof of work. 

# E Resources
- https://timilearning.com/posts/mit-6.824/lecture-19-bitcoin/