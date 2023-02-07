# CryptoZombies
To learn BLOCKCHAIN programming on Ethereum Solidity by building DApps(fun games).
### Ethereum
**Ethereum** is a decentralized open-source global software blockchain platform that use to build public blockchain applications. 
### Ether
**Ether** is the transactional token that promote operations on the Ethereum network.   While ether is the cryptocurrency of the Ethereum network. It is alseo refer to as the **"fuel"** of the network.
Ether is the world’s second-largest virtual currency (second only to Bitcoin (BTC)) according to market value.
### EVM
The **Ethereum Virtual Machine** is ta piece of software that executes smart contracts and computes the state of the Ethereum network after each new block is added to the chain.  Any participant can request the execution of arbitrary code on the EVM; code execution changes the state of the EVM.
### Nodes
Every computer in the network which are storing the EVM state. Nodes communicate with each other to transmit information about the EVM state and new state changes. It ensure everyone interacting with the blockchain has the same data. To accomplish this blockchains need a **consensus mechanism**(protocols that determine the validity of transactions and control of the blockchain).

### Blockchain
**A Blockchain** technology is a public database that is updated and allows transparent information sharing within a business network . A blockchain database stores data in **"BLOCKS"** that are linked together in a **"CHAIN"**. The data is chronologically consistent (can't delete or modify the chain without consensus from the network). blockchain technology use to create an unalterable or immutable ledger for tracking orders, payments, accounts, and other transactions. 
### key components of blockchain technology
#### A distributed ledger 
**A distributed ledger** is the shared database in the blockchain network that stores the shared file that everyone in the team can edit.Distributed ledger technologies have strict rules about who can edit and how to edit. It doesn't alloow to delete entries once they have been recorded.
#### Smart contracts
**Smart contracts** are self-executed programs stored on a blockchain that run when predetermined conditions are met.A reusable piece of code which a developer publishes into **EVM state**. Developers can create arbitrary executable applications into the EVM by publishing smart contracts, these are called as **dapps**, or **Decentralized Apps**.
#### Public key cryptography
Public key (asymmetric-key) cryptography is used to uniquely identify users in the blockchain network. It generates two sets of keys for network members. One key is a **public key** that is common to everyone in the network and used to **encrypt** the plaintext to cypertext. The other is a **private key** that is unique to every member and used to **decrypt** the cypertext to plaintext. This keys work together to unlock the data in the ledger.
## Chapter 1
##### Build a "Zombie Factory" 

- it will maintain a database of all zombies 
- Have a function for creating new zombies
- Each zombie will have a random and unique appearance

### Zombie DNA
 it's a 16-digit integer, like:
```js
8356281049284737
```
## Chapter 2
### Version Pragma
Declaration of the version of the Solidity compiler and this will be the starting point of all projects. This is to prevent issues with future compiler versions potentially introducing changes that would break your code.

Want to be able to compile smart contracts with any compiler version in the range of 0.5.0 (inclusive) to 0.6.0 (exclusive). It looks like this: 

```js
pragma solidity >=0.5.0 <0.6.0;
```
### Contracts
Solidity's code is encapsulated in contracts. All variables and functions belong to a contract.
```js
pragma solidity >=0.5.0 <0.6.0;
contract ZombieFactory {
}
```
## Chapter 3
### Variables
- `uint` data type is an unsigned integer, its value must be non-negative.
- `int` data type is a signed integers.
- `Boolean` data type returns ‘1' when the condition is true and ‘0' when it is false.
- `String` Single or double quotes can denote a string.

### State Variables
State variables are permanently stored in contract storage. This means it written to the Ethereum blockchain.
```js
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {
    uint dnaDigits = 16;
}
```
## Chapter 4
### Math Operations
- Addition: `x + y`
- Subtraction: `x - y`
- Multiplication: `x * y`
- Division: `x / y`
- Modulus / remainder: `x % y`
- exponential operator: `x ** y`

```js
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {
    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits; #equal to 10^16
}
```
## Chapter 5
### Structs
Structs allows to create more complicated data types that have multiple properties.
```js
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {
    struct Zombie {
        string name;
        uint dna;
    }
```
## Chapter 6
### Arrays
Array is a data structure, which stores a fixed-size sequential collection of elements of the same data type.Arrays hold contiguous memory locations.A specific element in an array is accessed by an index. 
In Solidity,
- **Fixed Array** - Array with a fixed length
```js
        uint[2] fixedArray;
```
- **Dynamic Array** - has no fixed size, can keep growing
```js
        uint[] dynamicArray;
```
#### Struct Array
we can also create an array of **structs**. reating a dynamic array of structs can be useful for storing structured data in contract, kind of like a database.
```js
    Zombie[] people; //dynamic array with struct zombie
```
#### Public Array
It can be declare an array as **public**, and Solidity will automatically create a **getter** method for it. The syntax looks like:
```js
    Zombie[] public people;
```
Other contracts would then be able to read from, but not write to, this array. So this is a useful pattern for storing public data in your contract.
```js
    pragma solidity >=0.5.0 <0.6.0;
    contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies; // Dynamic public Array using struct

}
```
#### Array Functions
- **Accessing Array Elements** - The elements of the array are accessed by using the index. If you want to access ith element then you have to access (i-1)th index.
- **length** − length returns the size of the array. length can be used to change the size of dynamic array be setting it.
- **push** − push allows to append an element to a dynamic storage array at the end. It returns the new length of the array.
-  **Pop** - Pop is used when the last element of the array is to be removed in any dynamic array.

#### Example Working With Structs and Arrays
To create new **Person** and add them to **people** array.
```js
struct Person {  
  uint age;
  string name;
}

Person[] public people; //dynamic array
people.push(Person(16, "Vitalik")); // push array value
```

## Chapter 7
### Function Declarations
A function is a group of reusable code which can be called anywhere in the program. This eliminates the need of writing the same code again and again. It helps programmers writing modular codes. Functions allow a programmer to divide a big program into a number of small and manageable functions.
A function declaration in solidity looks like the following:
```js
    function eatHamburgers(string memory _name, uint _amount) public {
    }
```
This is a function named **eatHamburgers** that takes
##### Function Parameters
Functions can be without parameters. But there is a facility to pass different parameters while calling a function. These passed parameters can be captured inside the function and any manipulation can be done over those parameters. A function can take multiple parameters separated by comma.
-   In this example there are 2 parameters: a **string** and a **uint**. 
##### The return Statement
A Solidity function can have an optional return statement. This is required if you want to return a value from a function. This statement should be the last statement in a function. To return values from the function, the data types of the return values should be defined at function declaration.

For now the body of the function is empty. The function visibility as **public**. Also providing instructions about where the **_name** variable should be stored in memory. This is required for all reference types such as arrays, structs, mappings, and strings.

#### Passing an Argument in Two Ways
- **By value**, which means that the Solidity compiler creates a new copy of the parameter's value and passes it to your function. This allows your function to modify the value without worrying that the value of the initial parameter gets changed.
- **By reference**, which means that your function is called with a... reference to the original variable. Thus, if your function changes the value of the variable it receives, the value of the original variable gets changed.

##### Example
```js
pragma solidity >=0.5.0 <0.6.0;
contract ZombieFactory {

    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits;

    struct Zombie {
        string name;
        uint dna;
    }

    Zombie[] public zombies; // Array Declaration

    function createZombie (string memory _name, uint _dna) public {
        // function declaration
    }
}
```
## Chapter 8
### Private / Public Functions
In Solidity, functions are **public** by default. This means anyone (or any other contract) can call your contract's function and execute its code.Obviously this isn't always desirable, and can make your contract vulnerable to attacks.

Thus it's good practice to mark your functions as **private** by default, and then only make public the functions you want to expose to the world.
Let's look at how to declare a **private** function:
```js
uint[] numbers;

function _addToArray(uint _number) private {
  numbers.push(_number);
}
```
This means only other functions within our contract will be able to call this function and add to the numbers array. we use the keyword **private** after the function name. And as with function parameters, it's **convention** to start **private function names** with an underscore (_).

### Function modifiers
Function Modifiers are used to modify the behaviour of a function.
- **View** functions ensure that they will not modify the state. A function can be declared as view meaning it's only viewing the data but not modifying it. **Getter** method are by default **view** functions.
```js
    function sayHello() public view returns (string memory) {
    }
```
- **Pure functions** ensure that they not read or modify the state. A function can be declared as pure. Pure functions can use the revert() and require() functions to revert potential state changes if an error occurs.
```js
    function _multiply(uint a, uint b) private pure returns (uint) {
     return a * b;
    }
```
This function doesn't even read from the state of the app — its return value depends only on its function parameters. So in this case we would declare the function as **pure**.
## Chapter 9
### Cryptographic Hash Functions
A cryptographic hash function is an algorithm that takes an any amount of data as input and produces the enciphered text of fixed size.The input can be a variable length string or number, but the result will always be a fixed **bytes32** data type. This consists of 64 characters (letters and numbers) that can be expressed as hexadecimal numbers.Even a slight change in the input gives a completely different output.

- **keccak256(bytes memory) returns (bytes32)** - Computes the Keccak-256 hash of the input
- **sha256(bytes memory) returns (bytes32)** - Computes the SHA-256 hash of the input
- **ripemd160(bytes memory) returns (bytes20)** - Compute RIPEMD-160 hash of the input
- **sha256(bytes memory) returns (bytes32)** -	Computes the SHA-256 hash of the input
- **ecrecover(bytes32 hash, uint8 v, bytes32 r, bytes32 s) returns (address)** -    
Recover the address associated with the public key from.
Elliptic curve signature used for cryptography or return Zero if an error occurs. 
The parameters correspond to ECDSA Signature values.

keccak256 expects a single parameter of type bytes. This means that we have to **"pack"** any parameters before calling keccak256:

Example:
```js
    //6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
    keccak256(abi.encodePacked("aaaab"));
    //b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
    keccak256(abi.encodePacked("aaaac"));
```

The returned values are totally different despite **only a 1 character** change in the input.
Hashing is an important aspect of cryptographic security for digital wallets and transactions on the blockchain. The Ethereum protocol uses keccak256 in its network with a consensus engine called **Ethash**. It plays an important role in producing blocks, and securing them on the blockchain.
#### ABI (Application Binary Interface)
An **ABI** (Application Binary Interface) interface between two software modules, most commonly between user programs and operating systems. Solidity is a high-level programming language based on this and is used exclusively by developers to create understandable smart contracts. Solidity has a global variable called **abi** that has an encode method, so we can use it to encode the parameters of any function.

The keyword **abi.encodePacked** is used with statements for encoding data input. It is used with the following conditions:
- Types shorter than **32 bytes** are concatenated directly, without **padding** or **sign extension**
- Dynamic types are encoded in-place and without the length
- Array elements are padded, but still encoded in-place

```js
    abi.encodePacked( <data input> )
```

This means that dynamic types are encoded in-place without length while static types will not be padded if they are shorter than 32 bytes.

Encoding was meant to give the programmer more control of how the data should be encoded. Before it was the compiler that performed this function, but it can be problematic. The reason is that it can cause what are called **collisions**. The **abi.encode** function can be useful when it comes to preventing collisions in hash functions. A collision can occur when two different inputs produce the same output. That may sound impossible, but it can happen in the least expected manner. 
Take for **example** the following inputs:
```js
    (AAA, BBB) -> AAABBB         
    (AA, ABBB) -> AAABBB
```
They are supposed to be different from each other, but when concatenated as a single string, they actually will produce the same output. It is recommended to use **abi.encode** instead of **abi.encodePacked**. 

### Typecasting
Solidity is a statically typed language, so all variables have a fixed type. it is not possible to change the type of the variable after its declaration. This is necessary because state variables have a dedicated space for them in storage.

It is possible, however, to convert one type to another. This is called type casting. There are two ways to do this: 
- Explicit conversions
- Implicit conversions

#### Implicit Conversions
Implicit conversions occur when the expression makes logical sense and there is no loss of information when casting. 
```js
    uint8 valor1 = 140;
    uint16 valor2 = 480;
    uint16 valor3 = valor1 + valor2; // 620
```
The variables **value1** and **value2** be of different types, the sum will be carried out without problem. There is no loss of information.
#### Explicit conversions
Sometimes you need to convert between data types explicitly.
**Example:**
```js
        uint8 a = 5;
        uint b = 6;
        // throws an error because a * b returns a uint, not uint8:
        uint8 c = a * b;
        // we have to typecast b as a uint8 to make it work:
        uint8 c = a * uint8(b);
```
In the above, **a * b** returns a **uint**, but we were trying to store it as a **uint8**, which could cause potential problems. By casting it as a **uint8**, it works and the compiler won't throw an error.
#### Unicode
Unicode is a standard for representing symbols like **text** and **emojis**. Each symbol requires between 1 and 4 bytes to be represented. Common letters use only 1 byte, while emojis use 4 bytes.
**Example**
converts 2 common letters, a whitespace and an emoji to the type bytes.
```js
        function showUnicode() public pure returns(bytes memory){
            String memory text =unicode"Hi 😀";
            returns bytes(text);
        }
```
the output will be,
```js
        0x486920f09f9880
```

It is a 14-digit hexadecimal number, which means 7 bytes: 2 bytes for the common letters, 1 byte for the blank space and 4 bytes for the emoji.
## Chapter 10
### Events
Solidity Event is an inheritable member of the contract, which stores the arguments passed in the transaction logs when emitted. Generally, events are used to inform the calling application about the current state of the contract, with the help of the logging facility of EVM. Events notify the applications about the change made to the contracts and applications which can be used to execute the dependent logic.

**Events** are a way for your contract to communicate that something happened on the blockchain to your app front-end, which can be 'listening' for certain events and take action when they happen.
**Example:**
```js
// declare the event
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public returns (uint) {
  uint result = _x + _y;
  // fire an event to let the app know the function was called:
  emit IntegersAdded(_x, _y, result);
  return result;
  }
```
app front-end could then listen for the event. A JavaScript implementation would look something like:
```js
YourContract.IntegersAdded(function(error, result) {
  // do something with result
})
```
## Chapter 11
#### Web3
A software program called a web service enables **computer-to-computer** communication over the internet. However, web services are nothing new and typically take the form of an **application programming interface (API)**. The Web is a collection of related hypertext materials that may be accessed online. For example, a user examines web pages that may contain multimedia using a web browser and uses hyperlinks to move between them.

Web3 browsers help users interact with **decentralized applications** (Dapp) built on **blockchain technology**. Web3 technologies like distributed ledgers, artificial intelligence, Metaverse and others aim to create the next-generation internet, which is accessible to everyone and offers benefits.

Key features of a Web3 browser include:

- Immutable ecosystem, i.e., trust that people will download the digital product just as the original creator intended.
- Enhanced transparency and security,
- Quicker browsing performance,
- Complete user anonymity and confidentiality,
- Integrating cryptocurrency wallets with multiple blockchains,
- Complete control over the content due to decentralization.


#### Web3.js
web3.js is a collection of **libraries** which allow you to interact with a local or remote ethereum node, using a **HTTP**, **WebSocket** or **IPC** connection. The web3 JavaScript library interacts with the Ethereum blockchain. Using this library, you can develop websites or clients that interact with the blockchain. It can retrieve user accounts, send transactions, interact with smart contracts, and more.
#### Web3.js API Type

- **eth**: Etherum blockchain related methods
- **net**: Node’s network status
- **personal**: Account functions and sending
- **db**: Get/put for local LevelDB
- **shh**: P2P messaging using Whisper



