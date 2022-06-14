---
description: Deploy and interact with your module locally!
---

# Testing the Module

There comes a time in everyones life where they see if they are as good as they claim to be... Today is that day. Put your module to the test!

We are going to setup your test-suite, make sure we have the correct types, and make sure it integrates nicely with the MVD.&#x20;

### Types

You will need to import the MVD contracts and the CelTreasury. To get access to the MVD, you must use the [https://www.npmjs.com/package/hardhat-dependency-compiler](https://www.npmjs.com/package/hardhat-dependency-compiler).

```
npm i hardhat-dependency-compiler
```

Then add this to the hardhat.config file.

```
dependencyCompiler: {
  paths: [
    "@fractal-framework/core-contracts/contracts/DAO.sol",
    "@fractal-framework/core-contracts/contracts/AccessControlDAO.sol",
  ],
},
```

Then you can add the types to your test file.

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
```

### Mocks

Add a mock ERC20 token to your contracts for easy testing. If you would like to create your own - please do. But for the purpose of this tutorial, you may copy the code found in the mocks folder of this github repo - [https://github.com/christopherdancy/treasury-module-example](https://github.com/christopherdancy/treasury-module-example).

### Before Each Block

This is where we setup the tests and make sure we have all the variables we need to properly test our treasury. Most importantly, we have to set up our DAOAccessControl so that authorizations are accounted for.

Let's take a look at the before block. Here we deploy and initialize all the contracts we need for proper integration testing. If we look a little closer at Access Control, we are passing in a role for the withdraw function, a list of addresses that have that role, a contract address, a function sig, and the role which is authorized to call this function sig.

This is what makes the access control unique, each method on a contract have an authorized modifier which determines who can call this method.&#x20;

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

### Contract Tests

Here is the easy part. Use waffle and chai matchers to make sure you have the intended behavior. For our tests, we will test that someone can deposit celToken, and only the withdrawer can withdraw the tokens.

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

Alright that is it, here is what the final code should look like:

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

Make sure the tests pass by running the command:

```
$ npx hardhat test
```

And on that note, congratulations! You've successfully created and tested an entirely new module. This means you're now geared-up to start building and proposing your own custom modules to the Fractal-Framework community.

Happy hacking!



###

###
