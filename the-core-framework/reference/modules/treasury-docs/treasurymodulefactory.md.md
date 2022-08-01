---
description: A factory contract for deploying Treasury Modules
---

# TreasuryModuleFactory

### Methods

#### addVersion

```solidity
function addVersion(string _semanticVersion, string _frontendURI, address _impl) external nonpayable
```

_add a new version to update module users_

**Parameters**

| Name              | Type    | Description                      |
| ----------------- | ------- | -------------------------------- |
| \_semanticVersion | string  | semantic version control         |
| \_frontendURI     | string  | IPFS hash of the static frontend |
| \_impl            | address | address of the impl              |

#### create

```solidity
function create(address creator, bytes[] data) external nonpayable returns (address[])
```

_Creates a Treasury module_

**Parameters**

| Name    | Type     | Description                                  |
| ------- | -------- | -------------------------------------------- |
| creator | address  | The address creating the module              |
| data    | bytes\[] | The array of bytes used to create the module |

**Returns**

| Name | Type       | Description                                             |
| ---- | ---------- | ------------------------------------------------------- |
| \_0  | address\[] | address\[] The array of addresses of the created module |

#### initialize

```solidity
function initialize() external nonpayable
```

Function for initializing the contract that can only be called once

#### owner

```solidity
function owner() external view returns (address)
```

_Returns the address of the current owner._

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | address | undefined   |

#### renounceOwnership

```solidity
function renounceOwnership() external nonpayable
```

_Leaves the contract without owner. It will not be possible to call `onlyOwner` functions anymore. Can only be called by the current owner. NOTE: Renouncing ownership will leave the contract without an owner, thereby removing any functionality that is only available to the owner._

#### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) external view returns (bool)
```

_See {IERC165-supportsInterface}._

**Parameters**

| Name        | Type   | Description |
| ----------- | ------ | ----------- |
| interfaceId | bytes4 | undefined   |

**Returns**

| Name | Type | Description |
| ---- | ---- | ----------- |
| \_0  | bool | undefined   |

#### transferOwnership

```solidity
function transferOwnership(address newOwner) external nonpayable
```

_Transfers ownership of the contract to a new account (`newOwner`). Can only be called by the current owner._

**Parameters**

| Name     | Type    | Description |
| -------- | ------- | ----------- |
| newOwner | address | undefined   |

#### versionControl

```solidity
function versionControl(uint256) external view returns (string semanticVersion, string frontendURI, address impl)
```

**Parameters**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

**Returns**

| Name            | Type    | Description |
| --------------- | ------- | ----------- |
| semanticVersion | string  | undefined   |
| frontendURI     | string  | undefined   |
| impl            | address | undefined   |

### Events

#### OwnershipTransferred

```solidity
event OwnershipTransferred(address indexed previousOwner, address indexed newOwner)
```

**Parameters**

| Name                    | Type    | Description |
| ----------------------- | ------- | ----------- |
| previousOwner `indexed` | address | undefined   |
| newOwner `indexed`      | address | undefined   |

#### TreasuryCreated

```solidity
event TreasuryCreated(address indexed treasuryAddress, address indexed accessControl)
```

**Parameters**

| Name                      | Type    | Description |
| ------------------------- | ------- | ----------- |
| treasuryAddress `indexed` | address | undefined   |
| accessControl `indexed`   | address | undefined   |

#### VersionCreated

```solidity
event VersionCreated(string semanticVersion, string frontendURI, address impl)
```

**Parameters**

| Name            | Type    | Description |
| --------------- | ------- | ----------- |
| semanticVersion | string  | undefined   |
| frontendURI     | string  | undefined   |
| impl            | address | undefined   |
