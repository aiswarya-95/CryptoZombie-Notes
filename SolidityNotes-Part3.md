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