# Developer Overview

The Fractal Protocol supports creating and managing DAOs by deploying contracts that establish a minimum viable DAO that can be extended using modules:

* Minimum viable DAO (MVD) - At a minimum an on-chain DAO must be able to execute code on the blockchain and restrict access to that execution. To add flexibility, a core Fractal DAO can also be extended using modules. The MVD is defined by the `fractal-core` contracts. See the [Fractal Core reference](reference/fractal-core/) material for details.
* Modules - Modules add functionality or extend the capability of a Fractal DAO. For example, Fractal provides, governance, treasury, and voting modules. To adhere to the Fractal Framework, modules implement the `ModuleBase.sol.` See the [Modules reference](reference/modules/) material for details.

Fractal's architecture targets the following design goals:

* Minimal base layer - Everything required to deploy an MVD is included in three contracts.
* Functionality extended by modules - Modularity means that you can deploy a DAO that only implements the features needed to support your use cases.
* Open development layer - Module interfaces make it easy to build on top of the framework.
* No gatekeeping - Fractal cannot control who deploys or accesses Fractal DAOs, or which modules are built or added to any DAOs.
* Expressive access controls - The access control module, which is included with every MVD deployment, supports groups, roles, and actions, which make it easy to define permissions for every aspect of your organization.

See the [Reference](reference/) documentation for details about every contract.



