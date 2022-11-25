# Reputation Ranking System (RRS)
Provides Reputation Scoring and Ranking on multiple chains.
RRS utilizes mathematical ranking algorithms to compute reputation scores and ranking of Web3 accounts from onchain transactions.

# Problem
Inspired by [PageRank](https://en.wikipedia.org/wiki/PageRank), aka a trillion-algorithm made-up Google's Success, Octan Labs wants to build a similar system in Web3 space, namely Reputation Ranking System to help
- Unify user data & reputation on multiple chains, then provides efficient metrics for new projects to target, acquire & incentivize potential users 
- Recognize all stakeholders, users & contributors by both staking & reputation weights, making DAOs more decentralized & democratic 
- Provide credit scoring to unleash under-collateral crypto lending.

# Preliminary concepts:
Octan Labs utilizes [Graph Theory](https://en.wikipedia.org/wiki/Graph_theory) to study and extract insights over accounts and transactions on various blockchains.
- A node means an account (or public address) on a blockchain. It may be a personal account or a contract address.
- An edge indicates one or many directed transactions from account A to account B. 
- A (directed) graph consists of nodes and edges, presenting transactions among accounts.  

We mainly investigate directed graphs (i.e. sets of transactions between accounts).
- Reputation of an account is its importance within a considered graph.
- Reputation score (or ranking score) of an account is a real, positive number measuring the account's reputation within a considered graph.

# Computation
Based on mathematical ranking algorithms (e.g. PageRank), Octan's Reputation Ranking System (RRS) typically computes reputation scores or ranking scores of all accounts in a certain graph.
Octan Labs defines three schemas when computing reputation scores as followed:
- *Global Reputation or Global Ranking* considers all accounts and transactions on a blockchain, for example, entirely on Ethereum (more than 200 million accounts and multi-billions of transactions between them since the genesis block). However, due to limitations in computing resources, we may restrict to the most recent blocks within a deterministic range, for instance, a range of blocks from block height of 10,000,000 to 16,003,045.   The global ranking score represents the reputation or importance measurement of an account in the entire blockchain network.
- *Local Reputation* considers subgraphs, i.e. sets of accounts and transactions filtered by specific (localized) criteria, for example, all transactions belonging to a token-contract (e.g. USDT), all accounts and transactions going through Uniswap protocol or participating in DeFi markets. Local ranking score represents the reputation or importance measurement of accounts within a subspace of a blockchain network (e.g. DeFi subspace, NFT-game subspace).
- *Personalized Reputation* computes the reputation score of accounts relevant to one or many specific accounts, for example, a token-contract (e.g. USDT), to Uniswap protocol, or to Axie Infinity game. The personalized ranking is similar to but not the same as the local ranking. It represents the reputation or importance measurement of accounts relevant to one or many specific accounts.

All reputation scores associated with a certain schema are normalized to 1000, i.e. sum of all reputation scores is 1000. 
https://drive.google.com/file/d/12F5FPP6ep_Scif35_Ankfvks67tF51D5/view?usp=sharing
![reputation score flow within Octan's Products](https://user-images.githubusercontent.com/45308207/202169476-6f0259cb-ddcd-479e-80a0-f68f35ba7baa.png)
> Current reputation score flow within Octan's Products

# Experiments
In the following, we provide several reputation rankings computed from data samples on BSC, BTTC, Aurora, and Polygon. The folder includes lists of entire ranking scores and Top100 with identity (if any), and additional statistics (total transactions, total in-transactions), according to specified chains. Refer to https://drive.google.com/drive/folders/1LmCoMSmgsz2Xz-IfELvijTrhNcZcqcDD?usp=share_link
- Reputation Rank on BSC (3.3M transactions)
- Reputation Rank on BTTC (~1.4M transactions, 90k blocks) and Aurora (~60k transactions, 270k blocks)
- Reputation Rank on Polygon (~7M transactions, 100k blocks) 

# Weighting transactions
Newer transactions should be weighted higher than old ones. The weighting function should be increasing and bounded as block height increases. [Logistic functions](https://en.wikipedia.org/wiki/Logistic_function) are good candidates. 

Problem setting:
- Consider transactions collected within a specific timeframe, e.g. 1 year = 360 days.
- Day variable is 'x' centering at 0.
- Weighting function by a logistic curve: W = a/(1+2^(-b*x)), where a>0 and 0<b<1 are adjusting parameters
https://www.desmos.com/calculator/hyzbkqhj0u
![Logistic curves](https://user-images.githubusercontent.com/45308207/203902122-6edf2bbc-4648-426b-9444-2eb06f3ae89b.png)

