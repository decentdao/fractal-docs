---
description: Use the Fractal web app to create a new Fractal RootDAO.
---

# Creating a Fractal Multisig SubDAO

## Overview

This guide shows how to create a **SubDAO**. If you would like to create a RootDAO, please visit: [Create a RootDAO](create-a-root-dao.md)

A SubDAO is a Fractal which has a ParentDAO. A ParentDAO is the DAO the SubDAO was deployed from.

A ParentDAO has the ability to set proposal parameters on the SubDAO and freeze the SubDAO which prevents the SubDAO from executing proposals.

## Create a Multisig SubDAO:

Visit the Multisig DAO you would like to create a SubDAO for (this DAO will be the parent). You will need to know the ParentDAO's address.

{% hint style="info" %}
You must be a signer on the parent DAO.
{% endhint %}

Click the three dots icon next to the DAO name, and click "Add a subDAO"

![](../../../../.gitbook/assets/add-a-sub-dao.png)

Enter a name for your SubDAO and click **Next**.

![](../../../../.gitbook/assets/enter-fractal-name.png)

Select "Governance Type" as "Multisig" and click "Next".

![](../../../../.gitbook/assets/choose-governance.png)

The **Configure Multisig** screen lets you define the signers who can create and vote on proposals.

Set your signatories and signer threshold:
- **Total signers**: The number of signers that can submit and approve transactions. One address must be entered under "Signer Addresses" for each signer.
- **Threshold**: How many signers must sign a proposal for it to pass (and be executed).

In the example image below, a proposal for this DAO would have 3 total signers, and would require 2 of those signers to sign for the proposal to pass.

![img.png](../../../../.gitbook/assets/multisig-dao-setup-params.png)

On the **Configure Parent Controls** page, fill out the parent controls.
- Timelock Period - The length of time (in minutes) a passed proposal must wait to be executed on the blockchain after it is queued.
- Execution Period - The length of time (in minutes) a successful proposal must be executed within after the timelock period ends.
- Freeze Votes Threshold - Total votes required by the parent to freeze the SubDAO.
- Freeze Proposal Period - The length of time (in minutes) for a Freeze Votes's starting and ending point.
- Freeze Period - The length of time (in minutes) a successful Freeze Vote will freeze this DAO.

Click "Create a SubDAO Proposal". This will create a proposal for the SubDAO which will need to be passed & executed by the parent DAO.

![](../../../../.gitbook/assets/configure-parent-controls.png)

Confirm the SubDAO proposal TX with your wallet. Once the SubDAO proposal transaction is confirmed, visit the proposal for the subDAO deployment.

This proposal should be the most recent proposal in the proposals list. 

Click "View Proposal" on the proposal to view it.

![](../../../../.gitbook/assets/view-subdao-proposal.png)

Once this proposal is passed (has enough signatures), click "Execute". 

![](../../../../.gitbook/assets/execute-transaction.png)

After the execution TX confirms, the SubDAO will be deployed.

Visit the "DAO Heirarchy" page from the left menu of your parent DAO, and you will see the SubDAO listed as a child.

![](../../../../.gitbook/assets/dao-heirarchy-icon.png)