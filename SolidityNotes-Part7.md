## Chapter 1
### Chainlink Data Feeds Introduction
Building a **DeFi** dapp, and want to give your users the ability to withdraw ETH worth a certain amount of USD. To fulfill this request, your smart contract (for simplicity's sake we'll call it the **"caller contract"** from here onwards) must know how much one Ether is worth.

And here's the thing: a JavaScript application can easily fetch this kind of information, making requests to the **Binance public API** (or any other service that publicly provides a price feed). But, a smart contract can't directly access data from the outside world.

Now we could build a JavaScript application ourselves, but then we are introducing a centralized point of failure! We also couldn't just pull from the Binance API, because that would be a centralized point of failure!

So what we want to do, is get our data from both a **decentralized oracle network** (DON) and **decentralized data sources**.

Chainlink is a framework for **decentralized oracle networks** (DONs), and is a way to get data in from multiple sources across multiple oracles. This DON aggregates data in a decentralized manner and places it on the blockchain in a smart contract (often referred to as a **"price reference feed" or "data feed"**) for us to read from. So all we have to do, is read from a contract that the Chainlink network is constantly updating for us!

Chainlink Data Feeds are the quickest way to connect your smart contracts to the **real-world data** such as **asset prices, reserve balances, NFT floor prices, and L2 sequencer health**.

*   **Price Feeds** -    Smart contracts often act in real-time on data such as prices of assets. This is especially true in **DeFi**.
*   **Proof of Reserve Feeds** -  provide the status of reserves for stablecoins, wrapped assets, and real world assets. Proof of Reserve Feeds operate similarly to Price Feeds, but provide answers in units of measurement such as **ounces (oz)** or number of tokens.
*   **NFT Floor Price Feeds** - provide the price of the lowest priced NFT available in a collection. 
*   **L2 sequencer uptime feeds** - It track the last known status of the sequencer on an L2 network at a given point in time. This helps you prevent mass liquidations by providing a grace period to allow customers to react to these events.

Using **Chainlink Data Feeds** is a way to **cheaply, more accurately**, and with **more security** gather data from the real world in this decentralized context. Since the data is coming from multiple sources, multiple people can partake in the ecosystem and it becomes even cheaper than running even a centralized oracle. The Chainlink network uses a system called **Off-Chain Reporting** to reach a consensus on data off-chain, and report the data in a cryptographically proven single transaction back on-chain for users to digest.

You can then make protocols like **Synthetix, Aave, and Compound** with this.
*   **Synthentix** - Synthetix is a protocol that allows you to generate **“synthetic”** assets. Synthetic assets are like derivatives in financial markets. These assets – also known as **“Synths”** – enable you to receive returns from another asset without owning it. Almost anything with a reliable trading history can be turned into a **Synth**. For example, crypto tokens like **Bitcoin and Ethereum, fiat currencies** like the USD and commodities like gold can all be in the form of synthetic assets.
*   **Aave** - Aave is a decentralised non-custodial liquidity protocol where users can participate as **depositors or borrowers**. Depositors provide liquidity to the market to earn a passive income, while borrowers are able to borrow in an over-collateralised (perpetually) or under-collateralised (one-block liquidity) fashion.
*   **Compound** - Compound is an **EVM** compatible protocol that enables supplying of crypto assets as collateral in order to borrow the base asset. Accounts can also earn interest by supplying the base asset to the protocol.

## Chapter 2
### Importing from NPM and Github
you can actually import code from outside your contracts? I know it's amazing right! Often times you don't have to have every piece of code directly in your project, you can borrow it from other applications!

We want to import the **AggregatorV3Interface** from the **Chainlink GitHub repository**. This interface will include all the functions that we need to interact with a Chainlink Data Feed contract, including functions like `latestRoundData()` which will return all the information we need.

Here is what the full interface looks like:
```js
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

interface AggregatorV3Interface {

  function decimals()
    external
    view
    returns (
      uint8
    );

  function description()
    external
    view
    returns (
      string memory
    );

  function version()
    external
    view
    returns (
      uint256
    );

  // getRoundData and latestRoundData should both raise "No data present"
  // if they do not have data to report, instead of returning unset values
  // which could be misinterpreted as actual reported values.
  function getRoundData(
    uint80 _roundId
  )
    external
    view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );

  function latestRoundData()
    external
    view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );
}
```
And we can find it in the GitHub repo for `AggregatorV3Interface`.

We can either import directly from **GitHub**, or from **NPM packages** to get this contract. The framework you're using (like Truffle, Brownie, Remix, Hardhat) will determine whether or not to use GitHub or NPM packages, but the syntax will be approximately the same!

```js
pragma solidity ^0.6.7;

import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {

}
```

## Chapter 3
### AggregatorV3Interface
Now, to interact with one of data fed contracts, since we already have the **interface**, all we need is the **address**. We can use the on-chain `Feeds Registry` which is an on-chain contract that keeps track of where all these feeds are, or we can just choose a contract address of our choosing by browsing all the contract addresses.

The `Chainlink Feed Registry` is an **on-chain mapping of assets to feeds**. It enables you to query Chainlink data feeds from asset addresses directly, without needing to know the feed contract addresses. They enable smart contracts to get the latest price of an asset in a single call, from a single contract.

Since we are trying to get the price of ETH in terms of USD, we need to pick the data feed that has this information.

**IMPORTANT**: Each network will have a **different address** for each piece of data you want. The address of the **ETH/USD** contract will be different on Mainnet Ethereum from Mainnet Polygon, from a Rinkeby testnet, etc.

##### Functions in  `AggregatorV3Interface`

*   `decimals` - 	The number of **decimals** in the response.
*   `description`  - The description of the aggregator that the proxy points to.
*   `getRoundData` -	Get data from a **specific** round.
*   `latestRoundData` - 	Get data from the **latest** round.
*   `version` -	The **version** representing the type of aggregator the proxy points to.

##### Example
```js
    function decimals() external view returns (uint8);
    function description() external view returns (string memory);
    function getRoundData(uint80 _roundId) external view returns (uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound);
    function latestRoundData() external view
    returns (
        uint80 roundId,
        int256 answer,
        uint256 startedAt,
        uint256 updatedAt,
        uint80 answeredInRound
    )
```
## Chapter 4
### Working with Tuples

Now we must retrieve the latest ETH price by calling the latestRoundData function of the priceFeed contract. This function has all the information we are looking for, plus some:

*   **roundId**: The round ID. Each price update gets a unique round ID.
*   **answer**: The current price.
*   **startedAt**: Timestamp of when the round started.
*   **updatedAt**: Timestamp of when the round was updated.
*   **answeredInRound**: The round ID of the round in which the answer was computed.
### Tuples

In programming, a `tuple` is a **data structure** that allows you to **group together a fixed number** of related values of different types. It is similar to an array, which also stores multiple values, but with a few key differences.

A tuple can hold any combination of data types, such as **integers, strings, floats, and other tuples**. It has a **fixed size**, which means you can't add or remove elements once it is defined. Each element in a tuple is **identified** by its **position, or index**, which starts at **0**. Tuples are defined by enclosing a comma-separated list of variables inside parentheses.

In summary, **tuples** are a useful **data structure** in Solidity that allow you to store related values. It is important to understand the **format and order of tuple values** when working with them on Etherscan to ensure that you are passing or receiving the correct values.

A **tuple** is a way in Solidity to create a **syntactic grouping of expressions**.

If a function returns multiple variables, such as the `latestRoundData` function, we consider the return type to be a tuple of many types.
```js
function latestRoundData() external view
    returns (
      uint80 roundId,
      int256 answer,
      uint256 startedAt,
      uint256 updatedAt,
      uint80 answeredInRound
    );
```

In order for us to assign variables to each return variable, we use the tuple syntax by grouping a **list of variables in parentheses**:
```js
 (uint80 roundId, int answer, uint startedAt, uint updatedAt, uint80 answeredInRound) = priceFeed.latestRoundData();
```
We can also **rename** the variables to anything that we please. For example, let's rename **answer to price**:
```js
 (uint80 roundId, int price, uint startedAt, uint updatedAt, uint80 answeredInRound) = priceFeed.latestRoundData();
```
Additionally, if there are variables that we are not going to use, it's often best practice to leave them as blanks, like so:
```js
 (,int price,,,) = priceFeed.latestRoundData();
```
This way, we can easily see what variables are important to us, and not declare anything extra.

#### Example
```js
pragma solidity ^0.6.7;

import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {
  AggregatorV3Interface public priceFeed;

  constructor() public {
    priceFeed = AggregatorV3Interface(0x8A753747A1Fa494EC906cE90E9f37563A8AF630e);
  }

  function getLatestPrice() public view returns (int) {
    (,int price,,,) = priceFeed.latestRoundData();
    return price;
  }
}
```

## Chapter 5
### Chainlink Data Feeds Decimals
A function that can pull the latest price of Ethereum in terms of the **United States Dollar!**

Now, as you know, decimals places don't work so well in Solidity,if we were to call this function, we'd get a response similar to this:
```js
        310523971888
```
So how do we know where the decimal place goes? The answer is simple: there is a `decimals` function that tells us!

#### Example
```js
pragma solidity ^0.6.7;

import "@chainlink/contracts/src/v0.6/interfaces/AggregatorV3Interface.sol";

contract PriceConsumerV3 {
  AggregatorV3Interface public priceFeed;

  constructor() public {
    priceFeed = AggregatorV3Interface(0x8A753747A1Fa494EC906cE90E9f37563A8AF630e);
  }

  function getLatestPrice() public view returns (int) {
    (,int price,,,) = priceFeed.latestRoundData();
    return price;
  }

  function getDecimals() public view returns (uint8) {
    uint8 decimals = priceFeed.decimals();
    return decimals;
  }
}
```
## Chapter 6
### Chainlink VRF Introduction
Randomness is very difficult to generate on blockchains. This is because every node on the blockchain must come to the same conclusion and form a **consensus**. Even though random numbers are versatile and useful in a variety of blockchain applications, they cannot be generated natively in smart contracts. The solution to this issue is `Chainlink VRF`, also known as **Chainlink Verifiable Random Function**.
#### The pseudo-random DNA
At this point in our contracts, we have been working with pseudo-randomness. Our keccak256 function was a great way to get started learning about how to create smart contracts. However, as we mentioned earlier, you can run into some major issues when creating smart contracts that rely on **pseudo-randomness**. Everything that is part of the **on-chain mechanism** is deterministic by design, including our hashing function.

When we tried to get random DNA from a string, we know that the DNA we are going to get is going to be the same every single time. This means, that our randomness creation function isn't actually random! We also can't rely on someone being honest when they input a "string" into this function.
```js
    uint(keccak256(abi.encodePacked(_str)))
```
So how can we fix this? Well, a naive approach is to use globally available variables like **msg.sender, block.difficulty, and block.timestamp**. You might see references that try to get random numbers like so:
```js
    uint(keccak256(abi.encodePacked(msg.sender, block.difficulty, block.timestamp)));
```
In this example, the developers try to mix up all these globally available variables to make it harder to predict the number. However, even these numbers have predictability:

*   `msg.sender` is known by the sender
*   `block.difficulty` is directly influenced by the miners
*   `block.timestamp` is predictable

So now it should be clear that everything inside the blockchain is deterministic and can lead to exploits if we try to use randomness from inside the blockchain. So how can we get randomness from outside the blockchain? You guessed it, with a **Chainlink Oracle**!

Let's take a look back at our good friend the zombie code, where we gave our zombie some pseudo-random DNA, and let's try to fix it. This time though, we will use the secure randomness of the **Chainlink Verifiable Randomness Function (Chainlink VRF)**.

#### Chainlink VRF

Chainlink VRF is a way to get randomness from outside the blockchain, but in a proven cryptographic manner. This is important because we always want our logic to be truly incorruptible. Another naive attempt at getting randomness outside the blockchain would be to use an off-chain API call to a service that returns a random number. But if that services goes down, is bribed, hacked, or otherwise, you could potentially be getting back a corrupt random number. Chainlink VRF includes on-chain verification contracts that cryptographically prove that the random number the contract is getting is really random.

#### Basic Request Model

Now, this is where we will be introduced to the basic request model of working with oracles.

The **first step** is when a smart contract (called the `"callee contract"`) makes a `"request"` to a Chainlink node which is comprised of a smart contract and the corresponding off-chain node. When it receives the request, the smart contract **emits a specific `event`** that the corresponding Chainlink node is subscribed to/looking for. This happens in **one transaction**.

The **Chainlink** oracle will then process the request (be it randomness, a data request, etc), and **return the data/computation** back to the `callee contract`, or a contract that will in turn send the response to the callee contract. This **"middle"** contract is often referred to as the `"oracle contract"`. This return process happens in a second separate transaction, so in total the basic request model is a **two transaction** event, and therefore, will take at the very minimum two blocks to complete.

This two transaction architecture is important, because it means that brute force attacks on randomness or data requests are throttled and impossible to hack without costing the attacker insane fees in gas costs.

To recap, the process is as such:

*   `Callee contract` makes a **request** in a transaction
    *   Callee contract or oracle contract emits an **event**
*   `Chainlink node` (Off-chain) is listening for the event, where the details of the request are logged in the event
*   In a second transaction created by the `Chainlink node`, it **returns the data on-chain** by calling a function described by the callee contract
*   In the case of the `Chainlink VRF`, a randomness proof is done to ensure **the number is truly random**

Now, similar to when we make a transaction on Ethereum or any Solidity compatible blockchain, as you know, we have to pay some **transaction gas**. To work with oracles, we have to pay a little bit of **oracle gas**, also known as the `LINK or Chainlink token`. The `LINK token` is specifically designed to work with oracles and ensure the security of the Chainlink oracle networks. Whenever we make a request following the basic request model, our contracts must be funded with a set amount of `LINK`, as defined by the specific oracle service that we are using (each service has different oracle gas fees).

#### Why didn't we do this with the data feeds?

With data feeds, an entire group of Chainlink nodes are being requested for data instead of just one Chainlink node (hence, it's a decentralized oracle network), however, only one entity has to kick off the request to the whole network. So we were able to benefit because someone else already created the requests to the oracles for the different kinds of data described in the data feeds. The data feeds are sponsored by a group of projects all making use of data feeds such as Aave, Compound, Synthetix, and more, and the updates to the network are being kicked off programmatically whenever there is a slight price change in the data they are reporting. Working together as a group, we can keep transaction costs down and create this collective good the whole ecosystem can benefit from!

In summary, someone else followed a bit more advanced version of the basic request model for us with the data feeds!

#### Chainlink VRF Under the Hood

The **Chainlink VRF** follows this basic request model, with one added benefit; since there is a **cryptographic proof on-chain** of the randomness of the number from a Chainlink VRF node, we are safe to work with a single Chainlink VRF node! As the technology gets better and better though, even more decentralized versions of the Chainlink VRF are being created, but luckily, we are good to work with this secure method of randomness for our smart contracts.

In brief, a smart contract requests randomness by specifying **a hash** used to uniquely identify a Chainlink oracle. That hash is used by the Chainlink node with its** own secret key** to generate a random number, which is then returned to the `contract on-chain`, along with a **cryptographic proof**. An on-chain contract (called the VRF Coordinator) takes the **random number along with the proof**, and is verified using the `oracle’s public key`. Relying on the widely accepted signature and proof verification capabilities of a blockchain, this enables contracts to consume only randomness that has also been verified by the same on-chain environment running the contract itself.

 Let's finally dive into learning how to pull a random number into our smart contract. We can get started, by once again pulling the Chainlink VRF contract code from `NPM / Github` that allows us to interact with a Chainlink VRF node. We are going to inherit the functionality of the `VRFConsumerbase` contract code into our code to emit events, and define what functions the Chainlink node is going to callback (respond) to.
 
 ```js
    pragma solidity ^0.6.6;
    import "@chainlink/contracts/src/v0.6/VRFConsumerBase.sol";
```
## Chapter 7
### Constructor in a constructor
You got it! The VRFConsumerbase contract includes all the code we need to send a request to a Chainlink oracle, including all the event logging code.

Now, as we said, to interact with a Chainlink node, we need to know a few variables.

*   The `address of the Chainlink token contract`. This is needed so our contract can tell if we have enough LINK tokens to pay for the gas.
*   The `VRF coordinator contract address`. This is needed to verify that the number we get is actually random.
*   The `Chainlink node keyhash`. This is used identify which Chainlink node we want to work with.
*   The `Chainlink node fee`. This represents the fee (gas) the Chainlink will charge us, expressed in LINK tokens.

we are going to inherit the functionality of this `VRFConsumerbase`. But how do we implement a constructor of an inherited contract? And the answer is that we can have a **constructor in a constructor**.

Let's take a look at this sample code:
```js
    import "./Y.sol";
    contract X is Y {
        constructor() Y() public{
        }
    }
```
To use a constructor of an inherited contract, we just put the constructor declaration as part of our contract's constructor.

We can do the same thing with the `VRFConsumerbase` contract:
```js
    constructor() VRFConsumerBase(
        0xb3dCcb4Cf7a26f6cf6B120Cf5A73875B7BBc655B, // VRF Coordinator
        0x01BE23585060835E02B77ef475b0Cc51aA1e0709  // LINK Token
    ) public{

    }
```
## Chapter 8
### Define our Chainlink VRF variables

Now this part is pretty easy, we just need to define our `keyHash and fee` in our **constructor**. We also should make a **global variable** called randomResult that will store the most recent return of a Chainlink VRF.

##### Example

Define three public variables:
    `keyHash, a bytes32`
    `fee, a uint256`
    `randomResult, a uint256`
    
```js
pragma solidity ^0.6.6;
import "@chainlink/contracts/src/v0.6/VRFConsumerBase.sol";

contract ZombieFactory is VRFConsumerBase {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    bytes32 public keyHash;
    uint256 public fee;
    uint256 public randomResult;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    constructor() VRFConsumerBase(
        0x6168499c0cFfCaCD319c818142124B7A15E857ab, // VRF Coordinator
        0x01BE23585060835E02B77ef475b0Cc51aA1e0709  // LINK Token
    ) public{
        keyHash = 0x2ed0feb3e7fd2022120aa84fab1945545a9f2ffc9076fd6156fa96eaff4c1311;
        fee = 100000000000000000;

    }
}
```
## Chapter 9
### The requestRandomness and fulfillRandomness functions
Now we can create our function that calls the Chainlink node.

Remember, the **Chainlink VRF** follows the basic request model, so we need to define:

1.  A function to **request the random number**
2.  A function for the **Chainlink node to return the random number to**.

Remember, the **Chainlink node** is actually going to call the `VRF Coordinator` first to verify the number is random, then the VRF Coordinator will be the one to call our `ZombieFactory` contract.

Since we are importing the `VRFConsumerBase` contract, we can use the two built in functions that do both of these!

A. `requestRandomness`

*   This function checks to see that our contract has **LINK tokens** to pay a Chainlink node
*   Then, it sends some **LINK tokens to the Chainlink node**
*   **Emits an event** that the Chainlink node is looking for
*   Assigns a **requestId** to our request for a random number on-chain

B. `fulfillRandomness`

*   The **Chainlink node** first calls a function on the **VRF Coordinator and includes a random number**
*   The **VRF Coordinator** checks to see **if the number is random**
*   Then, it returns the random number the **Chainlink node created**, along with the original **requestID from our request**

```js
ontract ZombieFactory is VRFConsumerBase {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    bytes32 public keyHash;
    uint256 public fee;
    uint256 public randomResult;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies;

    constructor() VRFConsumerBase(
        0x6168499c0cFfCaCD319c818142124B7A15E857ab, // VRF Coordinator
        0x01BE23585060835E02B77ef475b0Cc51aA1e0709  // LINK Token
    ) public{
        keyHash = 0x2ed0feb3e7fd2022120aa84fab1945545a9f2ffc9076fd6156fa96eaff4c1311;
        fee = 100000000000000000;

    }

    function _createZombie(string memory _name, uint _dna) private {
        zombies.push(Zombie(_name, _dna));
    }


    function getRandomNumber() public returns (bytes32 requestId) {
        return requestRandomness(keyHash, fee);
    }

    function fulfillRandomness(bytes32 requestId, uint256 randomness) internal override {
        randomResult = randomness;
    }
  }
  ```