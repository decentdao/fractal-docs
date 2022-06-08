---
description: Definitions of the Fractal contracts.
---

# Reference

Fractal's software supports creating modular DAO contracts on the Ethereum blockchain. The basic DAO contracts created by Fractal include the following capabilities: execute code on the blockchain, restrict the addresses that can call the contract's execute function, and add modules to the contract to support additional functionality.

In order to support the basic functions of the DAO and introduce modularity, Fractal separates it's code into two categories:

* [Core](fractal-core/) - The portions of the software that apply to every DAO: function execution, access control, extensibility.
* [Modules](modules/) - Contracts that add functionality to the DAO. Currently Fractal offers governance and treasury modules.
