---
description: Use the Fractal web app to execute a Fractal Multisig Proposal
---

## Overview

Successfully passed proposals can be executed, which processes all transactions within the proposal on the blockchain.

---

## Executing a Passed Proposal

1. Visit the details page of the proposal that you would like to execute. 
2. Review the transactions that will be executed by clicking **Show Executable Code**.

![](../.gitbook/assets/show-executable-code.png)

3. Click the **Execute** button to trigger the transaction in your wallet extension.

![](../.gitbook/assets/execute-transaction.png)

---

## Viewing an Executed Proposal

Once the execution transaction is confirmed on the blockchain, the state of the proposal will become `Executed`.

You can view the transaction by clicking on the link next to `Transaction Hash` in the details page of the proposal.

![](../.gitbook/assets/proposal-transaction-hash.png)

{% hint style="info" %}
Proposals have a **Timelock** period after passing which must elapse before the proposal can be executed.
{% endhint %}