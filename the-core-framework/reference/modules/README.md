# Modules

Modules extend the functionality of a basic DAO. Fractal modules must inherit from the Fractal core ModuleBase.sol contract. All modules must include a factory contract that inherits from the ModuleFactoryBase.sol contract in order to utilize the permission system of the AccessControlDAO.

Fractal currently provides the following modules:

* `fractal-module-token` - The Token Module mints and distributes vote tokens based on the ERC20 Votes OpenZ contracts and creates a claim contract which allows parent DAOs to mirror their token holdings to a sub DAO.
* `fractal-module-treasury` - The Treasury Module allows the depositing and withdrawing of arbitrary value on chain. Currently supported is ETH, ERC20, ERC721, ERC1155.
* `fractal-module-governor` - The Governor Module supports creating proposals, voting on proposals, and executing transactions on an accompanying DAO as a result of the vote based on the Governor OpenZ contracts.

### Token Module

The Token Module adds functionality to a basic DAO for minting and distributing vote tokens. The Token Module includes the following contracts:

* `VotesToken.sol` - Defines the constructor for a vote token which requires a name, symbol, holders, and allocations. This contract also inherits from OpenZeppelin' Snapshot.sol which provides a snapshot of a user's balance at a particular block-height.&#x20;
* `TokenFactory.sol` - Deploys a Token Module.
* ClaimSubsidiary.sol - If a DAO is classified as a SubDAO - they have the ability to mirror/fork the parent DAO's token allocations across the subDAO's token holders. This contract takes a snapshot of the parent token's user balances and sets aside some subDAO tokens which can be claimed by holders of the parent DAO's token.
* ClaimFactory.sol - Deploys a Claim Module.

### Treasury Module

The Treasury Module manages depositing and withdrawing ERC20, ERC721, and ERC1155 tokens between the DAO's wallets and other wallets. This makes it possible for DAOs to execute the results of voting on proposals.

The Treasury Module includes the following contracts:

* `TreasuryModule.sol` - Defines functions to initialize the contract with a set of authorization parameters and withdraw and deposit supported tokens to and from addresses.
* `TreasuryModuleFactory.sol` - Deploys a Treasury Module.

### Governor Module

The Governor Module adds proposal and voting features to a Fractal DAO. The Governor Module includes the following contracts:

* `GovernorModule.sol` - Defines functions for creating proposals, voting on proposals, and executing proposal transaction on the the accompanying DAO.
* &#x20;`GovTimelockUpgradeable.sol` - Extension of {Governor} that binds the execution process to an instance of {TimelockController}. This adds a delay, enforced by the {TimelockController} to all successful proposal (in addition to the voting duration).
* `TimelockUpgradeable.sol` - Module which acts as a timelocked controller. When set as the executor for the DAO execute action, it enforces a timelock/timeDelay on all DAO executions. This gives time for users of the controlled contract to exit before a potentially dangerous operation is applied.
* `GovernorModuleFactory.sol` - Deploys a Governor Module.
