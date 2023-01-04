---
description: Use the Fractal web app to create a new Fractal DAO.
---

# Creating a Fractal

## Overview

Creating a Fractal adds a smart contract for a basic DAO at a new address on the blockchain. You can create new Fractals on any [supported Ethereum testnet or mainnet](the-core-framework/reference/deployed-contracts.md). All you need is an ERC20 wallet and some ETH to cover the gas fees.

## Create a Fractal

Before you get started, open the Fractal app and connect to your wallet:

![](../../.gitbook/assets/fractal-app-landing-page.jpg)

Click **Create a Fractal**. The Essentials screen opens:

![](../../.gitbook/assets/create-fractal-configure-new-fractal.jpg)

Enter a name for your Fractal DAO and click **Next**. The Mint a New Token screen opens:

![](../../.gitbook/assets/create-fractal-mint-token.jpg)

This screen allows you to configure the governance token that will be used to allocate voting power for your DAO. Fill in values for the following fields:

* Token Name - The name of your governance token
* Taken Symbol - The symbol of your governance token that will be used when displaying wallet balances
* Token Supply - The initial quantity of governance tokens to mint
* Allocation - The target wallet addresses and quantities of governance tokens that will be distributed when the DAO is created
  * Address - An Ethereum wallet address that will receive governance tokens
  * Amount - The quantity of tokens to send to the address

If you want to distribute tokens to more than one wallet, click **Add Allocation** and enter values for the fields defined in the list value.

{% hint style="info" %}
The governance tokens are sent to the target addresses, but they cannot be used to vote until they have been [delegated](interacting-with-a-dao/delegate-your-voting-power.md).
{% endhint %}

When you have finished configuring your governance tokens, click **Next**. The Governance Setup screen opens:

![](../../.gitbook/assets/create-fractal-governance-setup.jpg)

The Governance Setup screen lets you define who can create proposals, the terms of voting on proposals, and how proposals will be executed if they are accepted. Fill in values for the following fields to configure your DAO's governance process:

* Proposal Creation - Restricts who is authorized to create proposals based on the token balance of the person creating the proposal. This is not a fee for creating proposals, and these tokens are not lost if the proposal fails. This just sets a check to see whether a wallet contains this amount of governance tokens before it can submit a proposal.
* Quorum - Defines the percentage of eligible voting power that must vote YES in order for a proposal to be accepted. Eligible voting power is voting power that has been delegated.
* Execution Delay - Sets the amount of time in hours that the DAO must wait to execute the proposal after the proposal passes. The execution delay gives people a chance to exit the system if they don't like the proposal.

{% hint style="info" %}
These values can be edited using proposals in the Fractal Dashboard.
{% endhint %}

Once you have configured your governance model, click **Deploy**. The Fractal App opens your connected wallet so that you can approve the transaction:

![](../../.gitbook/assets/metamask-confirm-trans.jpg)

The transaction amount covers the gas fees required to get the transaction mined on chain. Review the transaction and click **Confirm**. MetaMask sends the transaction and the Fractal contracts deploy your DAO. MetaMask displays a notification when the transaction is completed. The Fractal app opens the DAO dashboard for your new DAO.

{% hint style="info" %}
Grab the address of your new Fractal from the URL in your browser's address bar.
{% endhint %}

![](../../.gitbook/assets/create-fractal-complete.jpg)
