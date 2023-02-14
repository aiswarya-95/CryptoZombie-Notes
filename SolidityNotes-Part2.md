## Chapter1
### Address Type
An address value type is specifically designed to hold up to **20B, or 160 bits**, which is the size of an Ethereum address. **Hash functions** are the key element to create addresses. Ethereum uses the keccak-256 hash function to generate them.

The Ethereum blockchain is made up of accounts, which you can think of like bank accounts. An account has a balance of Ether (the currency used on the Ethereum blockchain), and you can send and receive Ether payments to other accounts, just like your bank account can wire transfer money to other bank accounts. Each account has an address, which you can think of like a bank account number. It's a unique identifier that points to that account.
the **main features** behind an address type in Solidity are:

- **20 bytes long (160 bits)** : as said, an ethereum address corresponds to the last 20 bytes of the **Keccak-256 hash** of its associated **public key**.
- **Hexadecimal format (base 16 notation)** : an Ethereum address contains **40 characters** exactly (2 characters = 1 byte) from the **hexadecimal range** (0 1 2 3 4 5 6 7 8 9 or a b c d e f).
- **always pre-fixed with 0x** : because it is hexadecimal format, it has to be prefixed with **0x**. Therefore, it’s total length is actually 42 characters long if we count the 0x.

To **define** a variable of address type, specify the keyword address in front of the variable name.
```js
        address user = msg.sender
```
here the Solidity **built-in** function **msg.sender** to retrieve the address of the current account interacting with the smart contract.

It can also hardcode specific addresses in your Solidity code, using address **literals*.
Address literals are the hexadecimal representation of an Ethereum address, hardcoded in a solidity file.
```js
        address owner = 0xc0ffee254729296a45a3885639AC7E10F9d54979
```

Solidity actually offers two address value types: 
- address 
- address payable. 

The difference between the two is that **address payable** can **send** and **transfer** Ether.
### Methods
The solidity methods associated with addresses divided into 2 categories:

- **methods related to ethers**: `.balance()`, `.transfer()` and `.send()`
- **methods related to contract interactions**: `.call()` , `.delegatecall()` and `.staticcall()`

##### - `address.balance` - Returns the account balance in Wei.
Any variable defined as an address has the method .balance() available. This enable to retrieve the amount of Ether hold by an Externally Owned Account (EOA / User) or a Contract Account. The number returned represents an Ether amount in Wei.
```js
        uint my_ether_balance = msg.sender.balance;
```
##### - `address.transfer()`
- Transfer the amount of ether (in wei) to the specified account.
- Reverts on failure and throw an exception on any error.
- Forwards 2,300 gas stipend, not adjustable.
- should be used in most cases as it's the safest way to send ether

Under the hood, the `.transfer()` function queries the balance of an address, by applying the property balance, before to send the ether.

##### - `address.send()`
- Similar to address.transfer()
- Returns false on failure (Warning : always check the return value of send !)
- Forwards 2,300 gas stipend, not adjustable.
- should be used in rare cases when you want to handle failure in the contract

##### - `address.call(bytes memory) returns (bool, bytes memory)`
- Creates an arbitrary message call (low-level CALL , see Opcode OxF1) given the memory (data payload) passed as an argument.
- Forward all available gas, adjustable.
- Returns a tuple with :
     1. a boolean for the call result (true on success, false on failure or error).
     2. The data in bytes format.
     
(ex:I am contract A and I want to execute contract’s B function for contract’s B storage. Calling B.function() can only update B’s storage.)
##### - `address.callcode(__payload__)`
- Low-level CALLCODE function, like address(this).call(…) but with this contract’s code replaced with that of address.
- Returns false on error.

(ex:I am contract A and I want to execute contract’s B function for my storage. Calling B.function() will update A’s storage.) 
##### ` _address.delegatecall(bytes memory) returns (bool, bytes memory)`
- Low-level DELEGATECALL , see Opcode OxF4) (with the full msg context seen by the current contract) given the memory (data payload) passed as an argument.
- Forward all available gas, adjustable
- Returns a tuple with :
    1. a boolean for the delegatecall result (true on success, false on failure or error).
    2. The data in bytes format.

(ex: I am contract A and I want to execute contract’s B function, but contract B can pretend to be me. B’s function can overwrite A’s storage and pretend to be A to any other contract. msg.sender will be A’s address, not B.If contract B call another contract C, contract C will see that msg.sender is contract A.)
 
 ##### ` _address.staticcall(bytes memory) returns (bool, bytes memory)`   
 - Low-level STATICCALL , see Opcode OxF4) (with the full msg context seen by the current contract) given the memory (data payload) passed as an argument.
 - Returns a tuple with :
    1. a boolean for the staticcall result (true on success, false on failure or error).
    2. The data in bytes format.
-   Forward all available gas, adjustable

### Mappings
Mappings are another way of storing organized data in Solidity. It is the most frequently used reference type in Solidity. Mapping in Solidity acts like a hash table or dictionary.  These are used to store the data in the form of key-value pairs, a key can be any of the built-in data types but reference types are not allowed while the value can be of any type. The mapping function can retrieve stored data using the supplied key.Because Solidity mapping only provides an option for one data storage location, this data type is permitted for state elements.

The mapping type is declared using the `mapping` keyword followed by data types for both key and value, separated by the `=>` notation.

Defining a mapping looks like this:
```js
        // For a financial app, storing a uint that holds the user's account balance:
        mapping (address => uint) public accountBalance;
        // Or could be used to store / lookup usernames based on userId
        mapping (uint => string) userIdToName;
```
the key is an **address** and the value is a **uint**, and in the second example the key is a **uint** and the value a **string**.

## Chapter 2
### msg.sender()
In Solidity, there are certain **global variables** that are available to all functions. The **msg.sender** is the address that has called or initiated a function or created a **transaction**.It refers to the address of the person (or smart contract) who called the current function. Now, this address could be of a contract or even a person like you and me.

To identify the initiator of a transaction or current call, we can make use of the globally available msg object. This msg object has a couple of properties tide to it. For example, we have a value, a sender, gas, and a data property on it.

In Solidity, function execution always needs to start with an external caller. A contract will just sit on the blockchain doing nothing until someone calls one of its functions. So there will always be a `msg.sender`.
Here's an example of using msg.sender and updating a mapping:
```js
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
  // Update our `favoriteNumber` mapping to store `_myNumber` under `msg.sender`
  favoriteNumber[msg.sender] = _myNumber;
  // ^ The syntax for storing data in a mapping is just like with arrays
}

function whatIsMyNumber() public view returns (uint) {
  // Retrieve the value stored in the sender's address
  // Will be `0` if the sender hasn't called `setMyNumber` yet
  return favoriteNumber[msg.sender];
}
```
In this trivial example, anyone could call `setMyNumber` and store a `uint` in our contract, which would be tied to their address. Then when they called `whatIsMyNumber`, they would be returned the`uint` that they stored.

Using `msg.sender` gives you the security of the Ethereum blockchain — the only way someone can modify someone else's data would be to steal the private key associated with their Ethereum address.
**Example**
```js
pragma solidity >=0.4.22 <0.7.0;
contract Sender {
    address private owner;
    
    constructor() public {
        owner = msg.sender;
    }
    
    function updateOwner(address newOwner) public {
        require(msg.sender == owner, "only current owner can update owner");
        owner = newOwner;
    }
    
    function getOwner() public view returns (address) {
        return owner;
    }
}
```
## Chapter 3
### Require()
The **require()** basically means to demand something before availing the service to the users. For example, websites require you to create an account or login into an existing account before giving you access to the account which belongs to you. This is the basic flow of how `require` works in Solidity also. 
`require` returns **two** boolean values that are either **true** or **false**, if the specified condition returns a true value it allows the code to flow and function accordingly. If the value returned is false, it throws an error and stops the code right there. `require` takes two parameters, first is the condition that you want to check and the second is an optional error message you want to show to the user when and if the value returned is false.
**Example** Solidity doesn't have native string comparison, so we compare their keccak256 hashes to see if the strings are equal
```js
function sayHiToVitalik(string memory _name) public returns (string memory) {
  // Compares if _name equals "Vitalik". Throws an error and exits if not true.
  require(keccak256(abi.encodePacked(_name)) == keccak256(abi.encodePacked("Vitalik")));
  // If it's true, proceed with the function:
  return "Hi!";
}
```
If you call this function with sayHiToVitalik("Vitalik"), it will return "Hi!". If you call it with any other input, it will throw an error and not execute.

Thus require is quite useful for verifying certain conditions that must be true before running a function.

## Chapter 4
### Inheritance
Rather than making one extremely long contract, sometimes it makes sense to split your code logic across multiple contracts to organize the code.One feature of Solidity that makes this more manageable is contract inheritance.It is a way of extending the functionality of a program, used to separate the code, reduces the dependency, and increases the re-usability of the existing code.
Solidity supports inheritance between smart contracts, where multiple contracts can be inherited into a single contract. Simply, they are referred to as parent-child contracts. The scope of inheritance in Solidity is limited to public and internal modifiers only.
To create a derived (or inheriting) contract, simply use the is keyword, as demonstrated in the example code below:
```js
// A is a derived contract of B
contract A is B{

}
```
#### Function Overriding
Solidity lets developers change how a function in the parent contract is implemented in the derived class. This is known as function overriding.

The function in the parent contract needs to be declared with the keyword **virtual** to indicate that it can be overridden in the deriving contract.In addition, the overriding function needs to have the keyword **override**. It is possible that you may want your overriding function to be overridden by another function. This is acceptable and can be achieved by using the virtual keyword as before. Here is an example demonstrating function overriding in Solidity:
```js
contract A {
function play(int x, int y) public virtual {

}
}

contract B  is A {
function play(int a, int b) public override {

}
}
```
In the derived contract above, the overriding function needs to have the same function name and the same number of arguments and types as in the parent class.
#### Modifier Overriding

Modifiers are properties that are used to change the behavior of functions. Just like functions, modifiers can be overridden in the derived contract only if they were marked with the keyword virtual in the parent class.

Similarly, you have to use the override keyword while overriding a modifier in the derived class. Check out the sample implementation below, which demonstrates modifier overriding in Solidity:
```js
contract A{
modifier X virtual {
}
}

contract B is A{
modifier X override {
}
}
```
#### Types of inheritance
**1. Single Inheritance** - In Single or single level inheritance the functions and variables of one base contract are inherited to only one derived contract.
**2. Multi-level Inheritance** - The child contract derived from a parent also acts as a parent for the contract which is derived from it.
**3. Hierarchical Inheritance** - In Hierarchical inheritance, a parent contract has more than one child contracts. It is mostly used when a common functionality is to be used in different places.
**4. Multiple Inheritance** - In Multiple Inheritance, a single contract can be inherited from many contracts. A parent contract can have more than one child while a child contract can have more than one parent.

## Chapter 5
### Imports
#### Virtual Filesystem

The compiler maintains an internal database (virtual filesystem or VFS for short) where each source unit is assigned a unique source unit name which is an opaque and unstructured identifier. When you use the import statement, you specify an import path that references a source unit name.
#### Import Callback
The VFS is initially populated only with files the compiler has received as input. Additional files can be loaded during compilation using an **import callback**.If the compiler does not find any source unit name matching the import path in the VFS, it invokes the callback, which is responsible for obtaining the source code to be placed under that name. The command-line compiler provides the **Host Filesystem Loader** - a rudimentary callback that interprets a source unit name as a path in the local filesystem.

When you have multiple files and you want to import one file into another, Solidity uses the import keyword.The import statement specifies an import path. Based on how the import path is specified, we can divide imports into two categories:
- `Direct imports`, where you specify the full source unit name directly.
- `Relative imports`, where you specify a path starting with ./ or ../ to be combined with the source unit name of the importing file.

```js
    import "./math/math.sol";
    import "contracts/tokens/token.sol";
```

In the above `./math/math.sol` and `contracts/tokens/token.sol` are import paths while the source unit names they translate to are `contracts/math/math.sol` and `contracts/tokens/token.sol` respectively.

#### Direct Imports

An import that does **not** start with `./ or ../` is a direct import.
```js
import "/project/lib/util.sol";         // source unit name: /project/lib/util.sol
import "lib/util.sol";                  // source unit name: lib/util.sol
import "@openzeppelin/address.sol";     // source unit name: @openzeppelin/address.sol
import "https://example.com/token.sol"; // source unit name: https://example.com/token.sol
```

After applying any **import remappings** the import path simply becomes the source unit name.(**Import remapping** allows you to redirect imports to a different location in the virtual filesystem. The mechanism works by changing the translation between import paths and source unit names.)

#### Relative Imports

An import starting with `./ or ../` is a relative import. Such imports specify a path relative to the source unit name of the importing source unit:

```js
import "./util.sol" as util;    // source unit name: /project/lib/util.sol
import "../token.sol" as token; // source unit name: /project/token.sol
```

**Note** - Relative imports always start with `./ or ../` so import `"util.sol"`, unlike import `"./util.sol"`, is a direct import. While both paths would be considered relative in the host filesystem, `util.sol` is actually absolute in the `VFS`.

## Chapter 6
### Data location

Each variable declared and used in a contract has a data location. It specifies where the value of that variable should be stored. Every transaction on Ethereum Virtual Machine costs us some amount of Gas. The lower the Gas consumption the better is your Solidity code. The Gas consumption of Memory is not very significant as compared to the gas consumption of Storage. Therefore, it is always better to use Memory for intermediate calculations and store the final result in Storage.
1.  State variables and Local Variables of structs, array are always stored in storage by default.
2.  Function arguments are in memory.
3. Whenever a new instance of an array is created using the keyword ‘memory’, a new copy of that variable is created. Changing the array value of the new instance does not affect the original array.

Solidity provides four types of data locations.

- Storage
- Memory
- Calldata

   
### Storage
The storage location is permanent data, which means that this data can be accessed into all functions within the contract. It as the hard disk data of your computer where all the data gets stored permanently. Similarly, the storage variable is stored in the state of a smart contract and is persistent between function calls. It holds data persistently and consumes more gas.
#### Example
```js
contract CodiesAlert {
    string twitterAccount = "Anni_Maan";

    function displayAccount() public view returns(string memory) {
        string storage Newvar;
        return twitterAccount;
    }
}
```
### Memory

The memory location is temporary data and cheaper than the storage location.Memory is reserved for variables that are defined within the scope of a function. It can only be accessible within the function. Usually, Memory data is used to save temporary variables for calculation during function execution. Once the function gets executed, its contents are discarded. You can think of it as a RAM of each individual function, you will have access to unsaved data as long as the laptop is running but once you shut down your laptop your data is cleared from RAM if it is not saved.
#### Example
```js
contract CodiesAlert {
    string twitterAccount = "Anni_Maan";

    function displayAccount(string memory _accountName) public pure returns(string memory) {
        return _accountName;
    }
```
### Calldata

Calldata is non-modifiable and non-persistent data location where all the passing values to the function are stored. Also, Calldata is the default location of parameters (not return parameters) of external functions.

The main difference is calldata storage is non-modifiable storage which means once function parameters are passed you cannot modify them inside the function.The second big difference is it is cheaper than memory and is mostly used with external function types. Mostly you would see calldata used with external functions.
#### Example
```js
contract CodiesAlert {
    string name = "Anni";

    // Access externally Marked function inside the contract
    function accessExternal() public view returns(string memory) {
        // this keyword is used to access externally marked function inside the contract
        return this.result(name);
    }

    // Below function can be access externally as well as internally
    function result(string calldata _a) external pure returns(string calldata) {
        return _a;
    }
}
```
There are some rules available in datalocation:
- **State variables are always stored in the storage.**
    ```js
            contract DataLocation {
                //storage
            uint stateVariable;
            uint[] stateArray;
            }
    ```
- **Also, you can not explicitly override the location of state variables.**
    ```js
            contract DataLocation{
                 uint storage stateVariable; // error  
                 uint[] memory stateArray; // error  
            }
    ```
- **Function parameters including return parameters are stored in the memory.**
    ```js
        contract Datalocation{
        uint stateVariable;
        uint[] stateArray;
        function calc(uint num1. uint num2) public pure returns(uint result){
            return num1+num2;
        }
        }
    ```
- **Local variables with a value type are stored in the memory. However, for a reference type, you need to specify the data location explicitly.Also Local variables with value types cannot be overriden explicitly.**
    ```js
    contract Locations {
        function doSomething() public  {
        /* these all are local variables  */
        //value types  
        //so they are stored in the memory  
            bool flag2;  
            uint number2;  
            address account2;
        
        //reference type  
            uint[] memory localArray;        
        }
    }   
    ```
-   **Function parameters (not including returns parameters) of external function are stored in the Calldata.**
-   **Assignment of one state variable to another state variable creates a new copy.**
-   **Assignment to storage state variable from a memory variable always creates a new copy.**

    ```js
    contract Locations {  
        uint public stateVar1 = 10;  
        uint stateVar2 = 20;  
        
        function doSomething() public returns (uint) {  
        stateVar1 = stateVar2;  
        stateVar2 = 30;  
        return stateVar1; //returns 20
        }
    }
    ```
-   **Assignment to a memory variable from state storage variable will create a copy.**
    ```js
        contract Locations {
            uint stateVar = 10; //storage
            function doSomething() public returns(uint) {
                uint localVar = 20; //memory    
                localVar = stateVar;  
                stateVar = 40;  
        return localVar; //returns 10    
             }     
        } 
    ```
## Chapter 7
#### Function visibility
In Solidity, you can control who has access to the functions and state variables in your contract and how they interact with them. This concept is known as **visibility**. It is also used to ensure that when functions are specified, their level of accessibility, including 
-   public
-   external
-   internal
-   private

**If the visibility modifier of a function is not explicitly declared in the code, the function is set to `public` visibility by `default`**.If a developer doesn’t use the right function visibility type, or if no visibility model is specified in their code, the contract's default public visibility is exposed to potentially exploitable security vulnerabilities. Apart from the security implications of not having function visibility specified, smart contracts might not work as intended since the functions will be working without proper instructions.

The relationship between function **visibility** modifiers and **inheritance** is explained below:

- If the modifier of the ParentContract function is `public`, a ChildContract can access it.
- If the modifier of the ParentContract function is `internal`, the ChildContract can access it.
- If the modifier of the ParentContract function is `private`, the inheriting ChildContract cannot access it.
- If the modifier of the ParentContract function is `external`, the inheriting ChildContract cannot access it.

#### 1. **Public**

A public function can be accessed by any of the three types of calling contracts: `main contract, derived contract, and a third party contract`. A function is public by default.
```js
contract RandomContract {
      function functionPublic() public {
            //performs a task
              }
}
```
As shown in the code sample above, any contract in the codebase can access the “functionPublic” function because of the` “public”` visibility.
#### 2. **External**

**An external function is a function that can only be called by a `third party`.** With the external function visibility, a contract that can call the function must be independent of the main contract and can not be a derived contract.
```js
contract RandomContract {
      function externalFunction() external {
            //performs a task
      }
}
```
Above is a random contract (RandomContract) that is performing some task. The code snippet below is an external contract that is calling the external function on the RandomContract.
```js
interface IRandomContract {
	function externalFunction() external;
}
contract CallingContract {
        	function functionIsExternalCall(address addr) external {
	IRandomContract(addr).externalFunction();
          }
}
```
#### 3. **Internal**

**An internal function can be called by the main contract and any of its derived contracts**. Internal functions are accessible from the main contract in which they were initially declared and by the contracts that extend from this main contract through inheritance. 
#### 4. **Private**

**A private function can only be called by the main contract in which it was specified.** Private functions are used initially according to common practice, but if the scope is wider than this modifier type, any other plausible modifier should be used.

## Chapter 8
### Interface
**A Solidity contract interface is a list of function definitions without implementation**. Interfaces are the same as abstract contracts created by using an `interface` keyword, also known as a **pure abstract contract**. Interfaces do not have any definition or any state variables, constructors, or any function with implementation, they only contain function declarations. Functions of Interface can be only of type **external**. They can inherit from other interfaces, but they can’t inherit from other contracts. An interface can have enum, structs which can be accessed using interface name dot notation.

#### Interacting with other contracts

For our contract to talk to another contract on the blockchain that we don't own, first we need to define an `interface`.

Let's look at a simple **example**.
```js
contract LuckyNumber {
  mapping(address => uint) numbers;

  function setNum(uint _num) public {
    numbers[msg.sender] = _num;
  }

  function getNum(address _myAddress) public view returns (uint) {
    return numbers[_myAddress];
  }
}
```
This would be a simple contract where anyone could store their **lucky number**, and it will be associated with their Ethereum address. Then anyone else could look up that person's lucky number using their address.

Now let's say we had an external contract that wanted to read the data in this contract using the **getNum** function.

First we'd have to define an interface of the LuckyNumber contract:
```js
contract NumberInterface {
  function getNum(address _myAddress) public view returns (uint);
}
```
Notice that this looks like defining a contract, with a few differences. For one, we're only declaring the functions we want to interact with — in this case **getNum** — and we don't mention any of the other functions or state variables.

Secondly, we're not defining the function bodies. Instead of curly braces ({ and }), we're simply ending the function declaration with a **semi-colon (;)**. So it kind of looks like a contract skeleton. This is how the compiler knows it's an interface.

By including this interface in our dapp's code our contract knows what the other contract's functions look like, how to call them, and what sort of response to expect.
We can use it in a contract as follows:
```js
contract MyContract {
  address NumberInterfaceAddress = 0xab38...
  // ^ The address of the FavoriteNumber contract on Ethereum
  NumberInterface numberContract = NumberInterface(NumberInterfaceAddress);
  // Now `numberContract` is pointing to the other contract

  function someFunction() public {
    // Now we can call `getNum` from that contract:
    uint num = numberContract.getNum(msg.sender);
    // ...and do something with `num` here
  }
}
```
### Handling Multiple Return Values
In Solidity, from any type of function, you can return multiple values. You can do this by using the `return(value1, value2, ...)` statement or by directly assigning the value to the variable name that's used in the `returns()` statement.

The following is an example of returning multiple values from a function:
```js
function multipleReturns() internal returns(uint a, uint b, uint c) {
  return (1, 2, 3);
}

function processMultipleReturns() external {
  uint a;
  uint b;
  uint c;
  // This is how you do multiple assignment:
  (a, b, c) = multipleReturns();
}

// Or if we only cared about one of the values:
function getLastReturnValue() external {
  uint c;
  // We can just leave the other fields blank:
  (,,c) = multipleReturns();
}

```
## Chapter 9
### Decision Making
Decision making in programming is used when we have to adopt one out of a given set of paths for program flow. For this purpose, **conditional statements** are used which allows the program to execute the piece of code when the condition fulfills. Solidity uses control statements to control the flow of execution of the program to advance and branch-based changes to the state. Following conditional statements are provided by Solidity.
##### 1. If statement

This is the most basic conditional statement. It is used to make a decision whether the statement or block of code will be executed or not. If the condition is true the statements will be executed, else no statement will execute. 

##### Syntax:
```js
if (condition) {
   statement or block of code to be executed if the condition is True
}
```
##### Example
If statements in Solidity look just like JavaScript:
```js
function eatBLT(string memory sandwich) public {
  // Remember with strings, we have to compare their keccak256 hashes
  // to check equality
  if (keccak256(abi.encodePacked(sandwich)) == keccak256(abi.encodePacked("BLT"))) {
    eat();
  }
}
```

##### 2. if…else statement

This statement is the next form of conditional statement which allows the program to execute in a more controlled way. Here if the condition is true then the, if block is executed while if the condition is false then else block, is executed. 

##### Syntax:

```js
if (condition) {
   statement or block of code to be executed if condition is True
} else {
   statement or block of code to be executed if condition is False
}
```
##### 3. if…else if…else statement

This is a modified form of if…else conditional statement which is used to make a decision among several options. The statements start execution from if statement and as the condition of any if block is true the block of code associated with it is executed and rest if are skipped, and if none of the condition is true then else block executes.

##### Syntax:

```js
if (condition) {
   statement or block of code to be executed if the condition is True
} else if (condition 2) {
   statement or block of code to be executed if the condition of else...if is True
} else {
   statement or block of code to be executed if none of the condition is True
}
```