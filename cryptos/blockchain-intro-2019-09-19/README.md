# D is for Dogecoin
An intro to blockchain and consensus


# Blockchain has been hyped
* Bitcoin
* Cryptocurrencies
* Fraud, Scams, Blackmarket Purchases


# Blockchain & Bitcoin are mysterious
* Simple and powerful ideas with complex mechanisms
* Hard to understand = hand-waving and outlandish claims


# Beneath they hype is something amazing
* PC...Internet...Blockchain?
* Next evolution of technology and civilization
* Finish the vision of the internet
* True decentralization & P2P + elimination of trust
* Organic, powerful, unstoppable


# Goals of This Talk
1. Understand why trust & consensus are immensely critical & immensely fragile today
2. Understand how Bitcoin ledger solves the trust & consensus problem
3. Understand how each block in the Bitcoin blockchain demonstrates Proof of Work
4. Understand how Bitcoin miners use Nakamato Consensus to make the Bitcoin Network a secure, decentralized, and trustless source of truth
5. Understand the difference between a cryptocurrency (ie Bitcoin) and a blockchain
6. Understand why blockchain can solve so many other problems


# What Are Trust & Consensus?
* **trust**: *(noun)* *firm belief in the reliability, truth, ability, or strength of someone or something.*
* **consensus**: *(noun)* *a general agreement.*
* We accept & acknolwedge a consensus due to a trust in the people or process that produced it.


# What Do Trust & Consensus Get Us?
* Just about everything we do depends on trust
* Payments, Contracts
* Insurance, Utility Bills
* Laws & Law Enforcement
* News, TV, Information
* Identity


# Imagine if we couldn't trust...
* Will the company pay me for work?
* Will my pay be worth something?
* Will my money be available?
* Will I have electricity and water?
* Will my food not be poisoned?


# What Establishes Consensus?
* Strongest, most powerful wins
* First person wins
* Popular vote wins
* Best reputation wins
* In science, experimentation & verifiability create consensus (foreshadow)


# Ledgers
* **noun** *a book or other collection of financial accounts of a particular type*
* Give us a **consensus** we agree to **trust**


# Example Ledger - House Deeds
* History of transactions
* Trust the state & gov't to determine consensus


# Example Ledger - Bank
* Balance of Money or Credit
* Trust the bank to maintain consensus of our deposits & transfers


# Trust Can Be Exploited
* What if someone changes a housing record?
* What if a bank says they sent money but vendor says they didn't receive it?
* What if a bank exploits subprime mortages giving them to people who can't pay them back?


# Financial Crisis of 2007-2008
* Banks gave out loans to people who couldn't pay them
* Housing market inflated
* Banks went bankrupt, government stepped to bail out
* Two organizations we "trust" making a mess


# Goal Checkpoint?
1. Understand why trust & consensus are immensely critical & immensely fragile today
2. Understand how Bitcoin ledger solves the trust & consensus problem


# Note on Blockchain & Bitcoin
* These are not the same!
* They were born together, blockchain extracted later
* Blockchain -> Bitcoin == Internet -> Email
* Blockchain -> Bircoin == git repo -> deployed application
* I'll talk about Bitcoin first


# What If Consensus Was Like Science?
* Carbon Dating, Layers of sediment, Rings in a tree
* Instead of trusting people...can we just trust things like electricity, mathematics, or code?
* A consensus that is replayable, verifyable, and unalterable
* (Channel my Einstein) Don't bring up quantum physics please...


# Domo Arigato Mr. Nakamato
* October 2008: Satoshi Nakamato publishes "Bitcoin: A Peer-To-Peer Electronic Cash System" on the cryptography mailing list at metzdowd.com
* January 3, 2009: Bitcoin network comes online with Nakamato mining genesis block with embedded message:
  > The Times 03/Jan/2009 Chancellor on brink of second bailout for banks.
* Mid-2010: Nakamato hands over bitcoin.org & all his source code, and disappears
* Today: Identity still a mystery, the Bitcoin in his wallet untouched ($19B in December 2017)


# Bitcoin Network Hosts Bitcoin Ledger
* The Bitcoin Network manages a public ledger
* **noun** *a book or other collection of financial accounts of a particular type*
* Bank of America manages financial ledger
* Country Records manages Property Title ledger


# What is a Transaction
* Transfer of bitcoin from one address to another
* Public key of BTC owner
* Signature of transaction to prove ownership of public key


# What is a Block?  
* A group of transactions comitted to permanent history
* Nakamato sends 2 BTC to Weebay
* Weebay send 0.5 BTC to Saul
* Nakamato sends 1 BTC to Snuffles
* Ledger: Weebay (1.5 BTC), Saul (0.5 BTC), Snuffles (1 BTC)


# Each Block is Validated By Network
* Bitcoin Network validates transactions (signature & balances)
* These transactions are combined as a variable in a difficult math problem
* Combination of all machines solve math problem
* Input: "Thumbprint" of current transactions (Merkle Hash)
* Input: Randon Number (nonce)
* Input: "Thumbprint" of previous block (Block Hash)

(Wave your hands...like you just don't care)


# What is the Chain?
* Each block references previous block, chaining them together in a permanent, unbreakable order.
* History cannot be altered without breaking the entire thing or replacing entire thing
* Hence: Immutable
* Kind of like a Linked List, or commits in a github repo


# Consensus
* The resulting ledger is the consensus produced by the system
* Unlike Property Title Ledger, any change in the history invalidates the entire thing - cannot be altered
* Unlike Bank Ledger, transactions are publicly confirmed, cannot by changed, and the system cannot be controlled by any one entity


# Trust
* Electricity: As long as electricity exists, computers can run and maintain Bitcoin
* Mathematics: Core principles in math ensure a ton of combined & aligned computations are needed to confirm a block (Proof of Work)
* Code: Bitcoin is open source, you can read through it, run a miner yourself, and watch it all in action
* Consensus & Trust closer to Carbon Dating than a Government


# Goal Checkpoint?
1. Understand why trust & consensus are immensely critical & immensely fragile today
2. Understand how Bitcoin ledger address the trust & consensus problem
3. Understand how each block in the Bitcoin blockchain demonstrates Proof of Work


# Hashing
* Take an input of arbitrary size and convert it to a fixed size
* Example using mod 10


# Cryptographic Hash (SHA256)
* Cryptographic hash -> very difficult to determine input based on output
  * Very easy [O(1)] to verify input produces hash
  * SHA256 - 30 million cores @ 5 million hashes per second would take ~52 years to guess a 12 character password
* Same input = same output
* Small change in input results in completely different output
* https://passwordsgenerator.net/sha256-hash-generator/


# The Guessing Game / Math Problem
* Take 4 values that are static + a timestamp and concatenate them together
* Add the "guessing number" '0' to the end
* Calculate the SHA256 hash -> Get an output hash
* If hash does not have 18 leading 0's, try with '1' as the guess, try again
* Keep incrementing the guess until you get an output with 18 leading 0's


# Block Header
* hashMerkleRoot (Transaction Hash) Take all the transactions in a block, and generate a single hash (Merkle Root)
* hashPrevBlock (Block Hash) The hash of the previous block
* 3 Other Fields (Version, Time, Bits/Difficulty)
* Nonce (Randomly Guessed Number)


# Proof of Work
* Eventually, you would get a solution
* Thanks to SHA256, there is no shortcut, it's manual trial and error
* Thus, a solution proves you put in the work of guessing
* Proof of Work!


# Every Valid Block is Proof of Work
* Contains valid transactions (verifiable)
* Enforced order, immutability, and dependence on previous block


# Goal Checkpoint?
1. Understand why trust & consensus are immensely critical & immensely fragile today
2. Understand how Bitcoin ledger address the trust & consensus problem
3. Understand how each block in the Bitcoin blockchain demonstrates Proof of Work
4. Understand how Bitcoin miners use Nakamato Consensus to make the Bitcoin Network a secure, decentralized, and trustless source of truth


# Bitcoin Miners
* These are the machines that are doing the proof of work
* Validating transactions, creating blocks, guessing at the hash
* The Doge's mining in the cave with GPU's
* Any one miner could take hours or days to guess the answer, that's too slow!


# Bitcoin Miner Incentive
* When a block is mined, an additional transaction is added crediting the miner's wallet with a reward
* Block Reward: 12.5 BTC (originally 50 BTC)
* This is where all Bitcoin comes from, creating an interesting distribution system
* BTC can only be earned by Proof of Work, giving it some inherent value!
* Why doesn't every miner just keep giving itself rewards?


# Nakamato Consensus
* All miners are connected and pulling from the same pool of transactions
* Bitcoin code will automatically respect the longest chain as the single source of truth
* Thus, mining a block only means the reward if your chain is the longest
* Creates a race between miners to create the block first and get the reward
* As soon as a new block is created, all miners build from that block since it's a much better chance of them winning the next block


# Bringing it all together
* Bitcoin is a ledger of balances, there aren't physical Bitcoins (just like how Bank of America tracks your balance without a stack of cash & coins)
* Miners validate transactions and solve hard math problems to make money
* Invalid transactions are ignored by Miners since they wouldn't allow them to win the reward
* A mined block in the chain is permanent and can never be changed
* Every miner has a full copy of the ledger so it can never be destroyed or lost


# A Trustless Consensus
* Not trusting an entity that can abuse the trust
* Participants are incentivized to validate transactions, the more miners, the stronger the network
* Only way to modify history is to completely replace the entire chain!  So 10+ years of mining ~560k blocks makes this incredibly secure
* The ledger is constantly validated independently.  You can verify the entire chain yourself, just like a science experiement!


# A Beautiful Harmony
* More miners = Faster Blocks & More Security
* More miners = Harder to Win
* Less miners = Easier to Win, attracting more Miners
* Higher Value of BTC = More incentive for miners
* Finite BTC = Limited Supply = Control Inflation = Higher Value of BTC


# Bitcoin in Action!
* https://www.blockchain.com/explorer


# Goals of This Talk
1. Understand why trust & consensus are immensely critical & immensely fragile today
2. Understand how Bitcoin ledger address the trust & consensus problem
3. Understand how each block in the Bitcoin blockchain demonstrates Proof of Work
4. Understand how Bitcoin miners use Nakamato Consensus to make the Bitcoin Network a secure, decentralized, and trustless source of truth
5. Understand the difference between a cryptocurrency (ie Bitcoin) and a blockchain


# Bitcoin Ledger & Network Implement a Blockchain
* Bitcoin is a cryptocurrency whose balances are tracked in the Bitcoin Ledger
* Bitcoin Ledger is made of transactions stored within the Bitcoin Blockchain
* Bitcoin Blockchain is maintained via the Bitcoin Network
* Bitcoin Network is made up of Miners who are running the Bitcoin code which defines Proof of Work and Nakamato Consensus


# Blockchain
* A blockchain is a ledger or database which maintains its truth via transactions within blocks
* Blocks are build on each other in a chain by including data from the previous block to ensure integrity
* A Blockchain can have any data at all
* A Blockchain depends on miners to keep it running and thus is only as secure as the number of miners


# Cryptocurrency
* Cryptocurrency is a store of value tracked and maintained within a blockchain
* A blockchain needs an incentive for Miners to maintain it, so nearly all blockchains maintain a currency as a way to reward the miners
* The value of a cryptocurrency is extremely fluid and depends on many things including how it's tied to the blockchain that supports it, the security & popularity of that network, etc.


# Bitcoin Forks
* Just like any software, there are updates & improvements
* Due to the nature of Bitcoin, updates can have really big impacts
* If a major release / breaking change is introduced, the miners that upgrade are now operating on a completely separate blockchain than the original
* If some miners never update, then these are 2 separate blockchains
* Bitcoin Cash, Bitcoin Satoshi Vision, and Bitcoin Gold are all examples of hard forks with their own miners who have duplicate blockchain histories up until the fork


# Dogecoin
* Separate blockchain & currency
* Also uses Proof of Work & Nakamato Consensus, but uses a variant of a hashing algorthym called "scrypt" instead of SHA256
  * Different blockreward, way more coins (121B vs 17M), no cap (BTC is 21,000,000), and way more blocks (~2.9M vs 560k)


# Ethereum
* Smart Contracts
* Ether is block reward for miners + Price paid to execute smart contracts
* Many powerful applications


# Goals of This Talk
1. Understand why trust & consensus are immensely critical & immensely fragile today
2. Understand how Bitcoin ledger address the trust & consensus problem
3. Understand how each block in the Bitcoin blockchain demonstrates Proof of Work
4. Understand how Bitcoin miners use Nakamato Consensus to make the Bitcoin Network a secure, decentralized, and trustless source of truth
5. Understand the difference between a cryptocurrency (ie Bitcoin) and a blockchain
6. Understand why blockchain can solve so many other problems


# Fun Time - Possibilities!








