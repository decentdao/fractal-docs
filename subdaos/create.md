---
description: Deploy a new sub-Safe on Fractal
---

## Overview

Creating a Fractal **sub-Safe** deploys a new Safe{Wallet} contract, with an attached governance module.

At the end of the sub-Safe creation flow a proposal is submitted to the parent-Safe, which must be passed by the parent in order to successfully deploy the DAO.

---

## Propose a sub-Safe

Visit the homepage of the DAO you would like to create a sub-Safe for (this DAO will be the parent).

{% hint style="info" %}
For multisig DAOs, you must be a signer to propose a sub-Safe.
{% endhint %}

Using the `Manage DAO` menu, select **Add a sub-Safe**.

![](../.gitbook/assets/add-a-sub-dao.png)

### New DAO creation

Follow the same [DAO creation flow](../rootdaos/create.md) as for parent-Safes.

### Configure Parent Controls

![](../.gitbook/assets/parentcontrols.png)

Here you will configure the parent-Safe control parameters for your sub-Safe:

- **Freeze Votes Threshold** - Total votes required by the parent to *freeze* the sub-Safe.
- **Freeze Proposal Period** - The length of time for a freeze vote on the sub-Safe.
- **Freeze Period** - The length of time a successful Freeze Vote will freeze this DAO from executing proposals.

### Create sub-Safe Proposal

Click `Create sub-Safe Proposal` to create a proposal on the parent-Safe to deploy this sub-Safe.

Passing this proposal will allow the sub-Safe deployment transaction to be executed.

### Viewing your sub-Safe

![](../.gitbook/assets/dao-heirarchy-icon.png)

After passing and executing your sub-Safe proposal, your DAO is now ready to interact with.

Visit the **DAO Heirarchy** page from the left menu of your parent-Safe to see all sub-Safes.

---

## Multisig sub-Safe

In addition to [DAO creation flow](../rootdaos/create.md#multisig-rootdao) for multisig DAOs, a multisig sub-Safe will have the following additional parameters:

- **Timelock Period** - The amount of time between when a proposal passes, and when it can actually be executed on the blockchain.
- **Execution Period** - The amount of time a passed proposal has to be executed before it expires.