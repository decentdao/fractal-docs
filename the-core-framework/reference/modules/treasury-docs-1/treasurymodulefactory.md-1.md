# ClaimSubsidiary

### Methods

#### accessControl

```solidity
function accessControl() external view returns (contract IDAOAccessControl)
```

**Returns**

| Name | Type                       | Description |
| ---- | -------------------------- | ----------- |
| \_0  | contract IDAOAccessControl | undefined   |

#### cToken

```solidity
function cToken() external view returns (address)
```

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | address | undefined   |

#### calculateClaimAmount

```solidity
function calculateClaimAmount(address claimer) external view returns (uint256 cTokenAllocation)
```

Calculate a users cToken allocation

**Parameters**

| Name    | Type    | Description                        |
| ------- | ------- | ---------------------------------- |
| claimer | address | Address which is being claimed for |

**Returns**

| Name             | Type    | Description             |
| ---------------- | ------- | ----------------------- |
| cTokenAllocation | uint256 | Users cToken allocation |

#### claimSnap

```solidity
function claimSnap(address claimer) external nonpayable
```

This function allows pToken holders to claim cTokens

**Parameters**

| Name    | Type    | Description                        |
| ------- | ------- | ---------------------------------- |
| claimer | address | Address which is being claimed for |

#### initialize

```solidity
function initialize(address _metaFactory, address _accessControl, address _pToken, address _cToken, uint256 _pAllocation) external nonpayable
```

Initilize Claim Contract

**Parameters**

| Name            | Type    | Description                                             |
| --------------- | ------- | ------------------------------------------------------- |
| \_metaFactory   | address | Address funding claimContract                           |
| \_accessControl | address | Address of AccessControl                                |
| \_pToken        | address | Address of the parent token used for snapshot reference |
| \_cToken        | address | Address of child Token being claimed                    |
| \_pAllocation   | uint256 | Total tokens allocated for pToken holders               |

#### moduleFactory

```solidity
function moduleFactory() external view returns (address)
```

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | address | undefined   |

#### name

```solidity
function name() external view returns (string)
```

Returns the module name

**Returns**

| Name | Type   | Description     |
| ---- | ------ | --------------- |
| \_0  | string | The module name |

#### pAllocation

```solidity
function pAllocation() external view returns (uint256)
```

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### pToken

```solidity
function pToken() external view returns (address)
```

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | address | undefined   |

#### proxiableUUID

```solidity
function proxiableUUID() external view returns (bytes32)
```

_Implementation of the ERC1822 {proxiableUUID} function. This returns the storage slot used by the implementation. It is used to validate that the this implementation remains valid after an upgrade. IMPORTANT: A proxy pointing at a proxiable contract should not be considered proxiable itself, because this risks bricking a proxy that upgrades to it, by delegating to itself until out of gas. Thus it is critical that this function revert if invoked through a proxy. This is guaranteed by the `notDelegated` modifier._

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | bytes32 | undefined   |

#### snapId

```solidity
function snapId() external view returns (uint256)
```

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

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

#### upgradeTo

```solidity
function upgradeTo(address newImplementation) external nonpayable
```

_Upgrade the implementation of the proxy to `newImplementation`. Calls {\_authorizeUpgrade}. Emits an {Upgraded} event._

**Parameters**

| Name              | Type    | Description |
| ----------------- | ------- | ----------- |
| newImplementation | address | undefined   |

#### upgradeToAndCall

```solidity
function upgradeToAndCall(address newImplementation, bytes data) external payable
```

_Upgrade the implementation of the proxy to `newImplementation`, and subsequently execute the function call encoded in `data`. Calls {\_authorizeUpgrade}. Emits an {Upgraded} event._

**Parameters**

| Name              | Type    | Description |
| ----------------- | ------- | ----------- |
| newImplementation | address | undefined   |
| data              | bytes   | undefined   |

### Events

#### AdminChanged

```solidity
event AdminChanged(address previousAdmin, address newAdmin)
```

**Parameters**

| Name          | Type    | Description |
| ------------- | ------- | ----------- |
| previousAdmin | address | undefined   |
| newAdmin      | address | undefined   |

#### BeaconUpgraded

```solidity
event BeaconUpgraded(address indexed beacon)
```

**Parameters**

| Name             | Type    | Description |
| ---------------- | ------- | ----------- |
| beacon `indexed` | address | undefined   |

#### SnapAdded

```solidity
event SnapAdded(address pToken, address cToken, uint256 pAllocation)
```

**Parameters**

| Name        | Type    | Description |
| ----------- | ------- | ----------- |
| pToken      | address | undefined   |
| cToken      | address | undefined   |
| pAllocation | uint256 | undefined   |

#### SnapClaimed

```solidity
event SnapClaimed(address indexed pToken, address indexed cToken, address indexed claimer, uint256 amount)
```

**Parameters**

| Name              | Type    | Description |
| ----------------- | ------- | ----------- |
| pToken `indexed`  | address | undefined   |
| cToken `indexed`  | address | undefined   |
| claimer `indexed` | address | undefined   |
| amount            | uint256 | undefined   |

#### Upgraded

```solidity
event Upgraded(address indexed implementation)
```

**Parameters**

| Name                     | Type    | Description |
| ------------------------ | ------- | ----------- |
| implementation `indexed` | address | undefined   |

### Errors

#### AllocationClaimed

```solidity
error AllocationClaimed()
```

#### NoAllocation

```solidity
error NoAllocation()
```

#### NotAuthorized

```solidity
error NotAuthorized()
```
