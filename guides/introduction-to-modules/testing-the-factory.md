---
description: Testing the factory contract
---

# Testing the Factory

This guide describes how to set up your test-suite, set the test types, and make sure the factory integrates nicely with your new module.&#x20;

### Set Up Your Test Suite

You may have already set up your test suite as part of the [Testing the Module](testing-the-module.md) guide. If you have not, follow the steps below to install and configure Hardhat:

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

### Types

If you haven't already done so, create a file called TresuryFactory.tests.ts in a /tests directory in your project, then add the types shown below to the file:

```
import { SignerWithAddress } from "@nomiclabs/hardhat-ethers/signers";
import {
  AccessControlDAO,
  AccessControlDAO__factory,
  VotesTokenWithSupply,
  VotesTokenWithSupply__factory,
  CelTreasury,
  CelTreasury__factory,
  TreasuryModuleFactory,
  TreasuryModuleFactory__factory,
} from "../typechain-types";
import chai from "chai";
import { ethers } from "hardhat";

const expect = chai.expect;
```

### Before Each Block

The `beforeEach` block is where you set up tests and make sure you have all the variables you need to properly test the treasury. For testing the factory contract, you must deploy and initialize all the contracts needed for integration testing, as shown below:

```
beforeEach(async function () {
  [deployer, userA] = await ethers.getSigners();

  accessControl = await new AccessControlDAO__factory(deployer).deploy();
  treasury = await new CelTreasury__factory(deployer).deploy();
  treasuryFactory = await new TreasuryModuleFactory__factory(
    deployer
  ).deploy();
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

  await treasuryFactory.initialize();
```

### Contract Tests

To run the tests, use [waffle and chai matchers](https://ethereum-waffle.readthedocs.io/en/latest/matchers.html) to make sure your contracts have the intended behavior. For these tests, make sure that someone can use the factory to create a new treasury module, and that the module is properly initialized:

```
it("Creates a treasury", async () => {
  const abiCoder = new ethers.utils.AbiCoder();
  
  const data = [
    abiCoder.encode(["address"], [accessControl.address]),
    abiCoder.encode(["address"], [treasury.address]),
    abiCoder.encode(["address"], [erc20TokenAlpha.address]),
    abiCoder.encode(["bytes32"], [ethers.utils.formatBytes32String("hi")]),
  ];
  const returnedContract = await treasuryFactory.callStatic.create(data);
  await treasuryFactory.create(data);
  treasuryCreated = CelTreasury__factory.connect(
    returnedContract[0],
    deployer
  );
  expect(await treasuryCreated.accessControl()).to.eq(
    accessControl.address
  );
  expect(await treasuryCreated.celToken()).to.eq(erc20TokenAlpha.address);
});
```

### Recap

That's it! Here is what the final code should look like:

```
import { SignerWithAddress } from "@nomiclabs/hardhat-ethers/signers";
import {
  AccessControlDAO,
  AccessControlDAO__factory,
  VotesTokenWithSupply,
  VotesTokenWithSupply__factory,
  CelTreasury,
  CelTreasury__factory,
  TreasuryModuleFactory,
  TreasuryModuleFactory__factory,
} from "../typechain-types";
import chai from "chai";
import { ethers } from "hardhat";

const expect = chai.expect;

describe("Treasury Factory", function () {
  let accessControl: AccessControlDAO;
  let treasury: CelTreasury;
  let treasuryCreated: CelTreasury;
  let treasuryFactory: TreasuryModuleFactory;

  // eslint-disable-next-line camelcase
  let erc20TokenAlpha: VotesTokenWithSupply;
  let deployer: SignerWithAddress;
  let userA: SignerWithAddress;

  describe("Treasury Factory Deploys Treasuries", function () {
    beforeEach(async function () {
      [deployer, userA] = await ethers.getSigners();

      accessControl = await new AccessControlDAO__factory(deployer).deploy();
      treasury = await new CelTreasury__factory(deployer).deploy();
      treasuryFactory = await new TreasuryModuleFactory__factory(
        deployer
      ).deploy();
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

      await treasuryFactory.initialize();
    });

    it("Creates a treasury", async () => {
      const abiCoder = new ethers.utils.AbiCoder();

      const data = [
        abiCoder.encode(["address"], [accessControl.address]),
        abiCoder.encode(["address"], [treasury.address]),
        abiCoder.encode(["address"], [erc20TokenAlpha.address]),
        abiCoder.encode(["bytes32"], [ethers.utils.formatBytes32String("hi")]),
      ];
      const returnedContract = await treasuryFactory.callStatic.create(data);
      await treasuryFactory.create(data);
      treasuryCreated = CelTreasury__factory.connect(
        returnedContract[0],
        deployer
      );
      expect(await treasuryCreated.accessControl()).to.eq(
        accessControl.address
      );
      expect(await treasuryCreated.celToken()).to.eq(erc20TokenAlpha.address);
    });
  });
});
```

Make sure the tests pass by running the command:

```
$ npx hardhat test
```

Congratulations! You've successfully created and tested a factory module. This means you're now geared-up to start building and proposing your own custom modules to the Fractal-Framework community.

Happy hacking!
