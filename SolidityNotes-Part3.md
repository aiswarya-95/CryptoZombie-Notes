# Lesson 3
## Chapter 1
## Immutability of Contracts
The contract is `immutable`, which means that it can never be modified or updated again.
The initial code you deploy to a contract is there to stay, permanently, on the blockchain. This is one reason security is such a huge concern in Solidity. If there's a flaw in your contract code, there's no way for you to patch it later. You would have to tell your users to start using a different smart contract address that has the fix.

In one line, the `immutable` keyword is used to define storage identifiers in a smart contract which would be initialized only during the contract deployment in the contract constructor or inline and there on out behave like a `constant`.

State variables can be declared as `constant` or `immutable`. In both cases, the variables cannot be modified after the contract has been constructed. For constant variables, the value has to be fixed at `compile-time`, while for immutable, it can still be assigned at construction time.

```js
contract Contract1 {
    uint public constant c1 = 7894578;
    uint public immutable i1;
    uint public v1;

    constructor(uint _i1, uint _v1){
        i1=_i1;
        v1=_v1;
    }
    function setV1(uint _v) public {
        v1=_v;
    }
}
```
## Chapter 2
### Ownable Contracts
Some functions are necessary for **only configuration purposes** or **one-time calculations**. Such functions if exposed publicly can be used maliciously or mistakenly to exhaust gas. To prevent misuse or reduce gas cost, such functions must be restricted to only selected external addresses (for simplicity, owner). To solve this, `ownable contracts` can be used. One common practice that has emerged is to make contracts `Ownable` — meaning they have an owner (you) who has special privileges.
### OpenZeppelin's Ownable contract
The `Ownable` contract taken from the `OpenZeppelin` Solidity library. OpenZeppelin is a library of secure and community-vetted smart contracts that you can use in your own DApps.

```js
/**
 * @title Ownable
 * @dev The Ownable contract has an owner address, and provides basic authorization control
 * functions, this simplifies the implementation of "user permissions".
 */
contract Ownable {
  address private _owner;

  event OwnershipTransferred(
    address indexed previousOwner,
    address indexed newOwner
  );

  /**
   * @dev The Ownable constructor sets the original `owner` of the contract to the sender
   * account.
   */
  constructor() internal {
    _owner = msg.sender;
    emit OwnershipTransferred(address(0), _owner);
  }

  /**
   * @return the address of the owner.
   */
  function owner() public view returns(address) {
    return _owner;
  }

  /**
   * @dev Throws if called by any account other than the owner.
   */
  modifier onlyOwner() {
    require(isOwner());
    _;
  }

  /**
   * @return true if `msg.sender` is the owner of the contract.
   */
  function isOwner() public view returns(bool) {
    return msg.sender == _owner;
  }

  /**
   * @dev Allows the current owner to relinquish control of the contract.
   * @notice Renouncing to ownership will leave the contract without an owner.
   * It will not be possible to call the functions with the `onlyOwner`
   * modifier anymore.
   */
  function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(_owner, address(0));
    _owner = address(0);
  }

  /**
   * @dev Allows the current owner to transfer control of the contract to a newOwner.
   * @param newOwner The address to transfer ownership to.
   */
  function transferOwnership(address newOwner) public onlyOwner {
    _transferOwnership(newOwner);
  }

  /**
   * @dev Transfers control of the contract to a newOwner.
   * @param newOwner The address to transfer ownership to.
   */
  function _transferOwnership(address newOwner) internal {
    require(newOwner != address(0));
    emit OwnershipTransferred(_owner, newOwner);
    _owner = newOwner;
  }
}
```
**Constructors**: `constructor()` is a constructor, which is an optional special function. It will get executed only one time, when the contract is first created.
**Function Modifiers**: modifier `onlyOwner()`. Modifiers are kind of half-functions that are used to modify other functions, usually to check some requirements prior to execution. In this case, onlyOwner can be used to limit access so only the owner of the contract can run this function.

So the `Ownable contract` basically does the following:

1. When a contract is created, its `constructor` sets the owner to `msg.sender` (the person who deployed it)
2. It adds an **onlyOwner** modifier, which can restrict access to certain functions to only the owner
3. It allows you to transfer the contract to a new owner

**Example** #### `ownable.sol`
```js
pragma solidity >=0.5.0 <0.6.0;

/**
* @title Ownable
* @dev The Ownable contract has an owner address, and provides basic authorization control
* functions, this simplifies the implementation of "user permissions".
*/
contract Ownable {
  address private _owner;

  event OwnershipTransferred(
    address indexed previousOwner,
    address indexed newOwner
  );

  /**
  * @dev The Ownable constructor sets the original `owner` of the contract to the sender
  * account.
  */
  constructor() internal {
    _owner = msg.sender;
    emit OwnershipTransferred(address(0), _owner);
  }

  /**
  * @return the address of the owner.
  */
  function owner() public view returns(address) {
    return _owner;
  }

  /**
  * @dev Throws if called by any account other than the owner.
  */
  modifier onlyOwner() {
    require(isOwner());
    _;
  }

  /**
  * @return true if `msg.sender` is the owner of the contract.
  */
  function isOwner() public view returns(bool) {
    return msg.sender == _owner;
  }

  /**
  * @dev Allows the current owner to relinquish control of the contract.
  * @notice Renouncing to ownership will leave the contract without an owner.
  * It will not be possible to call the functions with the `onlyOwner`
  * modifier anymore.
  */
  function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(_owner, address(0));
    _owner = address(0);
  }

  /**
  * @dev Allows the current owner to transfer control of the contract to a newOwner.
  * @param newOwner The address to transfer ownership to.
  */
  function transferOwnership(address newOwner) public onlyOwner {
    _transferOwnership(newOwner);
  }

  /**
  * @dev Transfers control of the contract to a newOwner.
  * @param newOwner The address to transfer ownership to.
  */
  function _transferOwnership(address newOwner) internal {
    require(newOwner != address(0));
    emit OwnershipTransferred(_owner, newOwner);
    _owner = newOwner;
  }
}
```
onlyOwner is such a common requirement for contracts that most Solidity DApps start with a copy/paste of this Ownable contract, and then their first contract inherits from it.

Since we want to limit `setKittyContractAddress` to onlyOwner, we're going to do the same for our contract.

`zombieFactory.sol`
```js
pragma solidity >=0.5.0 <0.6.0;

import "./ownable.sol";

contract ZombieFactory is Ownable {

    event NewZombie(uint zombieId, string name, uint dna);

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;
}
```
## Chapter 3
### onlyOwner Function Modifier
Access control—that is, "**who is allowed to do this thing**" —is incredibly important in the world of smart contracts. The access control of your contract may govern who can mint tokens, vote on proposals, freeze transfers, and many other things. 

The most common and basic form of access control is the concept of **ownership**: there’s an account that is the owner of a contract and can do administrative tasks on it. This approach is perfectly reasonable for contracts that have a **single administrative user**.

Ownable also lets you:
- `transferOwnership` from the owner account to a new one, and
- `renounceOwnership` for the owner to relinquish this administrative privilege, a common pattern after an initial stage with centralized administration is over.

A **function modifier** looks just like a function, but uses the keyword `modifier` instead of the keyword `function`.  And it can't be called directly like a function can — instead we can attach the modifier's name at the end of a function definition to change that function's behavior.

```js
pragma solidity >=0.5.0 <0.6.0;

/**
 * @title Ownable
 * @dev The Ownable contract has an owner address, and provides basic authorization control
 * functions, this simplifies the implementation of "user permissions".
 */
contract Ownable {
  address private _owner;

  event OwnershipTransferred(
    address indexed previousOwner,
    address indexed newOwner
  );

  /**
   * @dev The Ownable constructor sets the original `owner` of the contract to the sender
   * account.
   */
  constructor() internal {
    _owner = msg.sender;
    emit OwnershipTransferred(address(0), _owner);
  }

  /**
   * @return the address of the owner.
   */
  function owner() public view returns(address) {
    return _owner;
  }

  /**
   * @dev Throws if called by any account other than the owner.
   */
  modifier onlyOwner() {
    require(isOwner());
    _;
  }

  /**
   * @return true if `msg.sender` is the owner of the contract.
   */
  function isOwner() public view returns(bool) {
    return msg.sender == _owner;
  }

  /**
   * @dev Allows the current owner to relinquish control of the contract.
   * @notice Renouncing to ownership will leave the contract without an owner.
   * It will not be possible to call the functions with the `onlyOwner`
   * modifier anymore.
   */
  function renounceOwnership() public onlyOwner {
    emit OwnershipTransferred(_owner, address(0));
    _owner = address(0);
  }

  /**
   * @dev Allows the current owner to transfer control of the contract to a newOwner.
   * @param newOwner The address to transfer ownership to.
   */
  function transferOwnership(address newOwner) public onlyOwner {
    _transferOwnership(newOwner);
  }

  /**
   * @dev Transfers control of the contract to a newOwner.
   * @param newOwner The address to transfer ownership to.
   */
  function _transferOwnership(address newOwner) internal {
    require(newOwner != address(0));
    emit OwnershipTransferred(_owner, newOwner);
    _owner = newOwner;
  }
}
```
Notice the `onlyOwner` modifier on the `renounceOwnership` function. When you call renounceOwnership, the code inside onlyOwner executes **first**. Then when it hits the `_;` statement in onlyOwner, it goes back and executes the code inside renounceOwnership.So if condition of modifier is satisfied while calling this function, the function is executed and otherwise, an exception is thrown.

So while there are other ways you can use modifiers, one of the most common use-cases is to add a quick require check before a function executes.

In the case of onlyOwner, adding this modifier to a function makes it so only the owner of the contract (you, if you deployed it) can call that function.
## Chapter 4
### Gas — the fuel Ethereum DApps run on
In Solidity, your users have to pay every time they execute a function on your DApp using a currency called `gas`. Users buy gas with `Ether` (the currency on Ethereum), so your users have to spend ETH in order to execute functions on your DApp. 

In Solidity Gas is a `fee` which is required to conduct a transaction on the Ethereum blockchain. Gas prices are specified in `gwei`. Gwei is a denomination of the cryptocurrency Ether. Instead of saying that your gas costs **0.000000001 Ether**, you can say your gas costs `1 gwei`. Gas is used to allocate resources of the **Ethereum virtual machine (EVM)** so that wallet transactions and smart contract transactions can `self-execute`.

In the Ethereum documentation, gas is defined as follows:
**“Gas refers to the unit that measures the amount of computational effort required to execute specific operations on the Ethereum network"**.

**Supply** and **demand** on the block chain determine the price of gas in Solidity. If your gas price is **too low** then your transaction will not execute. Your transaction would be a candidate for the next block. If your gas price is **too high** then you are over paying.

The **Ethereum Virtual Machine** is a shared computer across the Ethereum network, so we have to think about how efficiently we can use the resources like in the old days. But instead of using time-sharing systems, Ethereum uses transaction fees due to the distributed nature of its execution environment. 

How much gas is required to execute a function depends on how complex that function's logic is. Each individual operation has a gas cost based roughly on how much computing resources will be required to perform that operation (e.g. writing to storage is much more expensive than adding two integers). The total gas cost of your function is the sum of the gas costs of all its individual operations.

Because running functions costs real money for your users, code optimization is much more important in Ethereum than in other programming languages. If your code is sloppy, your users are going to have to pay a premium to execute your functions — and this could add up to millions of dollars in unnecessary fees across thousands of users.

Transaction fee’s are calculated using the following formula 
```js
                gas used(units) * gas price 
```

`Gas price` = How much you are willing to pay for 1 gas. Think of gas price as the amount you pay for 1 gallon or liter of gas at the gas station.

-   **Lower** = slower wait time to process in block
-   **Higher** = faster time to process in block

`Gas used` = The first term of the formula is the total cost of the computational operations measured in the unit.

`Gas Limit` = Maximum gas you are willing to use for this transaction. Think of gas limit as the maximum amount of gas your car’s gas tank can hold and you are willing to use for one car trip (with no stops).

There are 2 upper bounds to the amount of gas you can spend
-   **gas limit** (max amount of gas you're willing to use for your transaction, set by you)
-   **block gas limit** (max amount of gas allowed in a block, set by the network)

### Why is gas necessary?

Ethereum is like a big, slow, but extremely secure computer. When you execute a function, every single node on the network needs to run that same function to verify its output — thousands of nodes verifying every function execution is what makes Ethereum decentralized, and its data immutable and censorship-resistant.

The creators of Ethereum wanted to make sure someone couldn't clog up the network with an infinite loop, or hog all the network resources with really intensive computations. So they made it so transactions aren't free, and users have to pay for computation time as well as storage.

### Struct packing to save gas

Normally there's no benefit to using `uint` sub-types because Solidity reserves 256 bits of storage regardless of the `uint` size. For example, using uint8 instead of uint (uint256) won't save you any gas.

But there's an exception to this: inside `structs`.

If you have multiple **uints** inside a `struct`, using a smaller-sized uint when possible will allow Solidity to pack these variables together to take up less storage. For example:
```js
struct NormalStruct {
  uint a;
  uint b;
  uint c;
}

struct MiniMe {
  uint32 a;
  uint32 b;
  uint c;
}

// `mini` will cost less gas than `normal` because of struct packing
NormalStruct normal = NormalStruct(10, 20, 30);
MiniMe mini = MiniMe(10, 20, 30); 
```
You'll also want to **cluster identical** data types together (i.e. put them next to each other in the struct) so that Solidity can minimize the required storage space. For example, a struct with fields `uint c; uint32 a; uint32 b;` will cost less gas than a struct with fields `uint32 a; uint c; uint32 b;` because the uint32 fields are clustered together.

## Chapter 5
### Time units
Solidity provides some native units for expressing **time** with suffixes, such as `seconds, minutes, hours, days, and weeks`.
The variable `now` will return the **current unix timestamp** of the latest block (the number of seconds that have passed since January 1st 1970). The unix time as I write this is 1515527488.

**Note:** Unix time is traditionally stored in a 32-bit number. This will lead to the "Year 2038" problem, when 32-bit unix timestamps will overflow and break a lot of legacy systems. So if we wanted our DApp to keep running 20 years from now, we could use a 64-bit number instead — but our users would have to spend more gas to use our DApp in the meantime. Design decisions!

**These units will convert to a `uint` of the number of seconds in that specific length of time**. For example, 1 minutes is 60, 1 hours is 3600 (60 seconds x 60 minutes), etc.
Seconds are the base unit, and units are considered to correspond to:
-   1 == 1 seconds
-   1 minutes == 60 seconds
-   1 hours == 60 minutes
-   1 days == 24 hours
-   1 weeks == 7 days

**Example**
```js
uint lastUpdated;

// Set `lastUpdated` to `now`
function updateTimestamp() public {
  lastUpdated = now;
}

// Will return `true` if 5 minutes have passed since `updateTimestamp` was 
// called, `false` if 5 minutes have not passed
function fiveMinutesHavePassed() public view returns (bool) {
  return (now >= (lastUpdated + 5 minutes));
}
```
## Chapter 6
### Passing struct as arguments
In solidity the `structs` given as parameters for a function are mostly **given by value**, meaning that the whole struct is practically **duplicated in memory** and handled as a **local copy of the original struct**. There is one exception that you can use if you want to use another semantics: with `internal` and `private` functions you can use the `storage` keyword to make sure that the struct us handled as **reference and not as value**. 

**The syntax looks like this:**
```js
function _doStuff(Zombie storage _zombie) internal {
  // do stuff with _zombie
}
```
**Example**
```js
function _triggerCooldown(Zombie storage _zombie) internal {
    _zombie.readyTime = uint32(now + cooldownTime);
  }

  function _isReady(Zombie storage _zombie) internal view returns (bool) {
      return (_zombie.readyTime <= now);
  }
```
## Chapter 7
### Public functions and security
`Public` functions can be called by anyone (by functions from inside the contract, by functions from inherited contracts or by outside users)

`External` functions can only be accessed externally, which means they cannot be called by other functions of the contract. The gist below does not compile, the external visibility of cannotBeCalled does not allow it to be called by the contract’s functions (however it can be called by another contract).External is cheaper to use because it uses the calldata opcode while public needs to copy all the arguments to memory.

`Private and internal` are simpler: `private` means that the function can only be called from inside the contract, while `internal` proves a more relaxed restriction allowing contracts that inherit from the parent contract to use that function.

That said, keep your functions `private or internal` unless there is a need for outside interaction.

**Example**
Looking back at this function, you can see we made it `public` in the previous lesson. An important security practice is to examine all your `public and external` functions, and try to think of ways users might abuse them. Remember — unless these functions have a modifier like `onlyOwner`, any user can call them and pass them any data they want to.

Re-examining this particular function, the user could call the function directly and pass in any **_targetDna or _species** they want to. This doesn't seem very game-like — we want them to follow our rules!

On closer inspection, this function only needs to be called by **feedOnKitty()**, so the easiest way to prevent these exploits is to make it `internal`.
```js
 // 1. Make this function internal
  function feedAndMultiply(uint _zombieId, uint _targetDna, string memory _species) internal {
    require(msg.sender == zombieToOwner[_zombieId]);
    Zombie storage myZombie = zombies[_zombieId];
    // 2. Add a check for `_isReady` here
    require ( _isReady (myZombie));
    _targetDna = _targetDna % dnaModulus;
    uint newDna = (myZombie.dna + _targetDna) / 2;
    if (keccak256(abi.encodePacked(_species)) == keccak256(abi.encodePacked("kitty"))) {
      newDna = newDna - newDna % 100 + 99;
    }
    _createZombie("NoName", newDna);
    // 3. Call `_triggerCooldown`
    _triggerCooldown(myZombie);
  }
```

### Function modifiers with arguments
Modifiers in Solidity can have arguments of any data type supported by Solidity. Modifiers with arguments are defined in the same way as regular modifiers, with the addition of one or more parameters in the function definition.
```js
mapping (uint => uint) public age;

// Modifier that requires this user to be older than a certain age:
modifier olderThan(uint _age, uint _userId) {
  require(age[_userId] >= _age);
  _;
}

// Must be older than 16 to drive a car (in the US, at least).
// We can call the `olderThan` modifier with arguments like so:
function driveCar(uint _userId) public olderThan(16, _userId) {
  // Some function logic
}
```
You can see here that the olderThan modifier takes arguments just like a function does. And that the driveCar function passes its arguments to the modifier.
#### Example

```js
pragma solidity >=0.5.0 <0.6.0;
import "./zombiefeeding.sol";
contract ZombieHelper is ZombieFeeding {

  modifier aboveLevel(uint _level, uint _zombieId) {
    require(zombies[_zombieId].level >= _level);
    _;
  }
}
```
## Chapter 8
### Saving Gas With 'View' Functions
`view` functions don't cost any gas when they're called **externally** by a user.

This is because `view` functions don't actually change anything on the blockchain – they **only read the data**. So marking a function with `view` tells **web3.js** that it only needs to query your local Ethereum node to run the function, and it doesn't actually have to create a transaction on the blockchain (which would need to be run on every single node, and cost gas).

**Note:** If a `view` function is called **internally** from another function in the same contract that is not a `view` function, it will still **cost gas**. This is because the other function creates a transaction on Ethereum, and will still need to be verified from every node. So view functions are only free when they're called **externally**.
```js
function getZombiesByOwner(address _owner) external view returns(uint[] memory) {
   
  }
```
### Storage is Expensive
One of the more expensive operations in Solidity is using `storage` — particularly **writes**.
This is because every time you write or change a piece of data, it’s written permanently to the blockchain. Forever! Thousands of nodes across the world need to store that data on their hard drives, and this amount of data keeps growing over time as the blockchain grows. So there's a cost to doing that.

In order to keep costs down, you want to avoid writing data to storage except when absolutely necessary. Sometimes this involves seemingly inefficient programming logic — like rebuilding an array in **memory** every time a function is called instead of simply saving that array in a variable for quick lookups.

In most programming languages, looping over large data sets is expensive. But in Solidity, this is way cheaper than using storage if it's in an external view function, since view functions don't cost your users any gas. (And gas costs your users real money!).
#### Declaring arrays in memory

You can use the `memory` keyword with arrays to create a new array inside a function without needing to write anything to **storage**. The array will only exist until the end of the function call, and this is a lot cheaper gas-wise than updating an array in storage — free if it's a `view` function called externally.

Here's how to declare an array in memory:
```js
function getArray() external pure returns(uint[] memory) {
  // Instantiate a new array in memory with a length of 3
  uint[] memory values = new uint[](3);

  // Put some values to it
  values[0] = 1;
  values[1] = 2;
  values[2] = 3;

  return values;
}
```

**Note:** memory arrays must be created with a length argument (in this example, 3). They currently cannot be resized like storage arrays can with `array.push()`, although this may be changed in a future version of Solidity.

## Chapter 9
### Loops
Loops are used when we have to perform an action **over and over again**. While writing a contract there may be a situation when we have to do some action repeatedly, In this situation, loops are implemented to reduce the number of lines of the statements. They make our code **more organized and manageable**.

#### 1. `while` loop

The while loop executes a block of code repeatedly, as long as the specified **condition** is **true**. When the condition becomes false, the loop will terminate.

If the condition is false at the start of the loop, it won’t execute the code.
##### **Syntax**
```js
        while (condition) {
          // body
        }
```
**Example**
```js
pragma solidity ^0.5.0;

contract Example {
  // declaring a state variable
  uint i = 0;

  // creating to function to use while loop
  function whileLoop() public returns(uint) {
    // creating a while loop
    while (i < 5) {
      i++;
    }

    // return i
    return i;
  }
}
```
#### 2. `do-while` loop

The do-while loop is similar to the while loop with the difference that **it checks the condition at the end of the loop**.

Therefore, **it executes a block of code at least once**, even if the condition is false.
#### Syntax
```js
        do {
          // body
        } while (condition);
```
**Example**
```js
pragma solidity ^0.5.0;

contract Example {
  // declaring a state variable
  uint i = 0;

  // creating to function to use do-while loop
  function doWhileLoop() public returns(uint) {
    // creating a do-while loop 
    do {
      i++;
    } while (i < 5);

    // return i
    return i;
  }
}
```
#### 3. `for` loop
The for loop also repeatedly executes a block of code. It accepts the three following arguments separated by a semi-colon (;).

1. **loop initialization:** This initializes the iterator. It is executed only once.
2. **loop condition:** This checks whether or not the condition is true. If the condition is **true**, the body will be **executed**. If the condition is **false**, the loop will be **terminated**.
3. **loop iteration:** This updates the iterator’s value. After updating the value, it will recheck the loop condition.
#### Syntax
```js
        for (initialization; condition; iteration) {
          // body
        }
```

For our `getZombiesByOwner` function, a naive implementation would be to store a mapping of **owners to zombie** armies in the **ZombieFactory** contract:
```js
        mapping (address => uint[]) public ownerToZombies
```
Then every time we create a new zombie, we would simply use `ownerToZombies[owner].push(zombieId)` to add it to that owner's zombies array. And getZombiesByOwner would be a very straightforward function:
```js
     function getZombiesByOwner(address _owner) external view returns (uint[] memory) {
     return ownerToZombies[_owner];
}
```
This approach is tempting for its simplicity. But let's look at what happens if we later add a function to t**ransfer** a zombie from **one owner to another**.

That transfer function would need to:

1. **Push** the zombie to the new owner's ownerToZombies array,
2. **Remove** the zombie from the old owner's ownerToZombies array,
3. **Shift** every zombie in the older owner's array up one place to fill the hole, and then
4. **Reduce** the array length by 1.

**Step 3** would be extremely **expensive gas-wise**, since we'd have to do a write for every zombie whose position we shifted. If an owner has 20 zombies and trades away the first one, we would have to do 19 writes to maintain the order of the array.

Since writing to storage is one of the most expensive operations in Solidity, every call to this transfer function would be extremely expensive gas-wise. And worse, it would cost a different amount of gas each time it's called, depending on how many zombies the user has in their army and the index of the zombie being traded. So the user wouldn't know how much gas to send.

Since `view` functions don't cost gas when called externally, we can simply use a `for-loop` in getZombiesByOwner to iterate the entire zombies array and build an array of the zombies that belong to this specific owner. Then our `transfer` function will be much cheaper, since we don't need to reorder any arrays in storage, and somewhat counter-intuitively this approach is cheaper overall.

**Example**
Let's look at an example where we want to make an array of even numbers:
```js
function getEvens() pure external returns(uint[] memory) {
  uint[] memory evens = new uint[](5);
  // Keep track of the index in the new array:
  uint counter = 0;
  // Iterate 1 through 10 with a for loop:
  for (uint i = 1; i <= 10; i++) {
    // If `i` is even...
    if (i % 2 == 0) {
      // Add it to our array
      evens[counter] = i;
      // Increment counter to the next empty index in `evens`:
      counter++;
    }
  }
  return evens;
}
```
This function will return an array with the contents [2, 4, 6, 8, 10].