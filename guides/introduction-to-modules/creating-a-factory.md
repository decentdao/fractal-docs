---
description: Building a factory for your module
---

# Creating a Factory

### Creating The Factory

So you may be asking, Decent... why do I need to create a factory contract? Well the simple answer is - other DAO creators would most likely like to use your module! So these users need a simple way to create/deploy their own module. That's where the factory comes in.\
\
Next question is what is a factory contract? Well it is a contract... that knows the bytecode of another contract. Therefore, by utilizing the create opcode - you can deploy this bytecode at an address on chain. It is like an factory that builds cars but instead we are building contracts.

Cool. So we know why so know we have to figure out how. That is why I am here. In the same repo - create a new file called TreasuryModuleFactory.sol and define our contract, we will inherit from the imported ModuleFactoryBase and ERC165. We will also import Create2 and ERC1967Proxy so we may [deterministically](https://docs.openzeppelin.com/cli/2.8/deploying-with-create2) create [UUPS contracts](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable).&#x20;

```
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import "@openzeppelin/contracts/proxy/ERC1967/ERC1967Proxy.sol";
import "@openzeppelin/contracts/utils/Create2.sol";
import "./CelTreasury.sol";

import "@fractal-framework/core-contracts/contracts/ModuleFactoryBase.sol";

/// @notice A factory contract for deploying Treasury Modules
contract TreasuryModuleFactory is
    ERC165,
    ModuleFactoryBase
{
}

```

### Implementing ModuleFactoryBase Functions

So if we look at ModuleFactoryBase.sol, we will find the functions required to turn your contract into a Fractal Module! Do you see the function called \_\_initFactoryBase? This will set the factory contract up so you can notify the community of new versions of TreasuryModules.

To implement this function, create a function called initialize, give it the initializer modifier, and implement \_\_initFactoryBase.

```
function initialize() external initializer {
    __initFactoryBase();
}
```

The function we should implement is called create. This is the main function DAO will utilize to create and deploy new modules. This method takes an array of bytes which are decoded within the function to help initialize/setup each module.&#x20;

{% hint style="info" %}
The salt - is an encoded array of bytes used by Create2 to determine the contract address of the deployed treasury module. This salt value is hashed together with tx.orgin, the chain.Id to create unique salt values per user and contract deployment.
{% endhint %}

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
    address celToken = abi.decode(data[2], (address));
    bytes32 salt = abi.decode(data[3], (bytes32));

    createdContracts[0] = Create2.deploy(
        0,
        keccak256(abi.encodePacked(tx.origin, block.chainid, salt)),
        abi.encodePacked(
            type(ERC1967Proxy).creationCode,
            abi.encode(treasuryImplementation, "")
        )
    );
    CelTreasury(payable(createdContracts[0])).initialize(accessControl, celToken);

    return createdContracts;
}
```

The method is supports interface. This simple function provides the UI an easy way to determine if the contract in question is a contract which inherits from moduleFactoryBase.sol.

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

And that's it! You've successfully created your module's factory. Let's take a look at our full TreasuryModuleFactory`.sol` file:

```
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/utils/introspection/ERC165.sol";
import "@openzeppelin/contracts/proxy/ERC1967/ERC1967Proxy.sol";
import "@openzeppelin/contracts/utils/Create2.sol";
import "./CelTreasury.sol";

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
        address celToken = abi.decode(data[2], (address));
        bytes32 salt = abi.decode(data[3], (bytes32));

        createdContracts[0] = Create2.deploy(
            0,
            keccak256(abi.encodePacked(tx.origin, block.chainid, salt)),
            abi.encodePacked(
                type(ERC1967Proxy).creationCode,
                abi.encode(treasuryImplementation, "")
            )
        );
        CelTreasury(payable(createdContracts[0])).initialize(accessControl, celToken);

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

Before we move on the testing, let's make sure everything compiles:

```
$ npm run compile
```

Assuming nothing broke, let's go ahead and write a Hardhat task testing that everything works as intended. It's time to put this shiny new module to good use!
