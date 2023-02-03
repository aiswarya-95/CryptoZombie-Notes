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








