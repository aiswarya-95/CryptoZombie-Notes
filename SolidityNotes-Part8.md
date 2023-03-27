## Chapter1
### Truffle

`Truffle` is the most popular blockchain development **framework** for good reason - it's packed with lots of useful features:

*   easy smart contract compilation
*   automated ABI generation
*   integrated smart contract testing - there's even support for Mocha and Chai!
*   support for multiple networks - code can be deployed to **Rinkeby, Ethereum or even to Loom**. 

let's install Truffle and make it available globally.

```js
    $ npm install truffle -g
```

### Getting Started with Truffle

To initialize our new project by running `truffle init`. All it is doing is to create a set of folders and config files with the following structure:
```js
├── contracts
    ├── Migrations.sol
├── migrations
    ├── 1_initial_migration.js
└── test
truffle-config.js
truffle.js
```

##### Truffle's Default Directory Structure

So, running the `truffle init` command inside of the **CryptoZombies** directory, should create several directories and some JavaScript and Solidity files. Let's have a closer look:

*   **contracts**: this is the place where Truffle expects to find all our smart contracts. To keep the code organized, we can even create nested folders such as **contracts/tokens**.
*   **migrations**: a migration is a JavaScript file that tells Truffle how to deploy a smart contract.
*   **test**: here we are expected to put the unit tests which will be JavaScript or Solidity files. Remember, once a contract is deployed it can't be changed, making it essential that we test our smart contracts before we deploy them.
*   **truffle.js and truffle-config.js**: config files used to store the network settings for deployment. Truffle needs two config files because on Windows having both **truffle.js and truffle.exe** in the same folder might generate conflicts. If you are running Windows, it is advised to delete truffle.js and use truffle-config.js as the default config file. 

First, **Truffle** will not work as expected if you change the names of these folders.

Second, by adhering to this convention your projects will be easily understood by other developers. To put it short, using a standard folder structures and code conventions make it easier if you expand or change your team in the future.

##### truffle-hdwallet-provider

In this lesson, we will be using `Infura` to deploy our code to **Ethereum**. This way, we can run the application without needing to set up our own **Ethereum node or wallet**. However, to keep things secure, `Infura` does not manage the private keys, which means it can't sign transactions on our behalf. Since deploying a smart contract requires Truffle to sign transactions, we are going to need a tool called `truffle-hdwallet-provider`. Its only purpose is to handle the **transaction signing**.
```js
        npm install truffle truffle-hdwallet-provider
```
Well... the `truffle init` command expects to find an **empty directory**. If there's any file there, it will error out. Thus, we need to do everything in the correct order and install `truffle-hdwallet-provider` after we run `truffle init`.
## Chapter 3
### Compiling the Source Code
The **Ethereum Virtual Machine** can't directly understand Solidity source code as we write it. Thus, we need to run a compiler that will "translate" our smart contract into **machine-readable** `bytecode`. The **virtual machine** then executes the **bytecode**, and completes the actions required by our smart contract.
bytecode lokks like,
```js
"0x60806040526010600155600154600a0a6002556201518060035566038d7ea4c6800060085560006009556046600a55336000806101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1..."
```
##### Using the Solidity Compiler

Let's pretend we'd want to modify the definition of the add function included in SafeMath:
```js
function add(uint16 a, uint16 b) internal returns (uint16) {
    uint16 c = a + b;
    assert(c >= a);
    return c;
}
```
If we're going to compile this function, the Solidity compiler will throw a **warning**:
```js
safemath.sol:110:11: Warning: Function state mutability can be restricted to pure
          function add(uint16 a, uint16 b) internal returns (uint16) {
          ^ (Relevant source part starts here and spans across multiple lines).
```
What the compiler is trying to say is that our **function** does not read or write from/to the blockchain and that we should use the `pure` modifier.
Well, making a function `pure` or `view` saves us **gas**. Since these functions are not going to modify the state of the blockchain, there is no need for miners to execute them. To put it in a few words, `pure` and `view` functions can be **called for free**.
#### CryptoZombies- The Game

Remember, we've embedded our logic in a smart contract called `ZombieOwnership.sol`.
We can use inheritance to create a smart contract with the same actions and features with whatever name we choose.

Let's create a new smart contract named `CryptoZombies` that inherits from `ZombieOwnership.sol`:
```js
pragma solidity ^0.4.24;

import "./zombieownership.sol";

contract CryptoZombies is ZombieOwnership
    {

    }
```
Next, we copied all our smart contracts into the `./contracts` folder. Now the project structure should look like this:
```js
.
├── contracts
    ├── Migrations.sol
    ├── CryptoZombies.sol
    ├── erc721.sol
    ├── ownable.sol
    ├── safemath.sol
    ├── zombieattack.sol
    ├── zombiefactory.sol
    ├── zombiefeeding.sol
    ├── zombiehelper.sol
    ├── zombieownership.sol
├── migrations
└── test
```
Everything is set up properly. Let's compile our project.
```js
        $ truffle compile
```

## Chapter 4 
### Migrations

Normally at this point, before deploying to Ethereum, you would want to test your smart contract locally. You can do this using a tool called `Ganache`, which sets up a local Ethereum network.
To deploy to Ethereum we will have to create something called a `migration`.

`Migrations` are **JavaScript** files that help Truffle deploy the code to Ethereum. Note that **truffle init** created a special contract called `Migrations.sol` that keeps track of the changes you're making to your code. The way it works is that the **history of changes** is saved onchain. Thus, there's no way you will ever deploy the same code twice.

##### Creating a New Migration

We'll start from the file truffle init already created for us- `./contracts/1_initial_migration.js`. Let's take a look at what's inside:
```js
var Migrations = artifacts.require("./Migrations.sol");
module.exports = function(deployer) {
  deployer.deploy(Migrations);
};
```
**First**, the script tells Truffle that we'd want to interact with the Migrations contract.

**Next**, it exports a function that accepts an object called `deployer` as a parameter. This object acts as an interface between you (the developer) and Truffle's deployment engine.

To get everything ready for deployment, we've gone ahead and created a new file `./contracts/2_crypto_zombies.js`, and copied and pasted the content from `./contracts/1_initial_migration.js`.

 ```js
var CryptoZombies = artifacts.require("./CryptoZombies.sol");
module.exports = function(deployer) {
  deployer.deploy(CryptoZombies);
};
```
## Chapter 5
### Configuration Files
There is still one more thing to do before we can deploy. We'll have to edit the **configuration file** to tell Truffle the networks we want to deploy to.
#### Ethereum Test Networks

Several **public Ethereum test networks** let you test your contracts for free before you deploy them to the **main net** (remember once you deploy a contract to the main net it can't be altered). These test networks use a different consensus algorithm to the **main net** (usually PoA), and Ether is free to encourage thorough testing. we will be using `Rinkeby`, a public test network created by The Ethereum Foundation.
#### The truffle.js configuration file

Now, let's take a look at the **default Truffle configuration** file:
```js
$ cat truffle.js
/*
 * NB: since truffle-hdwallet-provider 0.0.5 you must wrap HDWallet providers in a
 * function when declaring them. Failure to do so will cause commands to hang. ex:
 *
 * mainnet: {
 *     provider: function() {
 *       return new HDWalletProvider(mnemonic, 'https://mainnet.infura.io/<infura-key>')
 *     },
 *     network_id: '1',
 *     gas: 4500000,
 *     gasPrice: 10000000000,
 *   },
 */
```
It is just an empty shell. Thus, we'll be required to update this file to allow us to deploy contracts to `Rinkeby` and the `Ethereum mainnet`.
##### Truffle's HD Wallet Provider
We asked you to install an additional package called `truffle-hdwallet-provider` that helps Truffle **sign transactions**.

Now, we want to edit our configuration file to use `HDWalletProvider`. To get this to work we'll add a line at the top of the file:
```js
        const HDWalletProvider = require("truffle-hdwallet-provider");
```
Next, we'll create a new variable to store our `mnemonic`:
```js
        const mnemonic = "onions carrots beans ...";
```
Note that we don't recommend storing secrets like a `mnemonic` or a `private key` in a configuration file. bwcause Config files are often pushed to `GitHub`, where anyone can see them, leaving you wide open to attack.To avoid revealing your mnemonic (or your private key!), you should read it from a file and add that file to `.gitignore`. To keep things simple in this case, we've copied the mnemonic and stored in a variable.
#### Set up Truffle for Rinkeby and Ethereum main net

Next, to make sure Truffle "knows" the networks we want to deploy to, we will have to create **two** separate objects- one for `Rinkeby` and the other one for the `Ethereum main net`:
```js
networks: {
  // Configuration for mainnet
  mainnet: {
    provider: function () {
      // Setting the provider with the Infura Mainnet address and Token
      return new HDWalletProvider(mnemonic, "https://mainnet.infura.io/v3/YOUR_TOKEN")
    },
    network_id: "1"
  },
  // Configuration for rinkeby network
  rinkeby: {
    // Special function to setup the provider
    provider: function () {
      // Setting the provider with the Infura Rinkeby address and Token
      return new HDWalletProvider(mnemonic, "https://rinkeby.infura.io/v3/YOUR_TOKEN")
    },
    // Network id is 4 for Rinkeby
    network_id: 4
  }
```
**Note**: the provider value is wrapped in a function, which ensures that it won't get initialized until it's needed.

Now, let's put these piece together and have a look at how our config file should look:
```js
// Initialize HDWalletProvider
const HDWalletProvider = require("truffle-hdwallet-provider");

// Set your own mnemonic here
const mnemonic = "YOUR_MNEMONIC";

// Module exports to make this configuration available to Truffle itself
module.exports = {
  // Object with configuration for each network
  networks: {
    // Configuration for mainnet
    mainnet: {
      provider: function () {
        // Setting the provider with the Infura Mainnet address and Token
        return new HDWalletProvider(mnemonic, "https://mainnet.infura.io/v3/YOUR_TOKEN")
      },
      network_id: "1"
    },
    // Configuration for rinkeby network
    rinkeby: {
      // Special function to setup the provider
      provider: function () {
        // Setting the provider with the Infura Rinkeby address and Token
        return new HDWalletProvider(mnemonic, "https://rinkeby.infura.io/v3/YOUR_TOKEN")
      },
      // Network id is 4 for Rinkeby
      network_id: 4
    }
  }
};
```
## Chapter 6
### Deploying Our Smart Contract

Actually deploying to `Rinkeby` is going to be straightforward. To do this, Truffle relies on something called a `migration`.

##### Migrations

It sounds like a **migration** involves a lot of stuff moving around, but in practice, a **migration** is nothing more than a **JavaScript** file that tells Truffle how to modify the state of our smart contracts.

Obviously, the first migration will just deploy the smart contract. Some other migrations deploy a new version of the code to add features or fix bugs.

In a **nutshell**, migrations provide a convenient way to keep track of the changes you make to your code.

If you want to deploy more than one contract, a separate migration file must be created for each contract. Migrations are always executed in order- 1,2,3, etc.

Deployment to the main net would require real money to pay for the gas and, once deployed, we won't be able to modify the code. Hence, it's best to deploy to Rinkeby first, and thoroughly test the code.
##### Get some Ether

Before doing the deployment, make sure there is enough **Ether** in your account. The easiest way to get Ether for testing purposes is through a service known as a `faucet`. We recommend the Authenticated Faucet running on Rinkeby.


```js
$truffle migrate --network rinkeby
Using network 'rinkeby'.

Running migration: 1_initial_migration.js
Deploying Migrations...
... 0xfc34bb95778e4bd53b80c6b705e5df6e753b1accecc38cfd35cb02c0e37ee223
Migrations: 0x9aba355cd9e68758c7bd9f1e58f911e0479fd29c
Saving successful migration to network...
... 0x485c7e8096d61a8a1e3efa8d9f5c675b707ca7580ecdb46a307b11146351fc9d
Saving artifacts...
Running migration: 2_crypto_zombies.js
Deploying CryptoZombies...
... 0xeed64afd64669674d0bc5a68e8c1f86a620d4bd338a652fc42f54353a252d07c
CryptoZombies: 0xccdfdc3d7e002965843d52e610370e8d29d5737f
Saving successful migration to network...
... 0xdcb1e77f4c1b3982bb15b10bec69bdafbc88fd4ea0b868971e850b02875de68e
Saving artifacts...
```
## Chapter 7
### Use Truffle with Loom
##### Loom Basechain

Now, if you want to build **DApps** on Ethereum, there's one thing you should be aware of - on the **main net**, users are required to **pay gas fees** for every transaction. But this isn't ideal for a user-facing DApp or a game. It can easily ruin the user experience.

Conversely, on `Loom`, your users can have much **speedier** and **gas-free transactions**, making it a much better fit for games and other non-financial applications.

This means that your `Loom` zombies will be fast zombies!

Deploying to Loom is no different from deploying to `Rinkeby`, or to the Ethereum main net. If you know how to do one, you also know how to do the other.
#### loom-truffle-provider

We at `Loom` are using Truffle to build, test, and deploy our smart contracts. To make our life easier, we developed something called a provider that lets Truffle deploy to Loom just like it deploys to Rinkeby or Ethereum main net.
```js
$ npm install loom-truffle-provider
+ loom-truffle-provider@0.8.0
added 227 packages from 193 contributors and audited 158456 packages in 50.265s
found 0 vulnerabilities
```

Without delving too much into details, the **provider** works like a bridge that makes `Web3` calls compatible with `Loom`. The beauty of it is that, to use it, you don't have to understand how it works under the hood.
## Chapter 8
### Deploy to Loom Testnet
we’re going to deploy our smart contract to the `Loom Testnet`, but before doing the deployment, some prep work is needed.First, we should create our own `Loom private key`. 

creating a `private key` is as as simple as this:
```js
$./loom genkey -a public_key -k private_key
local address: 0x42F401139048AB106c9e25DCae0Cf4b1Df985c39
local address base64: QvQBE5BIqxBsniXcrgz0sd+YXDk=
$cat private_key
/i0Qi8e/E+kVEIJLRPV5HJgn0sQBVi88EQw/Mq4ePFD1JGV1Nm14dA446BsPe3ajte3t/tpj7HaHDL84+Ce4Dg==
```
#### Updating truffle.js

The first thing we are required to do is to **initialize loom-truffle-provider**. The syntax is similar to the one we've already used for `HDWalletProvider`:
```js
        const LoomTruffleProvider = require('loom-truffle-provider');
```
Next, we'll have to let Truffle know how to deploy on **Loom testnet**. To do so, let's add a new object to `truffle.js`

```js
loom_testnet: {
  provider: function() {
    const privateKey = 'YOUR_PRIVATE_KEY'
    const chainId = 'extdev-plasma-us1';
    const writeUrl = 'http://extdev-plasma-us1.dappchains.com:80/rpc';
    const readUrl = 'http://extdev-plasma-us1.dappchains.com:80/query';
    return new LoomTruffleProvider(chainId, writeUrl, readUrl, privateKey);
    },
  network_id: '9545242630824'
}
```

Deploy to Loom command

```js
    $ truffle migrate --network loom_testnet
    
    Starting migrations...
======================
> Network name:    'loom_testnet'
> Network id:      9545242630824
> Block gas limit: 0

1_initial_migration.js
======================

  Replacing 'Migrations'
  ----------------------
  > transaction hash:    0x526a244695c8d7d0a6e3647a2c07e2d66b1a4fdb0c2623563eaaafca1d84b7d6
  > Blocks: 0            Seconds: 0
  > contract address:    0x98F1F4E2cdfe9BcD505Eb98F4361aFECDEfD8f88
  > account:             0x99A19685cC885e73B3910fb65566d4cb75b7dC70
  > balance:             0
  > gas used:            0
  > gas price:           0 gwei
  > value sent:          0 ETH
  > total cost:          0 ETH


  > Saving migration to chain.
  > Saving artifacts
  -------------------------------------
  > Total cost:                   0 ETH


  2_crypto_zombies.js
  =================

    Replacing 'CryptoZombies'
    -----------------------
    > transaction hash:    0x35f57adc9d96e4cd100c173243a3363b2c9d2a2fceea272914831d2c732f55f9
    > Blocks: 0            Seconds: 0
    > contract address:    0x4c51584414374dD45Ad66F030dB0115d4BcB63a3
    > account:             0x99A19685cC885e73B3910fb65566d4cb75b7dC70
    > balance:             0
    > gas used:            0
    > gas price:           0 gwei
    > value sent:          0 ETH
    > total cost:          0 ETH


    > Saving migration to chain.
    > Saving artifacts
    -------------------------------------
    > Total cost:                   0 ETH
```
## Chapter 9
### Deploy to Basechain
The process of deploying to Basechain (that is our Mainnet).

1.  Create a new **private key**.
2.  Creating a new private key is pretty straightforward. But since we're talking about deploying to the main net, it's time to get more serious about security. Thus, we'll show you how to **securely pass the private key to `Truffle`**.
3.  Tell `Truffle` how to deploy to Basechain by adding a new object to the `truffle.js` configuration file.
4.  Whitelist your deployment keys so you can **deploy to Basechain**.
5.  Lastly, we wrap everything up by actually **deploying the smart contract**.

#### Create a new private key

You already know how to **create a private key**. However, we must change the name of the file in which we're going to save it:
```js
./loom genkey -a mainnet_public_key -k mainnet_private_key
local address: 0x07419790A773Cc6a2840f1c092240922B61eC778
local address base64: B0GXkKdzzGooQPHAkiQJIrYex3g=
```
#### Securely pass the private key to Truffle

The next thing we want to do is to prevent the private key file from being pushed to GitHub. To do so, let's create a new file called **.gitignore**:
```js
touch .gitignore
```
Now let's **"tell"** GitHub that we want it to ignore the file in which we saved the private key by entering the following command:
```js
echo mainnet_private_key >> .gitignore
```
Now that we made sure our secrets aren't going to be pushed to GitHub, we must edit the `truffle.js` configuration file and make it so that `Truffle` reads the private key from this file.

Let's start by importing a couple of things:
```js
const { readFileSync } = require('fs')
const path = require('path')
const { join } = require('path')
```
Next, we would want to define a function that reads the private key from a file and initializes a new `LoomTruffleProvider`:
```js
function getLoomProviderWithPrivateKey (privateKeyPath, chainId, writeUrl, readUrl) {
  const privateKey = readFileSync(privateKeyPath, 'utf-8');
  return new LoomTruffleProvider(chainId, writeUrl, readUrl, privateKey);
}
```

##### Tell Truffle how to deploy to Basechain

Now, we must let Truffle know how to deploy to Basechain. To do so, let's add a new object to truffle.js
```js
basechain: {
  provider: function() {
    const chainId = 'default';
    const writeUrl = 'http://basechain.dappchains.com/rpc';
    const readUrl = 'http://basechain.dappchains.com/query';
    const privateKeyPath = path.join(__dirname, 'mainnet_private_key');
    const loomTruffleProvider = getLoomProviderWithPrivateKey(privateKeyPath, chainId, writeUrl, readUrl);
    return loomTruffleProvider;
    },
  network_id: '*'
}
```
At this point, your `truffle.js` file should look something like the following:
```js
// Initialize HDWalletProvider
const HDWalletProvider = require("truffle-hdwallet-provider");

const { readFileSync } = require('fs')
const path = require('path')
const { join } = require('path')


// Set your own mnemonic here
const mnemonic = "YOUR_MNEMONIC";

function getLoomProviderWithPrivateKey (privateKeyPath, chainId, writeUrl, readUrl) {
  const privateKey = readFileSync(privateKeyPath, 'utf-8');
  return new LoomTruffleProvider(chainId, writeUrl, readUrl, privateKey);
}

// Module exports to make this configuration available to Truffle itself
module.exports = {
  // Object with configuration for each network
  networks: {
    // Configuration for mainnet
    mainnet: {
      provider: function () {
        // Setting the provider with the Infura Rinkeby address and Token
        return new HDWalletProvider(mnemonic, "https://mainnet.infura.io/v3/YOUR_TOKEN")
      },
      network_id: "1"
    },
    // Configuration for rinkeby network
    rinkeby: {
      // Special function to setup the provider
      provider: function () {
        // Setting the provider with the Infura Rinkeby address and Token
        return new HDWalletProvider(mnemonic, "https://rinkeby.infura.io/v3/YOUR_TOKEN")
      },
      // Network id is 4 for Rinkeby
      network_id: 4
    },

    basechain: {
      provider: function() {
        const chainId = 'default';
        const writeUrl = 'http://basechain.dappchains.com/rpc';
        const readUrl = 'http://basechain.dappchains.com/query';
        const privateKeyPath = path.join(__dirname, 'mainnet_private_key');
        const loomTruffleProvider = getLoomProviderWithPrivateKey(privateKeyPath, chainId, writeUrl, readUrl);
        return loomTruffleProvider;
        },
      network_id: '*'
    }
  }
};
```
Run `truffle migrate --network basechain`
```js
$ truffle migrate --network basechain

Starting migrations...
======================
> Network name:    'basechain'
> Network id:      13654820909954
> Block gas limit: 0


1_initial_migration.js
======================

  Replacing 'Migrations'
  ----------------------
  > transaction hash:    0x5aaab85541e843d54926612e0b4f5faf2d48df8ad461b5f0de571565cabe8dc6
  > Blocks: 0            Seconds: 0
  > contract address:    0xc0eCB06312a20f9435E6Ed3B7eFD7bc0c0865BF2
  > account:             0xE046eB99Cbe99d6760ff01304E25F1dD9116F558
  > balance:             0
  > gas used:            0
  > gas price:           0 gwei
  > value sent:          0 ETH
  > total cost:          0 ETH


  > Saving migration to chain.
  > Saving artifacts
  -------------------------------------
  > Total cost:                   0 ETH

  2_crypto_zombies.js
  =================

    Replacing 'CryptoZombies'
    -----------------------
    > transaction hash:    0xeed20de79685ab845e1e848bb871ce87d2fc532d7c87f40b9b25f2bf75b4e3a8
    > Blocks: 0            Seconds: 0
    > contract address:    0xDf2986bbDa42404C8f43A5851aF887B2A3a9CFaB
    > account:             0xE046eB99Cbe99d6760ff01304E25F1dD9116F558
    > balance:             0
    > gas used:            0
    > gas price:           0 gwei
    > value sent:          0 ETH
    > total cost:          0 ETH


    > Saving migration to chain.
    > Saving artifacts
    -------------------------------------
    > Total cost:                   0 ETH
```
