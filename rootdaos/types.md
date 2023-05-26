---
description: Fractal Governance Types
---

## Multisig

A multisig DAO is essentially just a Safe multisig wallet, no different than deploying one via [the Safe app itself](https://app.safe.global), except that Fractal adds an on-chain DAO name and optional Snapshot space to be associated with it.

For multisig *subDAOs*, Fractal also attaches a [Safe guard contract](https://docs.safe.global/learn/safe-core/safe-core-protocol/guards) which allows the ability of the parentDAO to vote to freeze the multisig from executing transactions.

---

## Token Voting

A Token Voting DAO is a Safe contract wallet with an Azorius [module](https://docs.safe.global/learn/safe-core/safe-core-protocol/modules) contract which allows for submitting and voting on proposals by token holders.

Our [Azorius module](https://github.com/decent-dao/fractal-contracts) conforms to Safe's [Zodiac pattern](https://gnosisguild.mirror.xyz/OuhG5s2X5uSVBx1EK4tKPhnUc91Wh9YM0fwSnC8UNcg), and has been audited by [Halborn](https://www.halborn.com/), the results of which can be found [here](https://app.fractalframework.xyz/docs/fractal_audit.pdf).

There are no signers on this Safe, all transaction executions are performed via the Safe module contract after a proposal is successfully voted on.

In the case of Token Voting *subDAOs* Fractal also attaches a Safe guard contract to allow for freezing, in the same way as a multisig subDAO.

---

## NFT voting (coming June 2023)

NFT-based voting will allow for holders of specific ERC-721 tokens to vote on proposals, in much the same way as Token Voting governance allows for ERC-20 holders to vote.

This will integrate with our existing Azorius module, and allow for multiple NFTs to be "registered" on the strategy, with optional voting weights for each NFT collection given voting power.

NFT voting *subDAOs* will also have an associated Freeze Guard contract, identical to Token Voting subDAOs.