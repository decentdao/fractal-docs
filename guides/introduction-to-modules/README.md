---
description: Learn to understand and work with modules!
---

# Introduction To Modules

### Getting Started

Modules are an integral part of Fractal Framework: if the MVD is the engine and drivetrain, the modules are the parts that you use to build a car, train, or plane. Modules are community built plug-ins that expand the functionality of the standard DAO execute method.

From a more technical perspective, modules are MVD authorized smart contracts that adhere to a specified interface. Modules are called by the DAO's execute function or utilize the permissions of the DAOaccessContol.sol to execute their own functions. In other terms, modules present code akin to "hooks" that are run at predetermined points.

There is no limit to the modules you can create. Most DAOs will need governance modules, implementation modules, and integration modules.&#x20;

#### Governance Modules

Governance modules allow for more complicated DAO execution control. The MVD allows addresses to call the DAO execution function. These addresses can be EOAs or other smart contracts, which makes it possible to scaffold functionality across multiple contracts. For example, to create a multisig module, first create the multisig module contract, then give the module contract permission to call the DAO contract. Now you have a multisig DAO.

This is just one example of all of the governance possibilities enabled by Fractal. With extensible DAOs, you could even build a DAO controlled by another DAO that is controlled by a multisig.

#### Implementation Modules

Implementation modules add functionality to the DAO's standard execute method. For example, you can create a withdraw module that withdraws funds from a treasury, and that can only be called by the DAO contract. The DAO contract's execute function calls the withdraw module when certain conditions are met. You could create another module that checks for the withdraw conditions and calls the DAO's execute function to make the withdraw. Those conditions could involve a multisig.

#### Integration Modules

Integration modules enable transactions between Ethereum protocols and apps. If you want to make the tokens in your DAO's treasury available for lending, create a contract that can transfer tokens from the treasury contract to the lending platform using the lending platforms developer tools.&#x20;

### Next Up

The [Quick Setup](quick-start.md), [Creating the Module](creating-the-module.md), and [Testing the Module](testing-the-module.md) guides provide a walkthrough of creating a basic implementation module that enables a DAO to control some funds.

Before working with Fractal module's you need to know Solidity, IDEs, Hardhat, and TypeScript.
