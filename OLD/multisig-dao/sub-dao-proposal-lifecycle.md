---
description: View and understand the lifecycle of a Multisig subDAO proposal.
---

# subDAO Proposal Lifecycle

## Overview

![](../../../../.gitbook/assets/proposal_state_flows.png)

#### Active
As soon as the proposal has been created, it becomes active and any signer on the DAO can vote on the proposal. There is no voting period, and the proposal will be able to be voted on as long as the proposal is not passed or executed.

#### Timelockable
Once the threshold number of signers have signed the proposal, the proposal is passed and becomes Timelockable.

#### Timelocked
The timelock period is the time between when a transaction is queued and becomes executable. During this time, the proposal can't be executed.

The parentDAO is responsible for setting the timelock period.

#### Executable
Once the timelock period ends, a queued proposal becomes executable and the execution period starts.

A proposal that is executable can be executed, executing the transactions in the proposal.

If a subDAO is frozen by the parentDAO, it can't execute transactions, reguardless of whether they've passed.

#### Executed
After a proposal is executed, the transactions on the proposal are executed and the proposal enters the **Executed** state.

#### Rejected
If another proposal with the same nonce is executed, then a proposal with the same nonce which is not executed will enter a Rejected state. A rejected proposal can no longer be executed.

#### Expired
There is a limited amount of time when the transaction can be executed, called its Execution Period.

If a proposal is not executed in this time period, the proposal becomes expired. 

The parentDAO is responsible for setting the execution period.