# ModuleFactoryBase.md

## ModuleFactoryBase

An abstract contract to be inherited by module contracts

### Methods

#### DEFAULT\_ADMIN\_ROLE

```solidity
function DEFAULT_ADMIN_ROLE() external view returns (bytes32)
```

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | bytes32 | undefined   |

#### VERSION\_ROLE

```solidity
function VERSION_ROLE() external view returns (bytes32)
```

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | bytes32 | undefined   |

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
function create(bytes[] data) external nonpayable returns (address[])
```

_Creates a module_

**Parameters**

| Name | Type     | Description                                  |
| ---- | -------- | -------------------------------------------- |
| data | bytes\[] | The array of bytes used to create the module |

**Returns**

| Name | Type       | Description                                      |
| ---- | ---------- | ------------------------------------------------ |
| \_0  | address\[] | address\[] Array of the created module addresses |

#### getRoleAdmin

```solidity
function getRoleAdmin(bytes32 role) external view returns (bytes32)
```

_Returns the admin role that controls `role`. See {grantRole} and {revokeRole}. To change a role's admin, use {\_setRoleAdmin}._

**Parameters**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| role | bytes32 | undefined   |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | bytes32 | undefined   |

#### grantRole

```solidity
function grantRole(bytes32 role, address account) external nonpayable
```

_Grants `role` to `account`. If `account` had not been already granted `role`, emits a {RoleGranted} event. Requirements: - the caller must have `role`'s admin role._

**Parameters**

| Name    | Type    | Description |
| ------- | ------- | ----------- |
| role    | bytes32 | undefined   |
| account | address | undefined   |

#### hasRole

```solidity
function hasRole(bytes32 role, address account) external view returns (bool)
```

_Returns `true` if `account` has been granted `role`._

**Parameters**

| Name    | Type    | Description |
| ------- | ------- | ----------- |
| role    | bytes32 | undefined   |
| account | address | undefined   |

**Returns**

| Name | Type | Description |
| ---- | ---- | ----------- |
| \_0  | bool | undefined   |

#### renounceRole

```solidity
function renounceRole(bytes32 role, address account) external nonpayable
```

_Revokes `role` from the calling account. Roles are often managed via {grantRole} and {revokeRole}: this function's purpose is to provide a mechanism for accounts to lose their privileges if they are compromised (such as when a trusted device is misplaced). If the calling account had been revoked `role`, emits a {RoleRevoked} event. Requirements: - the caller must be `account`._

**Parameters**

| Name    | Type    | Description |
| ------- | ------- | ----------- |
| role    | bytes32 | undefined   |
| account | address | undefined   |

#### revokeRole

```solidity
function revokeRole(bytes32 role, address account) external nonpayable
```

_Revokes `role` from `account`. If `account` had been granted `role`, emits a {RoleRevoked} event. Requirements: - the caller must have `role`'s admin role._

**Parameters**

| Name    | Type    | Description |
| ------- | ------- | ----------- |
| role    | bytes32 | undefined   |
| account | address | undefined   |

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

#### RoleAdminChanged

```solidity
event RoleAdminChanged(bytes32 indexed role, bytes32 indexed previousAdminRole, bytes32 indexed newAdminRole)
```

**Parameters**

| Name                        | Type    | Description |
| --------------------------- | ------- | ----------- |
| role `indexed`              | bytes32 | undefined   |
| previousAdminRole `indexed` | bytes32 | undefined   |
| newAdminRole `indexed`      | bytes32 | undefined   |

#### RoleGranted

```solidity
event RoleGranted(bytes32 indexed role, address indexed account, address indexed sender)
```

**Parameters**

| Name              | Type    | Description |
| ----------------- | ------- | ----------- |
| role `indexed`    | bytes32 | undefined   |
| account `indexed` | address | undefined   |
| sender `indexed`  | address | undefined   |

#### RoleRevoked

```solidity
event RoleRevoked(bytes32 indexed role, address indexed account, address indexed sender)
```

**Parameters**

| Name              | Type    | Description |
| ----------------- | ------- | ----------- |
| role `indexed`    | bytes32 | undefined   |
| account `indexed` | address | undefined   |
| sender `indexed`  | address | undefined   |

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

### Errors

#### NotAuthorized

```solidity
error NotAuthorized()
```
