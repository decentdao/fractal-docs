---
description: Deploy and interact with your module locally!
---

# Testing the Module

There comes a time in everyone's life when they find out whether they are up to the task. For you, today is that day: put your module to the test!

This guide covers setting up your test-suite, and testing your module to make sure it has the correct types and that it integrates with the MVD.&#x20;

### Types

First, install `hardhat-dependency-compiler` in your project directory so that your test will be able to access the MVD:

```
npm i hardhat-dependency-compiler
```

{% hint style="info" %}
See [https://www.npmjs.com/package/hardhat-dependency-compiler](https://www.npmjs.com/package/hardhat-dependency-compiler) for details.
{% endhint %}

Then, import the MVD and CelTreasury contracts by adding the code below to you hardhat.config file:

```
dependencyCompiler: {
paths: [
    "@fractal-framework/core-contracts/contracts/DAO.sol",
    "@fractal-framework/core-contracts/contracts/AccessControlDAO.sol",
  ],
},
```

Finally, add the types to your test file:

```
import { SignerWithAddress } from "@nomiclabs/hardhat-ethers/signers";
import {
  AccessControlDAO,
  AccessControlDAO__factory,
  DAO,
  DAO__factory,
  VotesTokenWithSupply,
  VotesTokenWithSupply__factory,
  CelTreasury,
  CelTreasury__factory,
} from "../typechain-types";
import chai from "chai";
import { ethers } from "hardhat";

const expect = chai.expect;
```

### Mocks

Add a mock ERC20 token to your contracts for easy testing. If you would like to create your own - please do. For this tutorial, you can copy the code found in the mocks folder of this GitHub repo: [https://github.com/christopherdancy/treasury-module-example](https://github.com/christopherdancy/treasury-module-example).

### Before Each Block

The `beforeEach` block is where you set up tests and make sure you have all the variables you need to properly test the treasury. Most importantly, set up DAOAccessControl to ensure that authorizations are working.

The `beforeEach` block below performs several large tasks:

* Deploys and initializes the contracts needed for integration testing
* Defines roles and authorizations for the AccessControl functions
* Defines the values used by the treasury module.

Here we deploy and initialize all the contracts we need for proper integration testing. If we look a little closer at Access Control, we are passing in a role for the withdraw function, a list of addresses that have that role, a contract address, a function sig, and the role which is authorized to call this function sig.

This is what makes the access control unique, each method on a contract has an authorized modifier which determines who can call the method.&#x20;

```
beforeEach(async function () {
      [deployer, withdrawer, userA] = await ethers.getSigners();

      dao = await new DAO__factory(deployer).deploy();
      accessControl = await new AccessControlDAO__factory(deployer).deploy();
      treasury = await new CelTreasury__factory(deployer).deploy();

      await accessControl
        .connect(deployer)
        .initialize(
          dao.address,
          [withdrawerRoleString],
          [daoRoleString],
          [[withdrawer.address]],
          [treasury.address],
          ["withdrawERC20Tokens(address,uint256)"],
          [[withdrawerRoleString]]
        );

      erc20TokenAlpha = await new VotesTokenWithSupply__factory(
        deployer
      ).deploy(
        "ALPHA",
        "ALPHA",
        [treasury.address, userA.address],
        [
          ethers.utils.parseUnits("100.0", 18),
          ethers.utils.parseUnits("100.0", 18),
        ],
        ethers.utils.parseUnits("200", 18),
        treasury.address
      );
      await treasury
        .connect(deployer)
        .initialize(accessControl.address, erc20TokenAlpha.address);

      await erc20TokenAlpha
        .connect(userA)
        .approve(treasury.address, ethers.utils.parseUnits("100.0", 18));
    });
```

{% hint style="info" %}
In the code above, you can see that`accessControl` is set on each method. Function-level, role-based permissions is a powerful feature of Fractal DAOs.
{% endhint %}

### Contract Tests

To run the tests, use [waffle and chai matchers](https://ethereum-waffle.readthedocs.io/en/latest/matchers.html) to make sure your contracts have the intended behavior. The tests below check that someone can deposit celToken, and that only a specified address can withdraw the tokens:

```
it("Receives ERC-20 tokens using the deposit function", async () => {
  await treasury
    .connect(userA)
    .depositERC20Tokens(ethers.utils.parseUnits("50.0", 18));

  expect(await erc20TokenAlpha.balanceOf(userA.address)).to.equal(
    ethers.utils.parseUnits("50.0", 18)
  );
  expect(await erc20TokenAlpha.balanceOf(treasury.address)).to.equal(
    ethers.utils.parseUnits("150.0", 18)
  );
});

it("Sends ERC-20 tokens using the withdraw function", async () => {
  await treasury
    .connect(withdrawer)
    .withdrawERC20Tokens(
      withdrawer.address,
      ethers.utils.parseUnits("100.0", 18)
    );

  expect(await erc20TokenAlpha.balanceOf(withdrawer.address)).to.equal(
    ethers.utils.parseUnits("100.0", 18)
  );
  expect(await erc20TokenAlpha.balanceOf(treasury.address)).to.equal(
    ethers.utils.parseUnits("0", 18)
  );
});

it("Reverts when a non authorized user attempts to withdraw ERC-20 tokens", async () => {
  await expect(
    treasury
      .connect(userA)
      .withdrawERC20Tokens(
        withdrawer.address,
        ethers.utils.parseUnits("50.0", 18)
      )
  ).to.be.revertedWith("NotAuthorized()");
});
```

### Recap

The code below shows the entire test code:

```
import { SignerWithAddress } from "@nomiclabs/hardhat-ethers/signers";
import {
  AccessControlDAO,
  AccessControlDAO__factory,
  DAO,
  DAO__factory,
  VotesTokenWithSupply,
  VotesTokenWithSupply__factory,
  CelTreasury,
  CelTreasury__factory,
} from "../typechain-types";
import chai from "chai";
import { ethers } from "hardhat";

const expect = chai.expect;

describe("Treasury", function () {
  let dao: DAO;
  let accessControl: AccessControlDAO;
  let treasury: CelTreasury;

  // eslint-disable-next-line camelcase
  let erc20TokenAlpha: VotesTokenWithSupply;
  let deployer: SignerWithAddress;
  let withdrawer: SignerWithAddress;
  let userA: SignerWithAddress;

  // Roles
  const daoRoleString = "DAO_ROLE";
  const withdrawerRoleString = "WITHDRAWER_ROLE";

  describe("Treasury supports ERC-20 tokens", function () {
    beforeEach(async function () {
      [deployer, withdrawer, userA] = await ethers.getSigners();

      dao = await new DAO__factory(deployer).deploy();
      accessControl = await new AccessControlDAO__factory(deployer).deploy();
      treasury = await new CelTreasury__factory(deployer).deploy();

      await accessControl
        .connect(deployer)
        .initialize(
          dao.address,
          [withdrawerRoleString],
          [daoRoleString],
          [[withdrawer.address]],
          [treasury.address],
          ["withdrawERC20Tokens(address,uint256)"],
          [[withdrawerRoleString]]
        );

      erc20TokenAlpha = await new VotesTokenWithSupply__factory(
        deployer
      ).deploy(
        "ALPHA",
        "ALPHA",
        [treasury.address, userA.address],
        [
          ethers.utils.parseUnits("100.0", 18),
          ethers.utils.parseUnits("100.0", 18),
        ],
        ethers.utils.parseUnits("200", 18),
        treasury.address
      );
      await treasury
        .connect(deployer)
        .initialize(accessControl.address, erc20TokenAlpha.address);

      await erc20TokenAlpha
        .connect(userA)
        .approve(treasury.address, ethers.utils.parseUnits("100.0", 18));
    });

    it("Receives ERC-20 tokens using the deposit function", async () => {
      await treasury
        .connect(userA)
        .depositERC20Tokens(ethers.utils.parseUnits("50.0", 18));

      expect(await erc20TokenAlpha.balanceOf(userA.address)).to.equal(
        ethers.utils.parseUnits("50.0", 18)
      );
      expect(await erc20TokenAlpha.balanceOf(treasury.address)).to.equal(
        ethers.utils.parseUnits("150.0", 18)
      );
    });

    it("Sends ERC-20 tokens using the withdraw function", async () => {
      await treasury
        .connect(withdrawer)
        .withdrawERC20Tokens(
          withdrawer.address,
          ethers.utils.parseUnits("100.0", 18)
        );

      expect(await erc20TokenAlpha.balanceOf(withdrawer.address)).to.equal(
        ethers.utils.parseUnits("100.0", 18)
      );
      expect(await erc20TokenAlpha.balanceOf(treasury.address)).to.equal(
        ethers.utils.parseUnits("0", 18)
      );
    });

    it("Reverts when a non authorized user attempts to withdraw ERC-20 tokens", async () => {
      await expect(
        treasury
          .connect(userA)
          .withdrawERC20Tokens(
            withdrawer.address,
            ethers.utils.parseUnits("50.0", 18)
          )
      ).to.be.revertedWith("NotAuthorized()");
    });
  });
});
```

Use the command below to execute the tests:

```
$ npx hardhat test
```

You've successfully created and tested an entirely new module. This means you're now geared-up to start building and proposing your own custom modules to the Fractal-Framework community.

Happy hacking!



###

###
