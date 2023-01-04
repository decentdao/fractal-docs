---
description: >-
  Disclaimer: app.fractalframework.xyz is in alpha on mainnet and unaudited. Use
  at your own risk.
---

# Welcome to Fractal

Fractal provides tools to support everyone from DAO members to Web3 developers:

* Developers - Build modules to extend the functionality of DAOs and develop new front-end user experiences.
* DAO Organizers - Create on-chain DAOs and establish their governance models.
* DAO Participants - Interact with DAOs, depending on the structure created by the organizers.
* DeFi protocols - Use DAOs to support sophisticated workflows and scale up your DeFi protocol.

<img src=".gitbook/assets/file.drawing.svg" alt="Ecosystem" class="gitbook-drawing">

The components of the Fractal framework support the creation of a composable operating system for DAOs, which allows DAOs to outscale and outperform traditional organizations.

This site includes the following documentation:

* **The Core Framework** - <TBD>
* **Developer Guides** - <TBD>
* **User Guides** - Information about [using the Fractal app](broken-reference) to deploy, manage, and participate in DAOs.
* **FAQ** - Short answers to common [questions](user-guides/faq.md) about Fractal.

## Why Fractal?

Fractal fills the need for better DAO tooling and social coordination across the Web3 space. Existing DAO platforms limit the functionality and structure of DAOs by making assumptions about the functions of DAOs. Fractal expands the possibilities for DAOs by providing a composable and extensible framework that supports any structure and governance model that an organization could require.

### Benefits

The Fractal framework has several benefits over traditional DAO implementations:

* Flexible - A framework for structuring any organization (a _Fractal_) as a group of independent governing bodies (_Nodes_).
* Extensible - Modules extend the capabilities of nodes by implementing specific business requirements that a DAO may have, such as treasury management and governance.
* Interoperable and upgradable - A shared Node architecture that enables any Module to service any Node for any Fractal or even every Node for every Fractal.

## Nomenclature

##### A Fractal
A specific type of DAO where contributors can be structured into unique Parent and subDAO hierarchies each with its own governance and Safe treasury (by Gnosis). What makes these hierarchies stand out as a Fractal DAO is that each ParentDAO has certain on-chain checks and balances over the Sub-DAO that it formed.

##### SubDAO
A Safe treasury that has been spawned by a ParentDAO. This SubDAO always has certain on-chain safe-guards and controls that the ParentDAO has customized on creation. Otherwise, the SubDAO has autonomy to use it's treasury to make on-chain asset decisions with it's own distinct governance method (e.g. Multi-sig vs. Token Vote). Any SubDAO also has the autonomy to additionally become a ParentDAO

##### ParentDAO 
A Safe treasury that has spawned a SubDAO. On creation, the ParentDAO can set the SubDAOs governance method, send it funds and set certain on-chain safe-guards and controls to keep the Parent in some sort of control.

##### RootDAO 
The initial ParentDAO that started the on-chain hierarchies that form the Fractal. This is the only Safe treasury that has no safeguards, checks or balances within the Fractal.
