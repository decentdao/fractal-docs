# Fractal Core

The Fractal core creates a DAO contract that can execute code on the blockchain, control the addresses that can run the execute function, and provides an interface for extending the DAO with modules. The Fractal core includes the following components:

* `fractal-core-contracts` - A set of contracts for deploying new Fractal DAOs that include the basic Fractal DAO contracts defined in the fractal-contracts-package.

## Core Contracts

The `fractal-core-contracts` repository acts as a contract factory for deploying new Fractal DAOs. New fractal DAOs include the functions and contracts required to execute code on the blockchain and control access to the functions of the DAO.

The `fractal-core-contracts` repository includes the following contracts in the `/contracts` directory:

* `DAO.sol` - This contract allows a permissioned party to execute a transaction on behalf of the DAO.
* `AccessControlDAO.sol` - This contract defines the permissions for the DAO and its modules. Methods that belong to the DAO's system, which require authorization, check the calling party's permissions and either grants or denies the party access based on this check.
* `DAOFactory.sol` - A factory contract that deploys instances of a DAO and an AccessControlDAO.
* `ModuleBase.sol` - An abstract contract that defines the required methods/interfaces to deploy a Fractal compatible module.
* `ModuleFactoryBase.sol` - An abstract contract that defines the required methods/interfaces to be compatible with the Metafactory and to contain the standard versioning system.
* `/interfaces` - Interface contracts that define the signature functions of the Fractal-core-contracts described above.
