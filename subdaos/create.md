---
description: Deploy a new sub-Safe on Fractal
---

## Overview

Creating a Fractal **sub-Safe** deploys a new Safe{Wallet} contract, with an attached governance module.

At the end of the sub-Safe creation flow a proposal is submitted to the parent-Safe, which must be passed by the parent in order to successfully deploy the new Safe.

---

## Propose a sub-Safe

Visit the homepage of the Safe you would like to create a sub-Safe for (this Safe will be the parent).

{% hint style="info" %}
For multisig governance, you must be a signer to propose a sub-Safe.
{% endhint %}

Using the `Manage Safe` menu (3 vertical dots), select **Add a sub-Safe**.

![](../.gitbook/assets/add-a-sub-dao.png)

### New Safe creation

Follow the same [Safe creation flow](../rootdaos/create.md) as for parent-Safes.

### Configure Parent Controls

![](../.gitbook/assets/parentcontrols.png)

Here you will configure the parent-Safe control parameters for your sub-Safe:

- **Freeze Votes Threshold** - Total votes required by the parent to *freeze* the sub-Safe.
- **Freeze Proposal Period** - The length of time for a freeze vote on the sub-Safe.
- **Freeze Period** - The length of time a successful Freeze Vote will freeze this Safe from executing proposals.

### Create sub-Safe Proposal

Click `Create sub-Safe Proposal` to create a proposal on the parent-Safe to deploy this sub-Safe.

Passing this proposal will allow the sub-Safe deployment transaction to be executed.

### Viewing your sub-Safe

![](../.gitbook/assets/dao-hierarchy-icon.png)

After passing and executing your sub-Safe proposal, your new Safe is now ready to interact with.

Visit the **Heirarchy** page from the left menu of your parent-Safe to see its sub-Safes.

---

## Multisig sub-Safe

In addition to the [Safe creation flow](../rootdaos/create.md#multisig-rootdao) a multisig sub-Safe will have the following additional parameters:

- **Timelock Period** - The amount of time between when a proposal passes, and when it can actually be executed on the blockchain.
- **Execution Period** - The amount of time a passed proposal has to be executed before it expires.