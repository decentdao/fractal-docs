---
description: Building a factory for your module
---

# Creating a Factory

Factory contracts make it possible for other DAO creators to use your module. By providing a simple way to create/deploy modules, Fractal can enable broad organizational models without having to anticipate every DAO's needs.\
\
Technically speaking, a Factory contract is a contract knows the bytecode of another contract and uses a create opcode function to deploy that contract at another address.

This guide describes how to create a factory contract for the Treasury module you [created](creating-the-module.md) and [tested](testing-the-module.md) in the previous guides. To follow along, look at the TreasuryModuleFactory.sol contract in the /contracts directory of the [Fractal Module Example repo](https://github.com/decent-dao/fractal-module-example).

### Creating The Factory

In the /contracts directory of your module project, create a new file called TreasuryModuleFactory.sol. The contract should inherit from the imported ModuleFactoryBase and ERC165. Also import Create2 and ERC1967Proxy so you can [deterministically](https://docs.openzeppelin.com/cli/2.8/deploying-with-create2) create [UUPS contracts](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable).

```
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import "@openzeppelin/contracts/proxy/ERC1967/ERC1967Proxy.sol";
import "@openzeppelin/contracts/utils/Create2.sol";
import "./Treasury.sol";

import "@fractal-framework/core-contracts/contracts/ModuleFactoryBase.sol";

/// @notice A factory contract for deploying Treasury Modules
contract TreasuryModuleFactory is
    ERC165,
    ModuleFactoryBase
{
}

```

### Implementing ModuleFactoryBase Functions

To add functionality to your factory module, you will implement three methods: `initialize`, `create`, and `supportsInterface`.

#### Initialize

Look at [ModuleFactoryBase.sol](https://github.com/decent-dao/fractal-core-contracts/blob/main/contracts/ModuleFactoryBase.sol) in the Fractal Core Contracts repo to find the functions required to turn your contract into a Fractal Module. The `__initFactoryBase` function sets the factory contract up so that you can notify the community of new versions of the module.

To implement the `__initFactoryBase` function in your factory, create a function called `initialize`, give it the `initializer` modifier, and implement `__initFactoryBase`:

```
function initialize() external initializer {
    __initFactoryBase();
}
```

#### Create

Next, add a `create` method to your factory. This will be the method that the DAO uses to create and deploy new modules. This method takes an array of bytes and decodes them to help initialize/setup each module.

{% hint style="info" %}
The salt variable created in line 14 below is an encoded array of bytes used by Create2 to determine the contract address of the deployed treasury module. This salt value is hashed together with tx.orgin, the chain.Id to create unique salt values per user and contract deployment.
{% endhint %}

The code below shows an example of the `create` method:

```
/// @dev Creates a Treasury module
/// @param data The array of bytes used to create the module
/// @return address[] The array of addresses of the created module
function create(bytes[] calldata data)
    external
    override
    returns (address[] memory)
{
    address[] memory createdContracts = new address[](1);

    address accessControl = abi.decode(data[0], (address));
    address treasuryImplementation = abi.decode(data[1], (address));
    address tokenAddress = abi.decode(data[2], (address));
    bytes32 salt = abi.decode(data[3], (bytes32));

    createdContracts[0] = Create2.deploy(
        0,
        keccak256(abi.encodePacked(tx.origin, block.chainid, salt)),
        abi.encodePacked(
            type(ERC1967Proxy).creationCode,
            abi.encode(treasuryImplementation, "")
        )
    );
    Treasury(payable(createdContracts[0])).initialize(accessControl, tokenAddress);

    return createdContracts;
}
```

#### Supports Interface

Finally, add a`supportsInterface` method. This method provides an easy way for the UI to determine whether the contract inherits from ModuleFactoryBase.sol.

```
/// @notice Returns whether a given interface ID is supported
/// @param interfaceId An interface ID bytes4 as defined by ERC-165
/// @return bool Indicates whether the interface is supported
function supportsInterface(bytes4 interfaceId)
    public
    view
    virtual
    override(ERC165, ModuleFactoryBase)
    returns (bool)
{
    return
        interfaceId == type(IModuleFactory).interfaceId ||
        super.supportsInterface(interfaceId);
}
```

### Recap

That's it! You've successfully created your module's factory. Here is the complete `TreasuryModuleFactory.sol` file:

```
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import "@openzeppelin/contracts/proxy/ERC1967/ERC1967Proxy.sol";
import "@openzeppelin/contracts/utils/Create2.sol";
import "./Treasury.sol";

import "@fractal-framework/core-contracts/contracts/ModuleFactoryBase.sol";

/// @notice A factory contract for deploying Treasury Modules
contract TreasuryModuleFactory is
    ERC165,
    ModuleFactoryBase
{
    function initialize() external initializer {
        __initFactoryBase();
    }

    /// @dev Creates a Treasury module
    /// @param data The array of bytes used to create the module
    /// @return address[] The array of addresses of the created module
    function create(bytes[] calldata data)
        external
        override
        returns (address[] memory)
    {
        address[] memory createdContracts = new address[](1);

        address accessControl = abi.decode(data[0], (address));
        address treasuryImplementation = abi.decode(data[1], (address));
        address tokenAddress = abi.decode(data[2], (address));
        bytes32 salt = abi.decode(data[3], (bytes32));

        createdContracts[0] = Create2.deploy(
            0,
            keccak256(abi.encodePacked(tx.origin, block.chainid, salt)),
            abi.encodePacked(
                type(ERC1967Proxy).creationCode,
                abi.encode(treasuryImplementation, "")
            )
        );
        CelTreasury(payable(createdContracts[0])).initialize(accessControl, tokenAddress);

        return createdContracts;
    }

    /// @notice Returns whether a given interface ID is supported
    /// @param interfaceId An interface ID bytes4 as defined by ERC-165
    /// @return bool Indicates whether the interface is supported
    function supportsInterface(bytes4 interfaceId)
        public
        view
        virtual
        override(ERC165, ModuleFactoryBase)
        returns (bool)
    {
        return
            interfaceId == type(IModuleFactory).interfaceId ||
            super.supportsInterface(interfaceId);
    }
}
```

Before moving on to testing, make sure everything compiles:

```
$ npm run compile
```

Assuming nothing broke, the next step is to write a Hardhat task that [tests](testing-the-factory.md) whether everything works as intended.
