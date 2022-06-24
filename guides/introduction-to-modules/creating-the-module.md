---
description: Build your first module!
---

# Creating the Module

This guide shows how to create a simple implementation module that lets the DAO manage funds. The smart contract allows anyone to deposit CEL tokens, and restricts the addresses that can withdraw them.

{% hint style="info" %}
This guide creates the CelTreasury.sol file in the contracts/ directory of the Treasury Module Example project.
{% endhint %}

Before proceeding, set up your dev environment as described in the [Quick Start](setup.md) guide.

### Create the contract

1. Install the Fractal core contracts in your project directory:

```
npm i @fractal-framework/core-contracts
```

The Fractal core contracts include the module base contracts that your module will implement.

2\. Create a file in your project's contracts/ directory called CelTreasury.sol.&#x20;

3\. In your CelTreasury.sol file, set the `pragma` to `^0.8.0` and import the Fractal `ModuleBase.sol` and OpenZeppelin `SafeERC20.sol` interfaces:

```
// Install Fractal-MVD NPM Packagae
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "@fractal-framework/core-contracts/contracts/ModuleBase.sol";
```

4\. Define your contract. The contract inherits from the imported ModuleBase.sol and casts it as IERC20 using the SafeERC20 library:

```
contract CelTreasury is ModuleBase {
  using SafeERC20 for IERC20;
  
}
```

At this point, your linter or compiler is probably pretty upset, and with good reason! The contract inherits from an interface but doesn't implement any of the functions. The next steps take care of that by building out the contract.

### Implementing ModuleBase Functions

Look at the ModuleBase.sol contract to find the required functions:

* `_initBase` - Sets up the module and provides access to all of Fractal's features.

To implement this function, create a function called `initialize`, give it the `initializer` modifier, and implement `__initBase`:

```
function initialize(address _accessControl) external initializer {
    __initBase(_accessControl, msg.sender, "CelTreasury");
}
```

{% hint style="info" %}
ModuleBase uses an `initialize` method instead of a constructor to allow for upgradable modules.
{% endhint %}

Now that the module is part of the Fractal ecosystem, add functionality by implementing custom logic.

### Implement Custom Logic

Now for the fun part: add the deposit and withdraw functions to your contract.

#### Deposit

The deposit function is simple: it allows a transfer of CEL tokens from any address to the DAO's wallet address.

1. Add the address for CEL to storage in your `contract` definition.
2. Write a `depositERC20Tokens` function that performs the transfer:

```
contract CelTreasury is ModuleBase {
    using SafeERC20 for IERC20;
    address celToken;

    /// @notice Function for initializing the contract that can only be called once
    /// @param _accessControl The address of the access control contract
    function initialize(address _accessControl, address _celToken)
        external
        initializer
    {
        __initBase(_accessControl, msg.sender, "CelTreasury");
        celToken = _celToken;
    }

    function depositERC20Tokens(uint256 amount) external {
        IERC20(celToken).safeTransferFrom(msg.sender, address(this), amount);
    }
}
```

#### Withdraw

Once you've used the deposit function to add some tokens to your treasury, you may want to add a withdraw function, so you can distribute them. The function must also make sure that only authorized DAO members or contracts can withdraw tokens.

To accomplish this, add a modifier called `authorized` to the withdraw function. The modifier checks the `msg.sender` address to see if they have permission to access the function:

```
function withdrawERC20Tokens(
    address recipient,
    uint256 amount
) external authorized {
    IERC20(celToken).safeTransfer(
        recipient,
        amount
    );
}
```

{% hint style="info" %}
Permissions are stored on the DAOAccessContol.sol contract.
{% endhint %}

### Recap

At this point, you've successfully created your own module that transfers funds to and from a treasury. The example below shows the complete CelTreasury.sol file:

```
//SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "@fractal-framework/core-contracts/contracts/ModuleBase.sol";

contract CelTreasury is ModuleBase {
    using SafeERC20 for IERC20;
    address celToken;
    

    /// @notice Function for initializing the contract that can only be called once
    /// @param _accessControl The address of the access control contract
    function initialize(address _accessControl, address _celToken)
        external
        initializer
    {
        __initBase(_accessControl, msg.sender, "CelTreasury");
        celToken = _celToken;
    }

    function depositERC20Tokens(uint256 amount) external authorized {
        IERC20(celToken).safeTransferFrom(msg.sender, address(this), amount);
    }

    function withdrawERC20Tokens(
        address recipient,
        uint256 amount
    ) external authorized {
        IERC20(celToken).safeTransfer(
            recipient,
            amount
        );
    }
}
```

Before we move on the testing, make sure everything compiles:

```
$ npm run compile
```

Once it compiles, move on to [Testing the Module](testing-the-module.md), which describes how to write a Hardhat task that makes sure everything works as intended.



\
