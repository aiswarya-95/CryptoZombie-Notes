## Chapter 1
### Web3.js
There are a few different aspects to developing blockchain applications with Ethereum:

1.   **Smart contract development** - writing code that gets deployed to the blockchain with the Solidity programming language
2.   **Developing websites or clients that interact with the blockchain** - writing code that reads and writes data from the blockchain with smart contracts.

Web3.js enables you to fulfill the second responsibility: **developing clients that interact with The Etherem Blockchain**. Web3.js is a collection of **libraries** that allow developers to interact with a remote or local Ethereum node using **HTTP, IPC, or WebSocket**. Using this library, you can develop **websites** or **clients** that interact with the blockchain. This can be actions like sending Ether from one user to another, checking data from smart contracts, creating smart contracts, among other things. Ethereum nodes provide interfaces to users in order to complete transactions: of which, **nodes** receive this information through a **JSON RPC** interface. This is an encoding format that allows running processes to receive new and verify existing data. **Web3.js** helps to make the process of running and selecting nodes participating in the Ethereum network simpler and easier to grasp.

The **web3.js** library is a collection of modules that contain functionality for the ethereum ecosystem.

*   `web3-eth` is for the ethereum blockchain and smart contracts.
*   `web3-shh` is for the whisper protocol, to communicate p2p and broadcast.
*   `web3-bzz` is for the swarm protocol, the decentralized file storage.
*   `web3-utils` contains useful helper functions for Dapp developers.


Now we're going to create a basic web page where your users can interact with it.

To do this, we're going to use a JavaScript library from the Ethereum Foundation called **Web3.js**.
Remember, the Ethereum network is made up of nodes, with each containing a copy of the blockchain. When you want to call a function on a smart contract, you need to query one of these nodes and tell it:

1.  The address of the smart contract
2.  The function you want to call, and
3.  The variables you want to pass to that function.

Ethereum nodes only speak a language called **JSON-RPC** which stands for **"Remote Procedure Call"** protocol., which isn't very human-readable. A query to tell the node you want to call a function on a contract looks something like this:
```js
// Yeah... Good luck writing all your function calls this way!
// Scroll right ==>
{"jsonrpc":"2.0","method":"eth_sendTransaction","params":[{"from":"0xb60e8dd61c5d32be8058bb8eb970870f07233155","to":"0xd46e8dd67c5d32be8058bb8eb970870f07244567","gas":"0x76c0","gasPrice":"0x9184e72a000","value":"0x9184e72a","data":"0xd46e8dd67c5d32be8d46e8dd67c5d32be8058bb8eb970870f072445675058bb8eb970870f072445675"}],"id":1}
```

Luckily, Web3.js hides these nasty queries below the surface, so you only need to interact with a convenient and easily readable **JavaScript** interface.

Instead of needing to construct the above query, calling a function in your code will look something like this:
```js
CryptoZombies.methods.createRandomZombie("Vitalik Nakamoto 🤔")
  .send({ from: "0xb60e8dd61c5d32be8058bb8eb970870f07233155", gas: "3000000" })
```
#### Getting started

Depending on your project's workflow, you can add Web3.js to your project using most package tools:
```js
// Using NPM
npm install web3

// Using Yarn
yarn add web3

// Using Bower
bower install web3
```
Or you can simply download the minified .js file from github and include it in your project:
```js
<script language="javascript" type="text/javascript" src="web3.min.js"></script>
```
Since we don't want to make too many assumptions about your development environment and what package manager you use, we're going to simply include Web3 in our project using a script tag as above.

## Chapter 2
### Web3 providers

The first thing we need is a **Web3 Provider**. Ethereum is made up of `nodes` that all share a copy of the same data. Setting a Web3 Provider in `Web3.js` tells our code which node we should be talking to handle our reads and writes. It's kind of like setting the **URL** of the remote web server for your **API** calls in a traditional web app.

You could host your own Ethereum `node` as a **provider**. However, there's a **third-party** service that makes your life easier so you don't need to maintain your own Ethereum node in order to provide a DApp for your users — `Infura`.
### Infura
`Infura` offers developers, dapp teams, and enterprises across a range of industries a suite of tools to connect their apps to the Ethereum network and other decentralized platforms.
“`Infura` essentially provides the necessary tools for any application to start developing on Ethereum immediately, without the need to run the complicated infrastructure themselves. This is equally necessary for developers with no traffic testing on networks such as **Kovan, Rinkeby, or Ropsten, or ones with tons of traffic running on mainnet**. `Infura provides a connection for all developers utilizing the Ethereum blockchain`.”

`Infura` provides what are called endpoints to Ethereum's API. If we're thinking in terms of a menu at a restaurant, then Infura is like a server, or a cashier: the person who takes your order and rings you up. Infura provides an endpoint that your code can 'point to', so that your requests to receive or record information can always get to the network. And, of course, they return to you the information you've requested.

Like a restaurant, Infura does charge for this service, but only after you've made a lot of requests in one month. For developers who are just testing something out, it's free to start and easy to set up an account.

`Infura` is a service that maintains a set of Ethereum nodes with a caching layer for fast reads, which you can access for free through their API. Using Infura as a provider, you can reliably send and receive messages to/from the Ethereum blockchain without needing to set up and maintain your own node.

You can set up Web3 to use Infura as your web3 provider as follows:
```js
        var web3 = new Web3(new Web3.providers.WebsocketProvider("wss://mainnet.infura.io/ws"));
```
However, since our DApp is going to be used by many users — and these users are going to WRITE to the blockchain and not just read from it — we'll need a way for these users to sign transactions with their private key.

**Note**: Ethereum (and blockchains in general) use a public / private key pair to digitally sign transactions. Think of it like an extremely secure password for a digital signature. That way if I change some data on the blockchain, I can prove via my public key that I was the one who signed it — but since no one knows my private key, no one can forge a transaction for me.

it's probably not a good idea to try to manage users' private keys yourself in our app's front-end.But luckily you don't need to — there are already services that handle this for you. The most popular of these is `Metamask`.

### Metamask

`Metamask` is a browser extension for **Chrome and Firefox** that lets users securely manage their Ethereum accounts and private keys, and use these accounts to interact with websites that are using Web3.js. `MetaMask` is the trailblazing tool enabling user interactions and experience on Web3. It is currently available as a browser extension and as a mobile app on both Android and iOS devices.

And as a developer, if you want users to interact with your DApp through a website in their web browser (like we're doing with our CryptoZombies game), you'll definitely want to make it Metamask-compatible.

**Note**: Metamask uses Infura's servers under the hood as a web3 provider, just like we did above — but it also gives the user the option to choose their own web3 provider. So by using Metamask's web3 provider, you're giving the user a choice, and it's one less thing you have to worry about in your app. MetaMask was created to meet the needs of secure and usable Ethereum-based web sites. 

MetaMask comes pre-loaded with fast connections to the Ethereum blockchain and several test networks via our friends at `Infura`. This allows you to get started without synchronizing a full node, while still providing the option to upgrade your security and use the blockchain provider of your choice. Today, **MetaMask** is compatible with any blockchain that exposes an Ethereum-compatible `JSON RPC API`, including custom and private blockchains.

#### Using Metamask's web3 provider

Metamask injects their `web3 provider` into the browser in the global **JavaScript** object **web3**. So your app can check to see if **web3** exists, and if it does use `web3.currentProvider` as its provider.

Here's some template code provided by **Metamask** for how we can detect to see if the user has Metamask installed, and if not tell them they'll need to install it to use our app:
```js
window.addEventListener('load', function() {

  // Checking if Web3 has been injected by the browser (Mist/MetaMask)
  if (typeof web3 !== 'undefined') {
    // Use Mist/MetaMask's provider
    web3js = new Web3(web3.currentProvider);
  } else {
    // Handle the case where the user doesn't have web3. Probably
    // show them a message telling them to install Metamask in
    // order to use our app.
  }

  // Now you can start your app & access web3js freely:
  startApp()
})
```
You can use this boilerplate code in all the apps you create in order to require users to have Metamask to use your DApp.

**Note**: There are other private key management programs your users might be using besides MetaMask, such as the web **browser Mist**. However, they all implement a common pattern of injecting the variable **web3**, so the method we describe here for detecting the user's web3 provider will work for these as well.

## Chapter 3
###  Talking to Contracts

Now that we've initialized Web3.js with MetaMask's Web3 provider, let's set it up to talk to our smart contract.

Web3.js will need 2 things to talk to your contract: 
*   its `address`
*  its `ABI`
#### Contract Address

After you finish writing your smart contract, you will **compile** it and **deploy** it to Ethereum. After you deploy your contract, it gets a **fixed address** on Ethereum where it will live forever. the address of the **CryptoKitties** contract on Ethereum **mainnet** is `0x06012c8cf97BEaD5deAe237070F9587f8E7A266d`. You'll need to copy this address after deploying in order to talk to your smart contract.

#### Contract ABI

The other thing Web3.js will need to talk to your contract is its `ABI`.

**ABI** stands for `Application Binary Interface`. Basically it's a representation of your contracts' methods in **JSON format** that tells **Web3.js** how to format function calls in a way your contract will understand. It is a collection of `Fragments` which specify how to interact with various components of a Contract. where each fragment specifies:

*   An Error
*   An Event
*   A Function
*   A Constructor 

An `Interface` helps organize Fragments by type as well as provides the functionality required to encode, decode and work with each component.
When you **compile** your contract to deploy to Ethereum, the **Solidity compiler** will give you the **ABI**, so you'll need to copy and save this in addition to the contract address.

#### Instantiating a Web3.js Contract

Once you have your **contract's address and ABI**, you can instantiate it in Web3 as follows:
```js
// Instantiate myContract
var myContract = new web3js.eth.Contract(myABI, myContractAddress);
```

## Chapter 4
### Calling Contract Functions
Web3.js has two methods we will use to call functions on our contract:
*   call 
*   send.

The reason we have two separate methods for calling functions that **do or do not alter the state of a smart contract** is because the former requires a transaction on the blockchain and the expense of ether in order to be effective.

The ether required to execute a smart contract function is called **“gas”** and it is a transaction that the user of the application will have to accept. If the user is using an ethereum wallet such as the Metamask plugin in their browser, they will have to accept the expense of the transaction via a popup window. With functions called via the `call()` method on the other hand, they can happen silently in the background and do not require user input to complete.

#### `Call`: Reading value from a smart contract
`call` is used for **view** and **pure** functions. It only runs on the local node, and won't create a transaction on the blockchain.

**Review**: `view` and `pure` functions are **read-only** and **don't change state** on the blockchain. They also **don't cost any gas**, and the user won't be prompted to sign a transaction with MetaMask.

Using Web3.js, you would `call` a function named **myMethod** with the parameter **123** as follows:
```js
        myContract.methods.myMethod(123).call()
```

#### `Send`: Sending a transaction to a smart contract function
`send` will create a transaction and change data on the blockchain. You'll need to use send for any functions that **aren't view or pure**.

**Note**: sending a transaction will require the user to **pay gas**, and will pop up their Metamask to prompt them to sign a transaction. When we use Metamask as our web3 provider, this all happens automatically when we call **send()**, and we don't need to do anything special in our code. Pretty cool!

Using Web3.js, you would send a transaction calling a function named **myMethod** with the parameter **123** as follows:
```js
        myContract.methods.myMethod(123).send()
```
The syntax is almost identical to `call()`.
#### Getting Zombie Data

Now let's look at a real example of using call to access data on our contract.

Recall that we made our array of zombies `public`:
```js
        Zombie[] public zombies;
```
In Solidity, when you declare a variable `public`, it automatically creates a public **"getter"** function with the same name. So if you wanted to look up the zombie with `id 15`, you would call it as if it were a function: `zombies(15)`.

Here's how we would write a **JavaScript function** in our front-end that would take a zombie id, query our contract for that zombie, and return the result:

```js
function getZombieDetails(id) {
  return cryptoZombies.methods.zombies(id).call()
}

// Call the function and do something with the result:
getZombieDetails(15)
.then(function(result) {
  console.log("Zombie 15: " + JSON.stringify(result));
});
```

`cryptoZombies.methods.zombies(id).call()` will communicate with the Web3 provider node and tell it to return the zombie with index id from `Zombie[] public zombies` on our contract.

Note that this is **asynchronous**, like an API call to an external server. So Web3 returns a promise here. Once the promise resolves (which means we got an answer back from the web3 provider), our example code continues with the then statement, which logs result to the console.

result will be a JavaScript object that looks like this:
```js
{
  "name": "H4XF13LD MORRIS'S COOLER OLDER BROTHER",
  "dna": "1337133713371337",
  "level": "9999",
  "readyTime": "1522498671",
  "winCount": "999999999",
  "lossCount": "0" // Obviously.
}
```
We could then have some front-end logic to parse this object and display it in a meaningful way on the front-end.

### Chapter 5
### Metamask & Accounts

Now let's put some pieces together — let's say we want our app's homepage to display a user's entire zombie army.

Obviously we'd first need to use our function `getZombiesByOwner(owner)` to look up all the **IDs** of zombies the current user owns.

But our Solidity contract is expecting **owner** to be a **Solidity address**. How can we know the address of the user using our app?
#### Getting the user's account in MetaMask

MetaMask allows the user to **manage multiple accounts** in their extension. We can see which account is currently active on the injected **web3** variable via:
```js
        var userAccount = web3.eth.accounts[0]
```
Because the user can **switch the active account** at any time in MetaMask, our app needs to monitor this variable to see if it has changed and update the UI accordingly. For example, if the user's homepage displays their zombie army, when they change their account in MetaMask, we'll want to update the page to show the zombie army for the new account they've selected.

We can do that with a setInterval loop as follows:
```js
var accountInterval = setInterval(function() {
  // Check if account has changed
  if (web3.eth.accounts[0] !== userAccount) {
    userAccount = web3.eth.accounts[0];
    // Call some function to update the UI with the new account
    updateInterface();
  }
}, 100);
```
What this does is check every **100 milliseconds** to see if userAccount is still equal `web3.eth.accounts[0]` (i.e. does the user still have that account active). If not, it reassigns userAccount to the currently active account, and calls a function to update the display.
#### Example
```js
<script>
      var cryptoZombies;
      var userAccount;

      function startApp() {
        var cryptoZombiesAddress = "YOUR_CONTRACT_ADDRESS";
        cryptoZombies = new web3js.eth.Contract(cryptoZombiesABI, cryptoZombiesAddress);

        var accountInterval = setInterval(function() {
          // Check if account has changed
          if (web3.eth.accounts[0] !== userAccount) {
            userAccount = web3.eth.accounts[0];
            // Call a function to update the UI with the new account
            getZombiesByOwner(userAccount)
            .then(displayZombies);
          }
        }, 100);
      }

      function displayZombies(ids) {
        // Start here
      }

      function getZombieDetails(id) {
        return cryptoZombies.methods.zombies(id).call()
      }

      function zombieToOwner(id) {
        return cryptoZombies.methods.zombieToOwner(id).call()
      }

      function getZombiesByOwner(owner) {
        return cryptoZombies.methods.getZombiesByOwner(owner).call()
      }
 window.addEventListener('load', function() {

        // Checking if Web3 has been injected by the browser (Mist/MetaMask)
        if (typeof web3 !== 'undefined') {
          // Use Mist/MetaMask's provider
          web3js = new Web3(web3.currentProvider);
        } else {
          // Handle the case where the user doesn't have Metamask installed
          // Probably show them a message prompting them to install Metamask
        }

        // Now you can start your app & access web3 freely:
        startApp()

      })
    </script>
```
## Chapter 6
### Displaying our Zombie Army - a rough example
We've added an empty `<div id="zombies"></div>` to the body of our document, as well as an empty `displayZombies` function.

Recall that in the previous chapter we called `displayZombies` from inside `startApp()` with the result of a call to `getZombiesByOwner`. It will be passed an array of zombie IDs that looks something like:
```js
        [0, 13, 47]
```
Thus we'll want our displayZombies function to:


1.  First clear the contents of the `#zombies div`, if there's anything already inside it. (This way if the user changes their active MetaMask account, it will clear their old zombie army before loading the new one).
2.  Loop through each `id`, and for each one call `getZombieDetails(id)` to look up all the information for that zombie from our smart contract, then
3.  Put the information about that zombie into an HTML template to format it for display, and append that template to the `#zombies div`.

Again, we're just using **JQuery** here, which doesn't have a templating engine by default, so this is going to be ugly. But here's a simple example of how we could output this data for each zombie:
```js
// Look up zombie details from our contract. Returns a `zombie` object
getZombieDetails(id)
.then(function(zombie) {
  // Using ES6's "template literals" to inject variables into the HTML.
  // Append each one to our #zombies div
  $("#zombies").append(`<div class="zombie">
    <ul>
      <li>Name: ${zombie.name}</li>
      <li>DNA: ${zombie.dna}</li>
      <li>Level: ${zombie.level}</li>
      <li>Wins: ${zombie.winCount}</li>
      <li>Losses: ${zombie.lossCount}</li>
      <li>Ready Time: ${zombie.readyTime}</li>
    </ul>
  </div>`);
});
```
##### What about displaying the zombie sprites?

In the above example, we're simply displaying the **DNA** as a **string**. But in your DApp, you would want to convert this to images to display your zombie.

We did this by splitting up the **DNA** string into substrings, and having every 2 digits correspond to an image. Something like:
```js
// Get an integer 1-7 that represents our zombie head:
var head = parseInt(zombie.dna.substring(0, 2)) % 7 + 1

// We have 7 head images with sequential filenames:
var headSrc = "../assets/zombieparts/head-" + head + ".png"
```
Each component is positioned with CSS using absolute positioning, to overlay it over the other images.

## Chapter 7
### Sending Transactions
Now let's look at using send functions to change data on our smart contract.

There are a few major differences from call functions:

1.  sending a transaction requires a **from address** of who's calling the function (which becomes msg.sender in your Solidity code). We'll want this to be the user of our DApp, so MetaMask will pop up to prompt them to sign the transaction.
2.  sending a transaction **costs gas**
3.  There will be a **significant delay** from when the user sends a transaction and when that transaction actually takes effect on the blockchain. This is because we have to wait for the transaction to be included in a block, and the block time for Ethereum is on average **15 seconds**. If there are a lot of pending transactions on Ethereum or if the user sends too **low of a gas price**, our transaction may have to wait several blocks to get included, and this **could take minutes**.

Thus we'll need logic in our app to handle the asynchronous nature of this code.
##### Creating zombies

Let's look at an example with the first function in our contract a new user will call: `createRandomZombie`.

As a review, here is the Solidity code in our contract:
```js
function createRandomZombie(string _name) public {
  require(ownerZombieCount[msg.sender] == 0);
  uint randDna = _generateRandomDna(_name);
  randDna = randDna - randDna % 100;
  _createZombie(_name, randDna);
}
```
Here's an example of how we could call this function in **Web3.js using MetaMask**:
```js
function createRandomZombie(name) {
  // This is going to take a while, so update the UI to let the user know
  // the transaction has been sent
  $("#txStatus").text("Creating new zombie on the blockchain. This may take a while...");
  // Send the tx to our contract:
  return cryptoZombies.methods.createRandomZombie(name)
  .send({ from: userAccount })
  .on("receipt", function(receipt) {
    $("#txStatus").text("Successfully created " + name + "!");
    // Transaction was accepted into the blockchain, let's redraw the UI
    getZombiesByOwner(userAccount).then(displayZombies);
  })
  .on("error", function(error) {
    // Do something to alert the user their transaction has failed
    $("#txStatus").text(error);
  });
}
```
Our function sends a transaction to our Web3 provider, and chains some event listeners:

*   `receipt` will fire when the transaction is included into a block on Ethereum, which means our zombie has been created and saved on our contract
*   `error` will fire if there's an issue that prevented the transaction from being included in a block, such as the user not sending enough gas. We'll want to inform the user in our UI that the transaction didn't go through so they can try again.

**Note**: You can optionally specify gas and gasPrice when you call send,** e.g. .send({ from: userAccount, gas: 3000000 })**. If you don't specify this, MetaMask will let the user choose these values.

## Chapter 8
### Calling Payable Functions
Let's look at another type of function that requires special treatment in Web3.js — `payable` functions.
Recall in `ZombieHelper`, we added a payable function where the user can level up:
```js
function levelUp(uint _zombieId) external payable {
  require(msg.value == levelUpFee);
  zombies[_zombieId].level++;
}
```
The way to send Ether along with a function is simple, with one caveat: we need to specify how much to send in `wei`, not Ether.

#### What's a `Wei`?

A `wei` is the smallest sub-unit of Ether — there are **10^18** wei in one **ether**.

That's a lot of zeroes to count — but luckily Web3.js has a conversion utility that does this for us.
```js
// This will convert 1 ETH to Wei
web3js.utils.toWei("1");
```
In our DApp, we set `levelUpFee = 0.001 ether`, so when we call our levelUp function, we can make the user send 0.001 Ether along with it using the following code:
```js
cryptoZombies.methods.levelUp(zombieId)
.send({ from: userAccount, value: web3js.utils.toWei("0.001", "ether") })
```

##### Example
```js
 function levelUp(zombieId) {
        $("#txStatus").text("Leveling up your zombie...");
        return cryptoZombies.methods.levelUp(zombieId)
        .send({ from: userAccount, value: web3.utils.toWei("0.001", "ether") })
        .on("receipt", function(receipt) {
          $("#txStatus").text("Power overwhelming! Zombie successfully leveled up");
        })
        .on("error", function(error) {
          $("#txStatus").text(error);
        });
      }
```

## Chapter 9
##  Subscribing to Events

As you can see, interacting with your contract via **Web3.js** is pretty straightforward — once you have your environment set up, `calling` functions and `sending` transactions is not all that different from a normal web **API**.

There's one more aspect we want to cover — subscribing to events from your contract.
Listening for **New Zombies**

If you recall from `zombiefactory.sol`, we had an event called **NewZombie** that we fired every time a new zombie was created:
```js
        event NewZombie(uint zombieId, string name, uint dna);
```
In `Web3.js`, you can subscribe to an event so your web3 provider triggers some logic in your code every time it fires:
```js
cryptoZombies.events.NewZombie()
.on("data", function(event) {
  let zombie = event.returnValues;
  // We can access this event's 3 return values on the `event.returnValues` object:
  console.log("A new zombie was born!", zombie.zombieId, zombie.name, zombie.dna);
}).on("error", console.error);
```
Note that this would trigger an alert every time ANY zombie was created in our DApp — not just for the current user. What if we only wanted alerts for the current user?
#### Using indexed

In order to **filter** events and only listen for changes related to the **current user**, our Solidity contract would have to use the `indexed` keyword, like we did in the Transfer event of our **ERC721** implementation:
```js
    event Transfer(address indexed _from, address indexed _to, uint256 _tokenId);
```
In this case, because **_from and _to** are indexed, that means we can filter for them in our event listener in our front end:
```js
// Use `filter` to only fire this code when `_to` equals `userAccount`
cryptoZombies.events.Transfer({ filter: { _to: userAccount } })
.on("data", function(event) {
  let data = event.returnValues;
  // The current user just received a zombie!
  // Do something here to update the UI to show it
}).on("error", console.error);
```
As you can see, using **events** and **indexed** fields can be quite a useful practice for listening to changes to your contract and reflecting them in your app's front-end.
#### Querying past events

We can even query past events using `getPastEvents`, and use the filters `fromBlock` and `toBlock` to give Solidity a time range for the event logs ("block" in this case referring to the Ethereum block number):
```js
cryptoZombies.getPastEvents("NewZombie", { fromBlock: 0, toBlock: "latest" })
.then(function(events) {
  // `events` is an array of `event` objects that we can iterate, like we did above
  // This code will get us a list of every zombie that was ever created
});
```
Because you can use this method to query the event logs since the beginning of time, this presents an interesting use case: Using events as a cheaper form of storage.

If you recall, saving data to the blockchain is one of the most expensive operations in Solidity. But using events is much much cheaper in terms of gas.The tradeoff here is that events are not readable from inside the smart contract itself. But it's an important use-case to keep in mind if you have some data you want to be historically recorded on the blockchain so you can read it from your app's front-end.

For example, we could use this as a historical record of zombie battles — we could create an event for every time one zombie attacks another and who won. The smart contract doesn't need this data to calculate any future outcomes, but it's useful data for users to be able to browse from the app's front-end.

##### Example
```js
function startApp() {
        var cryptoZombiesAddress = "YOUR_CONTRACT_ADDRESS";
        cryptoZombies = new web3js.eth.Contract(cryptoZombiesABI, cryptoZombiesAddress);

        var accountInterval = setInterval(function() {
          // Check if account has changed
          if (web3.eth.accounts[0] !== userAccount) {
            userAccount = web3.eth.accounts[0];
            // Call a function to update the UI with the new account
            getZombiesByOwner(userAccount)
            .then(displayZombies);
          }
        }, 100);

        cryptoZombies.events.Transfer({ filter: { _to: userAccount } })
    .on("data", function(event) {
    let data = event.returnValues;
      getZombiesByOwner(userAccount).then(displayZombies);
      }).on("error", console.error);
      }
```