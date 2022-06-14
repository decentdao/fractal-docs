---
description: Build your first module!
---

# Creating a Module

### Creating the Contract

So let's create a simple implementation module that lets the DAO control some funds. The smart contract will allow anyone to deposit celsius, and only we can withdraw it. Ofcourse, you would want to also deposit Terra into your treasury, But just for with it for this demonstration.

Let's start off by creating a file called CelTreasury.sol in the contracts/ folder. We will be working with solidity 0.8.10, so we'll use that as our pragma.

Since we are building a module, let's import/install the moduleBase from the core Fractal Framework and import the interface needed to safety deposit erc20 tokens into a contract.



```
// Install Fractal-MVD NPM Packagae
npm i @fractal-framework/core-contracts
```

```
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import "@fractal-framework/core-contracts/contracts/ModuleBase.sol";
```

Next, Let's define our contract, we will inherit from the imported contract and cast IERC20 with the SafeERC20 library.

```
...
contract CelTreasury is ModuleBase {
  using SafeERC20 for IERC20;
  
}
```

At this point, your linter or compiler is probably pretty upset, and with good reason! We're inheriting from an interface, but we aren't implementing any of the functions. The interface is like an outline, we've got to fill in the blanks now, and implement our functions!

That's right, it's time to actually build the contract. ![:sunglasses:](https://docs.lens.xyz/img/emojis/sunglasses.png)

### Implementing ModuleBase Functions

So it we look at ModuleBase.sol, we will find the functions required to turn your contract into a Fractal Module! Do you see the function called \_initBase? This will set the module up for you and provide you access to all the cool Fractal features.

To implement this function, create a function called initialize, give it the initializer modifier, and implement \_initBase.

```
function initialize(address _accessControl) external initializer {
    __initBase(_accessControl, msg.sender, "CelTreasury");
}
```

We utilize initialize instead of a constructor to provide each module with upgradability. This is standard with each module that utilizes ModuleBase.sol.

### Implement Custom Logic

Alright, time for the fun part. Now, we can add the deposit and withdraw function to our contract. Let's start off with an easy deposit function. We should allow a user to deposit some celToken. For this, we will need to know the address of celToken, so we will need to add this to storage.

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

Perfect - now we got some tokens in the treasury. Let's make sure no one can take them out! (besides us...). In order to accomplish this - we need a withdraw function with a special modifier called authorized.&#x20;

In short, the modifier checks the msg.sender to see if they have permissions to access this function. All the permissions are stored on the DAOAccessContol.sol.

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

### &#x20; Recap

And that's it! You've successfully created your own module. Let's take a look at our full `CelTreasury.sol` file:

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

Before we move on the testing, let's make sure everything compiles:

```
$ npm run compile
```

Assuming nothing broke, let's go ahead and write a Hardhat task testing that everything works as intended. It's time to put this shiny new module to good use!



\
