---
description: >-
  Governor Module used to implement 1 token 1 vote. This acts as an extension of
  the MVD and permissions are controlled by access control.Gov Module is
  extended by the timelock contract which creates a
---

# GovernorModule

### Methods

#### BALLOT\_TYPEHASH

```solidity
function BALLOT_TYPEHASH() external view returns (bytes32)
```

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | bytes32 | undefined   |

#### COUNTING\_MODE

```solidity
function COUNTING_MODE() external pure returns (string)
```

_See {IGovernor-COUNTING\_MODE}._

**Returns**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| \_0  | string | undefined   |

#### accessControl

```solidity
function accessControl() external view returns (contract IDAOAccessControl)
```

**Returns**

| Name | Type                       | Description |
| ---- | -------------------------- | ----------- |
| \_0  | contract IDAOAccessControl | undefined   |

#### castVote

```solidity
function castVote(uint256 proposalId, uint8 support) external nonpayable returns (uint256)
```

_See {IGovernor-castVote}._

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |
| support    | uint8   | undefined   |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### castVoteBySig

```solidity
function castVoteBySig(uint256 proposalId, uint8 support, uint8 v, bytes32 r, bytes32 s) external nonpayable returns (uint256)
```

_See {IGovernor-castVoteBySig}._

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |
| support    | uint8   | undefined   |
| v          | uint8   | undefined   |
| r          | bytes32 | undefined   |
| s          | bytes32 | undefined   |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### castVoteWithReason

```solidity
function castVoteWithReason(uint256 proposalId, uint8 support, string reason) external nonpayable returns (uint256)
```

_See {IGovernor-castVoteWithReason}._

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |
| support    | uint8   | undefined   |
| reason     | string  | undefined   |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### execute

```solidity
function execute(address[] targets, uint256[] values, bytes[] calldatas, bytes32 descriptionHash) external payable returns (uint256)
```

_See {IGovernor-execute}._

**Parameters**

| Name            | Type       | Description |
| --------------- | ---------- | ----------- |
| targets         | address\[] | undefined   |
| values          | uint256\[] | undefined   |
| calldatas       | bytes\[]   | undefined   |
| descriptionHash | bytes32    | undefined   |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### getVotes

```solidity
function getVotes(address account, uint256 blockNumber) external view returns (uint256)
```

module:reputation

_Voting power of an `account` at a specific `blockNumber`. Note: this can be implemented in a number of ways, for example by reading the delegated balance from one (or multiple), {ERC20Votes} tokens._

**Parameters**

| Name        | Type    | Description                    |
| ----------- | ------- | ------------------------------ |
| account     | address | Voting weight of this Address  |
| blockNumber | uint256 | Checkpoint at this blockNumber |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### hasVoted

```solidity
function hasVoted(uint256 proposalId, address account) external view returns (bool)
```

_See {IGovernor-hasVoted}._

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |
| account    | address | undefined   |

**Returns**

| Name | Type | Description |
| ---- | ---- | ----------- |
| \_0  | bool | undefined   |

#### hashProposal

```solidity
function hashProposal(address[] targets, uint256[] values, bytes[] calldatas, bytes32 descriptionHash) external pure returns (uint256)
```

_See {IGovernor-hashProposal}. The proposal id is produced by hashing the RLC encoded `targets` array, the `values` array, the `calldatas` array and the descriptionHash (bytes32 which itself is the keccak256 hash of the description string). This proposal id can be produced from the proposal data which is part of the {ProposalCreated} event. It can even be computed in advance, before the proposal is submitted. Note that the chainId and the governor address are not part of the proposal id computation. Consequently, the same proposal (with same operation and same description) will have the same id if submitted on multiple governors accross multiple networks. This also means that in order to execute the same operation twice (on the same governor) the proposer will have to change the description in order to avoid proposal id conflicts._

**Parameters**

| Name            | Type       | Description |
| --------------- | ---------- | ----------- |
| targets         | address\[] | undefined   |
| values          | uint256\[] | undefined   |
| calldatas       | bytes\[]   | undefined   |
| descriptionHash | bytes32    | undefined   |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### initialize

```solidity
function initialize(contract IVotesUpgradeable _token, contract ITimelock _timelock, uint64 _initialVoteExtension, uint256 _initialVotingDelay, uint256 _initialVotingPeriod, uint256 _initialProposalThreshold, uint256 _initialQuorumNumeratorValue, address _accessControl) external nonpayable
```

_Configures Gov Module implementationCalled once during deployment atomically_

**Parameters**

| Name                          | Type                       | Description                                                |
| ----------------------------- | -------------------------- | ---------------------------------------------------------- |
| \_token                       | contract IVotesUpgradeable | Voting token uses snapshot feature                         |
| \_timelock                    | contract ITimelock         | Timelock vest proposals to allow detractors to exit system |
| \_initialVoteExtension        | uint64                     | Allow users to vote if quorum attack is preformed          |
| \_initialVotingDelay          | uint256                    | Allow users to research proposals before voting period     |
| \_initialVotingPeriod         | uint256                    | Length of voting period (blocks)                           |
| \_initialProposalThreshold    | uint256                    | Total tokens required to submit a proposal                 |
| \_initialQuorumNumeratorValue | uint256                    | Total votes needed to reach quorum                         |
| \_accessControl               | address                    | Address of Access Control                                  |

#### lateQuorumVoteExtension

```solidity
function lateQuorumVoteExtension() external view returns (uint64)
```

_Returns the current value of the vote extension parameter: the number of blocks that are required to pass from the time a proposal reaches quorum until its voting period ends._

**Returns**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| \_0  | uint64 | undefined   |

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

#### proposalDeadline

```solidity
function proposalDeadline(uint256 proposalId) external view returns (uint256)
```

module:core

_Block number at which votes close. Votes close at the end of this block, so it is possible to cast a vote during this block._

**Parameters**

| Name       | Type    | Description                       |
| ---------- | ------- | --------------------------------- |
| proposalId | uint256 | keccak256 hash of proposal params |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### proposalEta

```solidity
function proposalEta(uint256 proposalId) external view returns (uint256)
```

_Public accessor to check the eta of a queued proposal_

**Parameters**

| Name       | Type    | Description                       |
| ---------- | ------- | --------------------------------- |
| proposalId | uint256 | keccak256 hash of proposal params |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### proposalSnapshot

```solidity
function proposalSnapshot(uint256 proposalId) external view returns (uint256)
```

_See {IGovernor-proposalSnapshot}._

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### proposalThreshold

```solidity
function proposalThreshold() external view returns (uint256)
```

_Total vote weight required to create a proposal_

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### proposalVotes

```solidity
function proposalVotes(uint256 proposalId) external view returns (uint256 againstVotes, uint256 forVotes, uint256 abstainVotes)
```

_Accessor to the internal vote counts._

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |

**Returns**

| Name         | Type    | Description |
| ------------ | ------- | ----------- |
| againstVotes | uint256 | undefined   |
| forVotes     | uint256 | undefined   |
| abstainVotes | uint256 | undefined   |

#### propose

```solidity
function propose(address[] targets, uint256[] values, bytes[] calldatas, string description) external nonpayable returns (uint256)
```

_Create a new proposal. Vote start {IGovernor-votingDelay} blocks after the proposal is created and ends {IGovernor-votingPeriod} blocks after the voting starts. Emits a {ProposalCreated} event._

**Parameters**

| Name        | Type       | Description                                   |
| ----------- | ---------- | --------------------------------------------- |
| targets     | address\[] | Contract addresses the DAO will call          |
| values      | uint256\[] | Ether values to be sent to the target address |
| calldatas   | bytes\[]   | Function Sigs w/ Params                       |
| description | string     | Description of proposal                       |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### proxiableUUID

```solidity
function proxiableUUID() external view returns (bytes32)
```

_Implementation of the ERC1822 {proxiableUUID} function. This returns the storage slot used by the implementation. It is used to validate that the this implementation remains valid after an upgrade. IMPORTANT: A proxy pointing at a proxiable contract should not be considered proxiable itself, because this risks bricking a proxy that upgrades to it, by delegating to itself until out of gas. Thus it is critical that this function revert if invoked through a proxy. This is guaranteed by the `notDelegated` modifier._

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | bytes32 | undefined   |

#### queue

```solidity
function queue(address[] targets, uint256[] values, bytes[] calldatas, bytes32 descriptionHash) external nonpayable returns (uint256)
```

_Function to queue a proposal to the timelock._

**Parameters**

| Name            | Type       | Description                                   |
| --------------- | ---------- | --------------------------------------------- |
| targets         | address\[] | Contract addresses the DAO will call          |
| values          | uint256\[] | Ether values to be sent to the target address |
| calldatas       | bytes\[]   | Function Sigs w/ Params                       |
| descriptionHash | bytes32    | Description of proposal                       |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### quorum

```solidity
function quorum(uint256 blockNumber) external view returns (uint256)
```

module:user-config

_Minimum number of cast voted required for a proposal to be successful. Note: The `blockNumber` parameter corresponds to the snaphot used for counting vote. This allows to scale the quroum depending on values such as the totalSupply of a token at this block (see {ERC20Votes})._

**Parameters**

| Name        | Type    | Description                    |
| ----------- | ------- | ------------------------------ |
| blockNumber | uint256 | Checkpoint at this blockNumber |

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### quorumDenominator

```solidity
function quorumDenominator() external view returns (uint256)
```

_Returns the quorum denominator. Defaults to 100, but may be overridden._

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### quorumNumerator

```solidity
function quorumNumerator() external view returns (uint256)
```

_Returns the current quorum numerator. See {quorumDenominator}._

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### relay

```solidity
function relay(address target, uint256 value, bytes data) external nonpayable
```

_Relays a transaction or function call to an arbitrary target. In cases where the governance executor is some contract other than the governor itself, like when using a timelock, this function can be invoked in a governance proposal to recover tokens or Ether that was sent to the governor contract by mistake. Note that if the executor is simply the governor itself, use of `relay` is redundant._

**Parameters**

| Name   | Type    | Description |
| ------ | ------- | ----------- |
| target | address | undefined   |
| value  | uint256 | undefined   |
| data   | bytes   | undefined   |

#### setLateQuorumVoteExtension

```solidity
function setLateQuorumVoteExtension(uint64 newVoteExtension) external nonpayable
```

_Changes the {lateQuorumVoteExtension}. This operation can only be performed by the governance executor, generally through a governance proposal. Emits a {LateQuorumVoteExtensionSet} event._

**Parameters**

| Name             | Type   | Description |
| ---------------- | ------ | ----------- |
| newVoteExtension | uint64 | undefined   |

#### setProposalThreshold

```solidity
function setProposalThreshold(uint256 newProposalThreshold) external nonpayable
```

_Update the proposal threshold. This operation can only be performed through a governance proposal. Emits a {ProposalThresholdSet} event._

**Parameters**

| Name                 | Type    | Description |
| -------------------- | ------- | ----------- |
| newProposalThreshold | uint256 | undefined   |

#### setVotingDelay

```solidity
function setVotingDelay(uint256 newVotingDelay) external nonpayable
```

_Update the voting delay. This operation can only be performed through a governance proposal. Emits a {VotingDelaySet} event._

**Parameters**

| Name           | Type    | Description |
| -------------- | ------- | ----------- |
| newVotingDelay | uint256 | undefined   |

#### setVotingPeriod

```solidity
function setVotingPeriod(uint256 newVotingPeriod) external nonpayable
```

_Update the voting period. This operation can only be performed through a governance proposal. Emits a {VotingPeriodSet} event._

**Parameters**

| Name            | Type    | Description |
| --------------- | ------- | ----------- |
| newVotingPeriod | uint256 | undefined   |

#### state

```solidity
function state(uint256 proposalId) external view returns (enum IGovernorUpgradeable.ProposalState)
```

_Overriden version of the {Governor-state} function with added support for the `Queued` status._

**Parameters**

| Name       | Type    | Description                       |
| ---------- | ------- | --------------------------------- |
| proposalId | uint256 | keccak256 hash of proposal params |

**Returns**

| Name | Type                                    | Description |
| ---- | --------------------------------------- | ----------- |
| \_0  | enum IGovernorUpgradeable.ProposalState | undefined   |

#### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceId) external view returns (bool)
```

_See {IERC165-supportsInterface}._

**Parameters**

| Name        | Type   | Description                                  |
| ----------- | ------ | -------------------------------------------- |
| interfaceId | bytes4 | An interface ID bytes4 as defined by ERC-165 |

**Returns**

| Name | Type | Description                                       |
| ---- | ---- | ------------------------------------------------- |
| \_0  | bool | bool Indicates whether the interface is supported |

#### timelock

```solidity
function timelock() external view returns (address)
```

_Public accessor to check the address of the timelock_

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | address | undefined   |

#### token

```solidity
function token() external view returns (contract IVotesUpgradeable)
```

**Returns**

| Name | Type                       | Description |
| ---- | -------------------------- | ----------- |
| \_0  | contract IVotesUpgradeable | undefined   |

#### updateQuorumNumerator

```solidity
function updateQuorumNumerator(uint256 newQuorumNumerator) external nonpayable
```

_Changes the quorum numerator. Emits a {QuorumNumeratorUpdated} event. Requirements: - Must be called through a governance proposal. - New numerator must be smaller or equal to the denominator._

**Parameters**

| Name               | Type    | Description |
| ------------------ | ------- | ----------- |
| newQuorumNumerator | uint256 | undefined   |

#### updateTimelock

```solidity
function updateTimelock(contract ITimelock newTimelock) external nonpayable
```

_Public endpoint to update the underlying timelock instance. Restricted to the timelock itself, so updates must be proposed, scheduled, and executed through governance proposals. CAUTION: It is not recommended to change the timelock while there are other queued governance proposals._

**Parameters**

| Name        | Type               | Description                     |
| ----------- | ------------------ | ------------------------------- |
| newTimelock | contract ITimelock | Address of new Timelock Address |

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

#### version

```solidity
function version() external view returns (string)
```

_See {IGovernor-version}._

**Returns**

| Name | Type   | Description |
| ---- | ------ | ----------- |
| \_0  | string | undefined   |

#### votingDelay

```solidity
function votingDelay() external view returns (uint256)
```

module:user-config

_Delay, in number of block, between the proposal is created and the vote starts. This can be increassed to leave time for users to buy voting power, of delegate it, before the voting of a proposal starts._

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

#### votingPeriod

```solidity
function votingPeriod() external view returns (uint256)
```

module:user-config

_Delay, in number of blocks, between the vote start and vote ends. NOTE: The {votingDelay} can delay the start of the vote. This must be considered when setting the voting duration compared to the voting delay._

**Returns**

| Name | Type    | Description |
| ---- | ------- | ----------- |
| \_0  | uint256 | undefined   |

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

#### LateQuorumVoteExtensionSet

```solidity
event LateQuorumVoteExtensionSet(uint64 oldVoteExtension, uint64 newVoteExtension)
```

**Parameters**

| Name             | Type   | Description |
| ---------------- | ------ | ----------- |
| oldVoteExtension | uint64 | undefined   |
| newVoteExtension | uint64 | undefined   |

#### ProposalCanceled

```solidity
event ProposalCanceled(uint256 proposalId)
```

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |

#### ProposalCreated

```solidity
event ProposalCreated(uint256 proposalId, address proposer, address[] targets, uint256[] values, string[] signatures, bytes[] calldatas, uint256 startBlock, uint256 endBlock, string description)
```

**Parameters**

| Name        | Type       | Description |
| ----------- | ---------- | ----------- |
| proposalId  | uint256    | undefined   |
| proposer    | address    | undefined   |
| targets     | address\[] | undefined   |
| values      | uint256\[] | undefined   |
| signatures  | string\[]  | undefined   |
| calldatas   | bytes\[]   | undefined   |
| startBlock  | uint256    | undefined   |
| endBlock    | uint256    | undefined   |
| description | string     | undefined   |

#### ProposalExecuted

```solidity
event ProposalExecuted(uint256 proposalId)
```

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |

#### ProposalExtended

```solidity
event ProposalExtended(uint256 indexed proposalId, uint64 extendedDeadline)
```

**Parameters**

| Name                 | Type    | Description |
| -------------------- | ------- | ----------- |
| proposalId `indexed` | uint256 | undefined   |
| extendedDeadline     | uint64  | undefined   |

#### ProposalQueued

```solidity
event ProposalQueued(uint256 proposalId, uint256 eta)
```

**Parameters**

| Name       | Type    | Description |
| ---------- | ------- | ----------- |
| proposalId | uint256 | undefined   |
| eta        | uint256 | undefined   |

#### ProposalThresholdSet

```solidity
event ProposalThresholdSet(uint256 oldProposalThreshold, uint256 newProposalThreshold)
```

**Parameters**

| Name                 | Type    | Description |
| -------------------- | ------- | ----------- |
| oldProposalThreshold | uint256 | undefined   |
| newProposalThreshold | uint256 | undefined   |

#### QuorumNumeratorUpdated

```solidity
event QuorumNumeratorUpdated(uint256 oldQuorumNumerator, uint256 newQuorumNumerator)
```

**Parameters**

| Name               | Type    | Description |
| ------------------ | ------- | ----------- |
| oldQuorumNumerator | uint256 | undefined   |
| newQuorumNumerator | uint256 | undefined   |

#### TimelockChange

```solidity
event TimelockChange(address oldTimelock, address newTimelock)
```

**Parameters**

| Name        | Type    | Description |
| ----------- | ------- | ----------- |
| oldTimelock | address | undefined   |
| newTimelock | address | undefined   |

#### Upgraded

```solidity
event Upgraded(address indexed implementation)
```

**Parameters**

| Name                     | Type    | Description |
| ------------------------ | ------- | ----------- |
| implementation `indexed` | address | undefined   |

#### VoteCast

```solidity
event VoteCast(address indexed voter, uint256 proposalId, uint8 support, uint256 weight, string reason)
```

**Parameters**

| Name            | Type    | Description |
| --------------- | ------- | ----------- |
| voter `indexed` | address | undefined   |
| proposalId      | uint256 | undefined   |
| support         | uint8   | undefined   |
| weight          | uint256 | undefined   |
| reason          | string  | undefined   |

#### VotingDelaySet

```solidity
event VotingDelaySet(uint256 oldVotingDelay, uint256 newVotingDelay)
```

**Parameters**

| Name           | Type    | Description |
| -------------- | ------- | ----------- |
| oldVotingDelay | uint256 | undefined   |
| newVotingDelay | uint256 | undefined   |

#### VotingPeriodSet

```solidity
event VotingPeriodSet(uint256 oldVotingPeriod, uint256 newVotingPeriod)
```

**Parameters**

| Name            | Type    | Description |
| --------------- | ------- | ----------- |
| oldVotingPeriod | uint256 | undefined   |
| newVotingPeriod | uint256 | undefined   |

### Errors

#### NotAuthorized

```solidity
error NotAuthorized()
```
