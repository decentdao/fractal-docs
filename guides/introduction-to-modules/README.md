---
description: Learn to understand and work with modules!
---

# Introduction To Modules

### Getting Started

Modules are an integral part of Fractal Framework: If the MVD is the engine and drivetrain, the modules are the composable pieces which allow you to build a car, train, or plane. These community built plug-ins greatly expand the functionality of the standard DAO execute method.

From a more technical perspective, modules are MVD authorized smart contracts that adhere to a specified interface which are called by the DAO or utilizes the permissions of the DAOaccessContol.sol. In other terms, modules present code akin to "hooks" that are run at predetermined points.

There is no limit to the different modules you can create; therefore, we will focus on explaining the most likely to be created modules: Governance modules, Implementation Modules, and Integration Modules.&#x20;

#### Governance Modules

Governance modules allow for more complicated DAO execution control. The MVD only allows for addresses to call the DAO execution function. These addresses can be EOAs or other smart contracts. Here is where the magic is, so if you wanted to create a contract which acts as a multi-sig, you can. Create the contract, allow the multi-sig to call the DAO contract, and now you have a multi-sig DAO.

This quick example should open your mind to all of the governance possibilities you could have, you could even have your DAO controlled by another DAO that is controlled by a DAO multi-sig... whoa.

#### Implementation Modules

Implementation modules provide the DAO with more functionality by empowering the standard execute method. So what do I mean... lets say you have a DAO but you would like for your DAO to keep funds in a treasury. Well, that is kind of tough to do without a treasury and a way to access this treasury. So you, as the module creator expert, decide to create a treasury contract that authorizes only the DAO to make withdrawals.

#### Integration Modules

So now the questions on everyone's mind is... what do we do with this billion TUSD in our treasury? Simple, place it on Anchor protocol. Hard, how do I do that from a treasury? Integration Modules! Create a smart contract that is allowed to pull from the treasury and deposit on Anchor. Module done, Money made!

### Next Up

Now that you understand how modules work and that Decent labs loves terra(JK... please do not buy terra, but please make modules for it). It's time to get your hands degen dirty.

We will be creating a custom Implementation Module from scratch (please wash your hands first). Please know Solidity, IDEs, Hardhat, and blah, blah, blah
