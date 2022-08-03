---
description: Manually deploy a custom DAO from the meta-factory contract.
---

# Deploy Custom DAO

If you want to create a DAO with some custom modules that are not available from the frontend, you will need to use the Metafactory to set up and deploy your custom DAO. The Metafactory is defined in the [Metafactory.sol](https://github.com/decent-dao/fractal-metafactory/blob/main/contracts/MetaFactory.sol) contract in the contracts directory of the [fractal-metafactory repo](https://github.com/decent-dao/fractal-metafactory).

Metafactory.sol contains one external function named `createDAOAndExecute`:

```
/// @notice Creates a DAO, Access Control, and any modules specified
/// @param daoFactory The address of the DAO factory
/// @param createDAOParams The struct of parameters used for creating the DAO and Access Control contracts
/// @param targets An array of addresses to target for the function calls
/// @param values An array of ether values to send with the function calls
/// @param calldatas An array of bytes defining the function calls
function createDAOAndExecute(
  address daoFactory,
  IDAOFactory.CreateDAOParams memory createDAOParams,
  address[] calldata targets,
  uint256[] calldata values,
  bytes[] calldata calldatas
) external {
  createDAO(daoFactory, createDAOParams);
  execute(targets, values, calldatas);
}
```

The `createDAOAndExectute` function calls two internal functions:

* `createDAO` - Utilizes the DAOFactory.sol contract to create the DAO and AccessControl.sol contracts for your custom DAO.
* `execute` - Executes low-level calls to any contracts that you specified in the `targets` parameter.

The example below shows how to use the Metafactory to create a custom DAO:

1\. Choose your network. This example uses the Goreli testnet.

2\. Choose your daoFactory.sol by setting the contract address based on your network. For this example, use 0xEc42913bB12d8976BADf0000151B679Dc4e549BC.

3\. Define values for `createDAOParams`. The `createDAOParams` values will create a proxy contract that references the include `daoImpl` and `AccessControlImpl`.&#x20;

* DAO Impl - 0x8A96a09b48e6968bF4e1Ac068C56446F4606F339
* DAOAccessControl Impl - 0x300Ce4fe07fbF0F1021060eCb788A9956810D920
* Salt values predetermine a contract's address to make it simple to reference a contract before the contract is fully deployed.
* The `roles` parameter determines the authorizations required for each module. For this example, only the `WITHDRAWER_ROLE` is required.
* Use the `roleAdmins` parameter to create one or more mangers. Managers will be able to update the roles of other members.
* The `members` parameter defines the addresses that will have the role types specified in the `roles` array:&#x20;
  * EXECUTE\_ROLE&#x20;
    * `metaFactory.address` - During DAO creation, the Metafactory contract needs to have the execute role so that it can call `execute` on the DAO contract to setup the  system. Remove the Metafactory from the member list at the end of the transaction.
    * `executor.address` - The address that can call the DAO's execute function.
  * WITHDRAWER\_ROLE - In this case, we only want the DAO to be able to withdraw from the treasury.&#x20;
* `daoFunctionDescs` and `daoActionRoles` set up the DAO's roles in the access control contract. This can remain the same.

```
const createDAOParams = {
  daoImplementation: daoImpl.address,
  daoFactory: daoFactory.address,
  accessControlImplementation: accessControlImpl.address,
  salt: ethers.utils.formatBytes32String("randombytes"),
  daoName: "TestDao",
  roles: [
    "EXECUTE_ROLE",
    "WITHDRAWER_ROLE",
  ],
  rolesAdmins: ["DAO_ROLE", "DAO_ROLE"],
  members: [
    [metaFactory.address, executor.address],
    [dao.address],
  ],
  daoFunctionDescs: [
    "execute(address[],uint256[],bytes[])",
  ],
  daoActionRoles: [["EXECUTE_ROLE"]],
};
```

4\. Next, create the `calldata` required to execute methods from the Metafactory:

```
const abiCoder = new ethers.utils.AbiCoder();
const { chainId } = await ethers.provider.getNetwork();
const predictedDAOAddress = ethers.utils.getCreate2Address(
  daoFactory.address,
  ethers.utils.solidityKeccak256(
    ["address", "uint256", "bytes32"],
    [
      deployer.address,
      chainId,
      ethers.utils.formatBytes32String("randombytes"),
    ]
  ),
  ethers.utils.solidityKeccak256(
    ["bytes", "bytes"],
    [
      // eslint-disable-next-line camelcase
      ERC1967Proxy__factory.bytecode,
      abiCoder.encode(["address", "bytes"], [daoImpl.address, []]),
    ]
  )
);
const predictedAccessControlAddress = ethers.utils.getCreate2Address(
  daoFactory.address,
  ethers.utils.solidityKeccak256(
    ["address", "uint256", "bytes32"],
    [
      deployer.address,
      chainId,
      ethers.utils.formatBytes32String("randombytes"),
    ]
  ),
  ethers.utils.solidityKeccak256(
    ["bytes", "bytes"],
    [
      // eslint-disable-next-line camelcase
      ERC1967Proxy__factory.bytecode,
      abiCoder.encode(
        ["address", "bytes"],
        [accessControlImpl.address, []]
      ),
    ]
  )
);
const predictedTreasuryAddress = ethers.utils.getCreate2Address(
  treasuryFactory.address,
  ethers.utils.solidityKeccak256(
    ["address", "uint256", "bytes32"],
    [
      deployer.address,
      chainId,
      ethers.utils.formatBytes32String("treasurySalt"),
    ]
  ),
  ethers.utils.solidityKeccak256(
    ["bytes", "bytes"],
    [
      // eslint-disable-next-line camelcase
      ERC1967Proxy__factory.bytecode,
      abiCoder.encode(["address", "bytes"], [treasuryImpl.address, []]),
    ]
  )
);
```

5\. Finally, create the `calldata` that will set up the treasury and access control contracts.

6\. Set the call data define below to deploy a Treasury contract from the TreasuryFactory:

* `predictedAddress` - This address is the same as the `predictedAddress` parameter defined above.
* `treasuryImpl.address` - This address is created after deploying your first treasury.sol contract.
* The salt value as defined above.
* `calldata`  - Created using `encodeFunctionData`.

```
const treasuryData = [
  abiCoder.encode(["address"], [predictedAccessControlAddress]),
  abiCoder.encode(["address"], [treasuryImpl.address]),
  abiCoder.encode(
    ["bytes32"],
    [ethers.utils.formatBytes32String("treasurySalt")]
  ),
];

const treasuryFactoryCalldata =
  treasuryFactory.interface.encodeFunctionData("create", [treasuryData]);

```

7\. Set up access control with the function sigs required for treasury authorizations.

8\. Encode the data within the DAO execute function sigs so it can make the call to the DAOAccessControl to add the new roles

```
const innerAddActionsRolesCalldata =
  accessControl.interface.encodeFunctionData("daoAddActionsRoles", [
    [
      predictedTreasuryAddress,
    ],
    [
      "withdrawERC20Tokens(address[],address[],uint256[])",
    ],
    [
      ["WITHDRAWER_ROLE"],
    ],
  ]);

const outerAddActionsRolesCalldata = dao.interface.encodeFunctionData(
  "execute",
  [[accessControl.address], [0], [innerAddActionsRolesCalldata]]
);
```

9\. Finally, remove the metafactory from the executerole so the system is secured.

```
const revokeMetafactoryRoleCalldata =
  accessControl.interface.encodeFunctionData("userRenounceRole", [
    "EXECUTE_ROLE",
    metaFactory.address,
  ]);
```

That's it! You're ready to execute the transaction to create your DAO. Here is what the transaction looks like all together:

```
tx = await metaFactory.createDAOAndExecute(
  daoFactory.address,
  createDAOParams,
  [
    treasuryFactory.address,
    dao.address,
    accessControl.address,
  ],
  [0, 0, 0, 0, 0],
  [
    treasuryFactoryCalldata,
    outerAddActionsRolesCalldata,
    revokeMetafactoryRoleCalldata,
  ],
  {
    gasLimit: 30000000,
  }
);
```

There you go! You deployed your very own DAO with your own custom module... They grow up so fast. 😭😭😭
