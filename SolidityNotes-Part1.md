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
```sh
8356281049284737
```
## Chapter 2
### Version Pragma
Declaration of the version of the Solidity compiler and this will be the starting point of all projects. This is to prevent issues with future compiler versions potentially introducing changes that would break your code.

Want to be able to compile smart contracts with any compiler version in the range of 0.5.0 (inclusive) to 0.6.0 (exclusive). It looks like this: 

```sh
pragma solidity >=0.5.0 <0.6.0;
```
### Contracts
Solidity's code is encapsulated in contracts. All variables and functions belong to a contract.
```sh
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
```sh
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

```sh
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {
    uint dnaDigits = 16;
    uint dnaModulus = 10 ** dnaDigits; #equal to 10^16
}
```
## Chapter 5
### Structs
Structs allows to create more complicated data types that have multiple properties.
```sh
pragma solidity >=0.5.0 <0.6.0;

contract ZombieFactory {
    struct Zombie {
        string name;
        uint dna;
    }
```


