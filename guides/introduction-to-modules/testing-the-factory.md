---
description: Testing the factory contract
---

# Testing the Factory

We are going to setup your test-suite, make sure we have the correct types, and make sure it integrates nicely with the your new module.&#x20;

### Types

Then you can add the types to your test file.

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

This is where we setup the tests and make sure we have all the variables we need to properly test our treasury factory.

Let's take a look at the before block. Here we deploy and initialize all the contracts we need for proper integration testing.

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

Here is the easy part. Use waffle and chai matchers to make sure you have the intended behavior. For our tests, we will test that someone can create a new Treasury, and it is properly initialized.

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

Alright that is it, here is what the final code should look like:

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

And on that note, congratulations! You've successfully created and tested a factory module. This means you're now geared-up to start building and proposing your own custom modules to the Fractal-Framework community.

Happy hacking!
