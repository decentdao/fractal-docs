---
description: Set up a project to build a module
---

# Setup

To get started, create a Hardhat project in your project directory and configure your development environment.

{% hint style="info" %}
If you just want to follow along with the module developer guides, clone the Treasury Module Example repo. It provides a complete example of a working Fractal module, and includes all of the code shown in these tutorials.

Run the command below in your project directory to clone the example project, then continue with the rest of the steps on this page:

```
git clone https://github.com/decent-dao/fractal-module-example
```
{% endhint %}

Follow the steps below to start a new module project from scratch:

#### 1. Create Hardhat Project

If you do not already have hardhat installed, run the command below in your project directory:

```
// If you do not have hardhat installed
npm install --save-dev hardhat
```

Use `npx` to create a Hardhat project and respond to the prompts:

```
// Create A hardhat project
npx hardhat
// Choose Create w/ Typescript
```

#### 2. Configure Your Dev Environment

Set up and configure your dev environment based on the needs of your project. At a minimum, you will need to edit the following files:

* [ ] .env
* [ ] .nvmrc
* [ ] .gitignore
* [ ] tsconfig.json
* [ ] package.json
* [ ] hardhat.config.ts

See the [Fractal Treasury Module](https://github.com/decent-dao/fractal-module-treasury) repo for an example of a fully configured environment.

Once your project is set up, move on to the [Creating the Module](creating-the-module.md) guide and start coding.

