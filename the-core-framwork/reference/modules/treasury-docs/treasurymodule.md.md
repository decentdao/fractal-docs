# TreasuryModule.md

## TreasuryModule

A treasury module contract for managing a DAOs assets

### Methods

#### accessControl

```solidity
function accessControl() external view returns (contract IAccessControlDAO)
```

**Returns**

| Name | Type                       | Description |
| ---- | -------------------------- | ----------- |
| \_0  | contract IAccessControlDAO | undefined   |

#### depositERC20Tokens

```solidity
function depositERC20Tokens(address[] tokenAddresses, address[] senders, uint256[] amounts) external nonpayable
```

Allows the owner to deposit ERC-20 tokens from multiple addresses

**Parameters**

| Name           | Type       | Description                                                       |
| -------------- | ---------- | ----------------------------------------------------------------- |
| tokenAddresses | address\[] | Array of token contract addresses                                 |
| senders        | address\[] | Array of addresses that the ERC-20 token will be transferred from |
| amounts        | uint256\[] | Array of amounts of the ERC-20 token that will be transferred     |

#### depositERC721Tokens

```solidity
function depositERC721Tokens(address[] tokenAddresses, address[] senders, uint256[] tokenIds) external nonpayable
```

Allows the owner to deposit ERC-721 tokens from multiple addresses

**Parameters**

| Name           | Type       | Description                                                         |
| -------------- | ---------- | ------------------------------------------------------------------- |
| tokenAddresses | address\[] | Array of token contract addresses                                   |
| senders        | address\[] | Array of addresses that the ERC-721 tokens will be transferred from |
| tokenIds       | uint256\[] | Array of amounts of the ERC-20 token that will be transferred       |

#### initialize

```solidity
function initialize(address _accessControl) external nonpayable
```

Function for initializing the contract that can only be called once

**Parameters**

| Name            | Type    | Description                                |
| --------------- | ------- | ------------------------------------------ |
| \_accessControl | address | The address of the access control contract |

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

#### onERC721Received

```solidity
function onERC721Received(address, address, uint256, bytes) external nonpayable returns (bytes4)
```

_See {IERC721Receiver-onERC721Received}. Always returns `IERC721Receiver.onERC721Received.selector`._

**Parameters**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | address | undefined   |
| \_1  | address | undefined   |
| \_2  | uint256 | undefined   |
| \_3  | bytes   | undefined   |

**Returns**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| \_0  | bytes4 | undefined   |

#### proxiableUUID

```solidity
function proxiableUUID() external view returns (bytes32)
```

_Implementation of the ERC1822 {proxiableUUID} function. This returns the storage slot used by the implementation. It is used to validate that the this implementation remains valid after an upgrade. IMPORTANT: A proxy pointing at a proxiable contract should not be considered proxiable itself, because this risks bricking a proxy that upgrades to it, by delegating to itself until out of gas. Thus it is critical that this function revert if invoked through a proxy. This is guaranteed by the `notDelegated` modifier._

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | bytes32 | undefined   |

#### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) external view returns (bool)
```

Returns whether a given interface ID is supported

**Parameters**

| Name        | Type   | Description                                  |
| ----------- | ------ | -------------------------------------------- |
| interfaceId | bytes4 | An interface ID bytes4 as defined by ERC-165 |

**Returns**

| Name | Type | Description                                       |
| ---- | ---- | ------------------------------------------------- |
| \_0  | bool | bool Indicates whether the interface is supported |

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

#### withdrawERC20Tokens

```solidity
function withdrawERC20Tokens(address[] tokenAddresses, address[] recipients, uint256[] amounts) external nonpayable
```

Allows the owner to withdraw ERC-20 tokens from multiple addresses

**Parameters**

| Name           | Type       | Description                                                     |
| -------------- | ---------- | --------------------------------------------------------------- |
| tokenAddresses | address\[] | Array of token contract addresses                               |
| recipients     | address\[] | Array of addresses that the ERC-20 token will be transferred to |
| amounts        | uint256\[] | Array of amounts of the ERC-20 token that will be transferred   |

#### withdrawERC721Tokens

```solidity
function withdrawERC721Tokens(address[] tokenAddresses, address[] recipients, uint256[] tokenIds) external nonpayable
```

Allows the owner to withdraw ERC-721 tokens from multiple addresses

**Parameters**

| Name           | Type       | Description                                                       |
| -------------- | ---------- | ----------------------------------------------------------------- |
| tokenAddresses | address\[] | Array of token contract addresses                                 |
| recipients     | address\[] | Array of addresses that the ERC-721 tokens will be transferred to |
| tokenIds       | uint256\[] | Array of amounts of the ERC-20 token that will be transferred     |

#### withdrawEth

```solidity
function withdrawEth(address[] recipients, uint256[] amounts) external nonpayable
```

Allows the owner to withdraw ETH to multiple addresses

**Parameters**

| Name       | Type       | Description                                      |
| ---------- | ---------- | ------------------------------------------------ |
| recipients | address\[] | Array of addresses that ETH will be withdrawn to |
| amounts    | uint256\[] | Array of amounts of ETH that will be withdrawnnn |

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

#### ERC20TokensDeposited

```solidity
event ERC20TokensDeposited(address[] tokenAddresses, address[] senders, uint256[] amounts)
```

**Parameters**

| Name           | Type       | Description |
| -------------- | ---------- | ----------- |
| tokenAddresses | address\[] | undefined   |
| senders        | address\[] | undefined   |
| amounts        | uint256\[] | undefined   |

#### ERC20TokensWithdrawn

```solidity
event ERC20TokensWithdrawn(address[] tokenAddresses, address[] recipients, uint256[] amounts)
```

**Parameters**

| Name           | Type       | Description |
| -------------- | ---------- | ----------- |
| tokenAddresses | address\[] | undefined   |
| recipients     | address\[] | undefined   |
| amounts        | uint256\[] | undefined   |

#### ERC721TokensDeposited

```solidity
event ERC721TokensDeposited(address[] tokenAddresses, address[] senders, uint256[] tokenIds)
```

**Parameters**

| Name           | Type       | Description |
| -------------- | ---------- | ----------- |
| tokenAddresses | address\[] | undefined   |
| senders        | address\[] | undefined   |
| tokenIds       | uint256\[] | undefined   |

#### ERC721TokensWithdrawn

```solidity
event ERC721TokensWithdrawn(address[] tokenAddresses, address[] recipients, uint256[] tokenIds)
```

**Parameters**

| Name           | Type       | Description |
| -------------- | ---------- | ----------- |
| tokenAddresses | address\[] | undefined   |
| recipients     | address\[] | undefined   |
| tokenIds       | uint256\[] | undefined   |

#### EthDeposited

```solidity
event EthDeposited(address sender, uint256 amount)
```

**Parameters**

| Name   | Type    | Description |
| ------ | ------- | ----------- |
| sender | address | undefined   |
| amount | uint256 | undefined   |

#### EthWithdrawn

```solidity
event EthWithdrawn(address[] recipients, uint256[] amounts)
```

**Parameters**

| Name       | Type       | Description |
| ---------- | ---------- | ----------- |
| recipients | address\[] | undefined   |
| amounts    | uint256\[] | undefined   |

#### Upgraded

```solidity
event Upgraded(address indexed implementation)
```

**Parameters**

| Name                     | Type    | Description |
| ------------------------ | ------- | ----------- |
| implementation `indexed` | address | undefined   |

### Errors

#### NotAuthorized

```solidity
error NotAuthorized()
```

#### UnequalArrayLengths

```solidity
error UnequalArrayLengths()
```
