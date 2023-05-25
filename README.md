---
description: >-
  Do more with Composable DAOs
---

# Docs

Fractal is the most composable Decentralized Autonomous Organization (DAO) app and framework that unleashes a new wave of agile, on-chain, and interoperable DAOs that are empowered to get stuff done.

Start entirely from scratch or import an existing (Gnosis) Safe or ERC-20 token to use for governance. Then create unique subDAO hierarchies to the needs of your specific community or organization. Each subDAO is empowered with their own treasury, permissions, and governance style. Suddenly they can operate like a completely autonomous DAO and get stuff done whilst never losing on-chain accountability to their community or parent stakeholders. 

Fractal offers value to everyone in your DAO from:

* **Leaders and Founders** - Create your own on-chain subDAO hierarchies. Establish the structure, permissions and governance unique to your DAO. Offer your contributors as much - or as little - decision making power on assets as you feel comfortable with. Ensure that whatever structure you decide on, it is transparent, scalable, and on-chain to build community confidence.
* **Community Members**  - Track all DAO activities and asset movements. Engage on proposals based on permissions. Let contributor subDAOs action daily tasks whilst you review - and potentially freeze - activities that may go against your wishes.
* **Daily Contributors** - Get clarity on what budgets and assets you've been allocated. Action your responsibilities with on-chain buy-in from the community. Speed up regular DAO tasks with a template builder that interoperates no-code with any smart contract on Ethereum to access all the benefits DeFi offers your organization.

---

## Why Fractal?

There are an infinite number of structures and possibilities available to DAOs today. However, even web3 organisations right now are hesitant to move on-chain. Current options make potential users feel they may be stuck giving up their agility, scalability, and community experimentation when they make the switch. Fractal fills the need for more composable solutions for DAOs by being highly modular and adaptable to new trends.

Even better, the switching costs are zero by being entirely built on Safe multisignature contract wallets at their core. Get the most out of composable DAOs but always feel free to leave with a fully functioning Safe at any point.

### Benefits

The Fractal framework has several benefits over traditional DAO implementations:

* **Structure** - Compose subDAO hierarchies each with their own treasury, governance, and permissions. This could include a CEO with executive powers over a community as they progressively decentralize. Or, imagine a community who empowers subDAOs of contributors to deliver daily tasks for them. Every DAO structure is in reach and it's all no-code to set up and begin using.
* **Govern** - Create and vote on proposals with multiple governance options per subDAO. Every subDAO can choose what specifically suits them from token voting, NFT-based voting, or a simple multi-sig Safe and many more options as we continue to grow.
* **Operate** - Start quickly with any existing Safe. Then use our DAO modules & template builder to infinitely expand its functionality. Any smart contract on Ethereum can be interacted with in our app and added to your DAO's UI to fast track regular DAO operations without writing a line of code yourself.

---

## Nomenclature

##### Safe
The uppercased word Safe (formerly Gnosis Safe) refers to a multisignature smart contract wallet by [Gnosis](https://safe.global/). Safe is the most trusted name in on-chain asset ownership in Ethereum, and the foundation on which every Fractal DAO is based.

##### Fractal
A Fractal is a specific type of DAO where contributors can be structured into unique *parentDAO* and *subDAO* hierarchies each with their own governance and Safe treasury. At its heart each Fractal is a Safe contract wallet with a governance [module](https://docs.safe.global/learn/safe-core/safe-core-protocol/modules) and an optional [guard](https://docs.safe.global/learn/safe-core/safe-core-protocol/guards) contract to allow for *freezing* a subDAO. 

##### rootDAO 
The initial DAO that has started the on-chain hierarchies that form the Fractal. This is the only Safe treasury that has no parent and so no executive powers or on-chain relationships limiting its control.

##### parentDAO 
A Safe treasury that has spawned a subDAO (see below). On creation of a subDAO, the parentDAO can set the subDAO's governance method, send it funds and set certain on-chain safe-guards and controls to give the parent some measure of control (e.g. freeze powers, delaying timelock period per proposal etc.).

##### subDAO
A Fractal that has been spawned by a parentDAO Fractal. A subDAO always has certain on-chain relationships and controls that the parentDAO has customized on creation. Aside from this, the subDAO has autonomy to use its treasury to make on-chain asset decisions with its own distinct governance method. Any subDAO also has the autonomy to additionally become a parentDAO itself by creating subDAOs.
