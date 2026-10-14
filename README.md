<!-- solve-problem:a6bec5d1-9a2f-42de-87f7-5f0a8be3f392 -->
# Stability of mining counterstrategies

A [Tangle Solve problem](https://solve.tangle.tools/problems/a6bec5d1-9a2f-42de-87f7-5f0a8be3f392).

## What is the problem?

Consider a permissionless proof-of-work blockchain in which miners organize into pools. Each epoch/pool controls a fraction αᵢ of total hash power and chooses a strategy: **honest mining** (extend the public chain and publish immediately), **selfish mining** (withhold blocks to force others onto forks, per Eyal–Sirer 2014), or a **counterstrategy** — deviating specifically against a known deviator, e.g., withholding blocks that would extend a selfish miner's public chain, or refusing to mine on its forks (the Gal–Szabo–Rottenstichen 2025 counterstrategy).

The open question is a **repeated-game** one: model pool interaction as an infinitely repeated game with stage payoffs equal to expected discounted block rewards (and possibly a value for consensus health). Characterize the equilibrium set — ideally the (subgame-perfect or sequentially rational) equilibrium payoff correspondence, or at least:

1. **When does the threat of counterstrategy sustain honesty as an equilibrium?** I.e., for which hash-power vectors (α₁,…,αₙ) and discount factors δ does a punishment strategy profile (all honest on-path; any selfish deviator is counterstruck) form a subgame-perfect equilibrium?
2. **When does retaliation instead *concentrate* effective power?** Counterstrategy often requires coordination among retaliating pools (they must jointly ignore the deviator's chain); if only one large pool can retaliate, the punishment path may give that pool de facto control, degrading decentralization even though everyone is "punishing an attacker."

Precise setting to fix: n pools, PoW with selfish-mining-style state machine (private chain lengths, mining race), payoffs = fraction of blocks in the canonical chain, information = public chain state plus (optionally) pool identity. The census is informal; formalizing the counterstrategy as a strategy in a stochastic game and analyzing the repeated/stochastic equilibrium structure is itself part of the problem.

## Why does it matter?

A solution tells us whether the "markets punish attackers" narrative actually holds in proof-of-work: if counterstrategies sustain honest equilibria without a 51% enforcer, decentralization is self-stabilizing; if they require a dominant retaliator, then the cure for selfish mining is worse than the disease. This directly informs pool-operator policy (should pools adopt counterstrategies by default?), protocol design (parameters like the Eyal–Sirer γ that determine counterstrategy efficacy), and how regulators/communities should view pool concentration. It also connects blockchain incentives to the classical repeated-game folk theorem literature in a concrete, computable setting.

## What's already known?

- **Selfish mining baseline (Eyal–Sirer 2014):** a pool of size α > ¼ (optimistically, α > 1/3 with γ = 1) profits from withholding. Many extensions (stubborn mining, underhanded strategies) refine these thresholds.
- **Kiayias et al. (2016), *Blockchain Mining Games*:** models mining as a stochastic game, showing the strategy space is complex and honest mining is generally *not* an equilibrium of the one-shot game; selfish behavior can be profitable even for small miners under certain conditions. Provides the stochastic-game machinery.
- **Gal, Szabo, Rottenstreich (2025):** a counterstrategy to selfish mining that does *not* require majority hash power — a sufficiently large honest-side pool can withhold publication against a selfish miner so that the selfish miner's withholding backfires, restoring (roughly) proportional or better rewards for honest miners. This establishes the *stage-game* viability of retaliation but **not** repeated-game equilibrium structure: it treats the counterstrategy as exogenously adopted.
- **Gap:** No characterization exists of (i) subgame-perfect equilibria in the repeated mining game with counterstrategies as punishments, (ii) conditions on the power vector under which punishment is credible (profitable for the punisher on the punishment path), or (iii) the decentralization cost of punishment (effective power concentration). Folk-theorem machinery exists but is unapplied; the non-anonymity of pools and the public-chain Markov state make standard arguments non-trivial.

## What would count as solving it?

Any of the following, in increasing strength:

1. **Equilibrium existence/conditions:** A theorem giving necessary and/or sufficient conditions on (α₁,…,αₙ, δ, γ) under which "honest unless deviated against, then counterstrike for T periods" is a subgame-perfect equilibrium of the repeated mining game. Ideally matching upper/lower bounds on the critical α below which honesty is sustainable.
2. **Concentration result:** A characterization of punishment paths showing when retaliation requires effective power ≥ threshold β, with a bound on resulting effective-power concentration (e.g., "any equilibrium sustaining honesty against an α-pool requires some retaliator to control ≥ f(α) of hash power").
3. **Folk theorem (positive or negative):** Either a folk theorem for the repeated mining game (characterizing the equilibrium payoff set) or an impossibility result showing the equilibrium set is small/degenerate.
4. **Computational companion:** A Markov/stochastic-game solver computing equilibrium payoffs for small n, demonstrating the phase transition empirically.

A referee would accept clean theorems in a well-specified model (state assumptions explicitly) or a rigorous computational study with the model fully formalized.

## How to start

1. **Read Eyal–Sirer (2014) Sections III–IV first** to internalize the selfish-mining Markov chain and the role of γ; everything is built on this state machine.
2. **Read Kiayias et al. (2016)** for the stochastic-game formulation — Sections on the mining game model and the non-equilibrium of honesty. Reuse their formalism so your model is comparable.
3. **Read Gal, Szabo & Rottenstreich (2025)** and extract the counterstrategy's stage-game payoff matrix against a selfish miner; this gives you the punishment-stage payoffs you need for repeated-game analysis.
4. **Natural approach:** Start with 2 or 3 pools, model the game as a stochastic game where the state is the fork state and the strategy includes retaliation, then apply trigger-strategy/renegotiation-proofness arguments. **Main obstacle:** the punishment path payoffs depend on the Markov fork dynamics (not stage-independent), so textbook folk-theorem proofs need adaptation; also the punisher's own incentives during punishment are the binding constraint.

## Key references

- I. Eyal, E. G. Sirer, "Majority is not Needed: Bitcoin Mining is Vulnerable" (2014), arXiv:1310.7935 — defines selfish mining, the Markov analysis, and the γ threshold; the baseline everyone attacks. (verify link resolves: https://arxiv.org/abs/1310.7935)
- A. Kiayias, E. Koutsoupias, M. Kyropoulou, Y. Tselekounis, "Blockchain Mining Games" (2016), arXiv:1607.02420 — models mining as a stochastic game and shows honest mining is not generally a one-shot equilibrium. https://arxiv.org/abs/1607.02420
- O. Gal, ? Szabo, L. Rottenstreich, "Majority is not Needed: A Counterstrategy to Selfish Mining," IEEE ICBC 2025, DOI:10.1109/ICBC64466.2025.11114470 — shows a sub-majority pool can deploy a withholding counterstrategy that makes selfish mining unprofitable. (verify author initials and content — census gives surname-level citation only)
- (Background, optional) A. Ewerhart, "Profitable Double-Spending Attacks," or L. Cong, Z. He on blockchain game theory surveys — for repeated-game framing. (verify)

## Warnings

- The 2025 ICBC paper's exact counterstrategy mechanics are not specified in the census; read it before modeling — your punishment-path payoffs must match its actual construction, not an assumption.
- Ambiguity: "stability" could mean dynamic/evolutionary stability or repeated-game equilibrium stability; pick repeated-game SPE as the primary target and note the ESS interpretation as a variant.
- Common dead end: treating pools as a single deviator. Retaliation credibility depends on *which* pool retaliates; coalition-proofness / renegotiation issues are where the decentralization cost actually bites.
- Do not ignore γ (fraction of non-deviating hash that follows the longest chain during forks) — results qualitatively change at γ = ½.
- The census attribution "fleet:gpt6pro" is a secondary source; verify all bibliographic details against the DOI/arXiv records.

---
*`BCGT-000238` · flagged for review*

## Acceptance guidance

Any of the following, in increasing strength:

1. **Equilibrium existence/conditions:** A theorem giving necessary and/or sufficient conditions on (α₁,…,αₙ, δ, γ) under which "honest unless deviated against, then counterstrike for T periods" is a subgame-perfect equilibrium of the repeated mining game. Ideally matching upper/lower bounds on the critical α below which honesty is sustainable.
2. **Concentration result:** A characterization of punishment paths showing when retaliation requires effective power ≥ threshold β, with a bound on resulting effective-power concentration (e.g., "any equilibrium sustaining honesty against an α-pool requires some retaliator to control ≥ f(α) of hash power").
3. **Folk theorem (positive or negative):** Either a folk theorem for the repeated mining game (characterizing the equilibrium payoff set) or an impossibility result showing the equilibrium set is small/degenerate.
4. **Computational companion:** A Markov/stochastic-game solver computing equilibrium payoffs for small n, demonstrating the phase transition empirically.

A referee would accept clean theorems in a well-specified model (state assumptions explicitly) or a rigorous computational study with the model fully formalized.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/a6bec5d1-9a2f-42de-87f7-5f0a8be3f392?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000238.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000238)
