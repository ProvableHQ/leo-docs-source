# Provers

### Role of Provers in the Network

Provers are an integral part of the Aleo network but do not participate in Aleo's network consensus. They run specific algorithms to solve the *CoinbasePuzzle* and obtain a *ProverSolution* that satisfies the *ProofTarget*. This *ProverSolution* is then broadcasted. When the consensus network confirm and include the *ProverSolution* in a block, the Prover receives the corresponding *CoinbaseReward* incentive.

### Economic Incentives for Provers

In the long term, the *CoinbaseReward* incentive that Provers can obtain is directly proportional to their computational power relative to the entire network. The economic incentive for Provers is similar to PoW in Bitcoin, but unlike Bitcoin, Aleo's network doesn't employ a winner-takes-all strategy. As long as the *ProverSolution* satisfies the *ProofTarget*, it is accepted by the network. This approach ensures fairer and more stable rewards for Provers. It's noteworthy that unlike the *BlockReward* for validators, the *CoinbaseReward* decreases over time, reducing by 10% annually until there are no *CoinbaseReward* incentives after 10 years.

> PuzzleReward = CoinbaseReward * 2/3
>
> BlockReward = 23.8 + CoinbaseReward * 1 / 3
>
> Where:
>
> BlockReward is distributed by validators based on staking ratios.
>
> PuzzleReward is distributed based on the proportion of Prover Solutions' ProofTargets.

### CoinbasePuzzle

In the Aleo network, every 360 blocks form an Epoch. When a new Epoch is generated, the CoinbasePuzzle changes accordingly. If a Prover continues to compute the old CoinbasePuzzle, the network won't accept its ProverSolution. This implies that Provers need to monitor block changes in the consensus network and update their CoinbasePuzzle information when a new Epoch is generated.

Besides requiring ProverSolutions to be valid, the network also demands that they meet a ProofTarget greater than or equal to the specified value. The ProofTarget measures the workload of Provers; similar to PoW, the higher the ProofTarget, the greater the computational work required by the Prover. Aleo's network adjusts the ProofTarget based on the current computational power participating in the network. When computational power increases, the network enlarges the ProofTarget to prevent excessive ProverSolutions, thus avoiding network congestion and situations where numerous ProverSolutions cannot be included in blocks.

### Become a Prover Node

Becoming a Prover node does not require staking or authorization, anyone can [start a Prover node](https://github.com/ProvableHQ/snarkOS?tab=readme-ov-file#32-run-an-aleo-prover).
