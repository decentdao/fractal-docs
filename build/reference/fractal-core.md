# Fractal Core

The Fractal core creates a DAO contract that can execute code on the blockchain, control the addresses that can run the execute function and provides an interface for extending the DAO with modules. The Fractal core includes the following components:

* `fractal-contracts` - A set of contracts that for deploying new Fractal DAOs that include the basic Fractal DAO contracts defined in the fractal-contracts-package.

## Contracts

The `fractal-contracts` repository acts as a contract factory for deploying new Fractal DAOs. New fractal DAOs include the functions and contracts required to execute code on the blockchain and control access to the functions of the DAO.

The Fractal DAO structure is based on of the OpenZeppelin framework and contracts import additional modules as needed.

The `fractal-contracts` repository includes the following contracts in the `/contracts` directory:

* `DAO.sol` - Initializes a new DAO that can execute functions on other contracts and can check whether imported interfaces are supported.
* `AccessControlDAO.sol` - Creates and modifies roles and grants or revokes roles from target addresses.
* `DAOFactory.sol` - A factory contract for deploying DAOs that include the access control contract.
* `MetaFactory.sol` - A factory contract for deploying DAOs along with the access control contract any other supported modules.
* `ModuleBase.sol` - Defines the functions that support extending the capabilities of a Fractal DAO using modules: initialize a module contract, check authorizations, call functions on different contracts, include interfaces, and authorize upgrades.
* `/interfaces` - Interface contracts that define the signature functions of the Fractal core contracts described above.
