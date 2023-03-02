## Lesson 5
## Chapter 1
### Tokens on Ethereum
A `token` on Ethereum is basically just a smart contract that follows some common rules — namely it **implements a standard set of functions that all other token contracts share**, such as `transferFrom(address _from, address _to, uint256 _amount)` and `balanceOf(address _owner)`.Internally the smart contract usually has a **mapping**, `mapping(address => uint256) balances`, that keeps track of how much balance each address has. So basically a token is just a contract that keeps track of who owns how much of that token, and some functions so those users can transfer their tokens to other addresses.

#### ERC20 Token
**The ERC-20 is a popular fungible cryptocurrency token compatible with Ethereum or EVM-compatible blockchains**. It serves as a digital asset that can represent anything on the blockchain, which enables flexibility for many use cases.Since all ERC20 tokens share the same set of functions with the same names, they can all be interacted with in the same ways.This preset smart contract contains a specific interface that allows it to be used as a technical standard for development. Developers can leverage the ERC-20 token and its associated token rules for an easier developer experience. 

This means if you build an application that is capable of interacting with one ERC20 token, it's also capable of interacting with any ERC20 token. That way more tokens can easily be added to your app in the future without needing to be custom coded. You could simply plug in the new token contract address, and boom, your app has another token it can use.

One example of this would be an exchange. When an exchange adds a new ERC20 token, really it just needs to add another smart contract it talks to. Users can tell that contract to send tokens to the exchange's wallet address, and the exchange can tell the contract to send the tokens back out to users when they request a withdraw.

The exchange only needs to implement this transfer logic once, then when it wants to add a new ERC20 token, it's simply a matter of adding the new contract address to its database.
### What is an ERC-20 token?

An ERC-20 token is a smart contract on Ethereum that implements the methods and events specified in the ERC-20 standard. It is designed to be used as a fungible token, meaning each instance (or unit) of a token has the same value as another instance of the same token. It provides the following functionalities: 

*   transfer tokens from one account to another
*   get the current token balance of an account
*   get the total supply of the token available on the network
*   approve whether an amount of token from an account can be spent by a third-party account 

Put simply, the ERC20 standard defines a set of **functions** to be implemented by all ERC20 tokens so as to allow integration with other **contracts, wallets, or marketplaces**. This set of functions is rather short and basic.

*   `function totalSupply() public view returns (uint256);` -   The **totalSupply** method denotes the current circulating total supply of the tokens
*   `function balanceOf(address tokenOwner) public view returns (uint);` -  The **balanceOf** method calculates the number of tokens contained in a particular wallet address.
*   `function allowance(address tokenOwner, address spender) public view returns (uint);` -    The **allowance** method is the amount one address can spend on behalf of another address. 
*   `function transfer(address to, uint tokens) public returns (bool);` -   The **transfer** method sends tokens from one address to another, where the sender is the origin of the transaction.
*   `function approve(address spender, uint tokens)  public returns (bool);` -  The **approve** method allows another address to spend tokens on your behalf. This is useful when interacting with smart contracts that need access to your tokens. A popular use case for the approve method is a decentralized exchange. 
*   `function transferFrom(address from, address to, uint tokens) public returns (bool);` - The **transferFrom** method is useful together with the approve method. Once you approve someone to spend your tokens, they can transferFrom your account to another account as part of a transaction. A good use case is a decentralized exchange.

ERC20 functions allow an external user, say a crypto-wallet app, to find out a user’s balance and transfer funds from one user to another with proper authorization.

**The smart contract defines two specifically defined **events**:**

*   `event Approval(address indexed tokenOwner, address indexed spender, uint tokens);`
*   `event Transfer(address indexed from, address indexed to, uint tokens);`

**Important keywords used:**

*   `Event`: An event is Solidity’s way of allowing clients e.g notifying your front end application about the occurrence of a specific event.
*   `View`: View functions ensure that they will not modify the state
*   `public`: A public function can be accessed outside the contract itself
*   `indexed`: Up to three parameters can receive the attribute indexed through which we can search respective arguments.

#### Other token standards ERC721

**ERC20** tokens are really cool for tokens that act like currencies. But they're not particularly useful for representing zombies in our zombie game.

For one, zombies aren't divisible like currencies — I can send you 0.237 ETH, but transfering you 0.237 of a zombie doesn't really make sense.

Secondly, all zombies are not created equal. Your Level 2 zombie **"Steve"** is totally not equal to **my Level 732** zombie **"H4XF13LD MORRIS 💯💯😎💯💯"**. (Not even close, Steve).

There's another token standard that's a much better fit for crypto-collectibles like CryptoZombies — and they're called **ERC721** tokens.

**ERC721** tokens are not interchangeable since each one is assumed to be **unique**, and are not divisible. You can only trade them in whole units, and each one has a unique ID. So these are a perfect fit for making our zombies tradeable.

**Note** that using a standard like **ERC721** has the benefit that we don't have to implement the **auction or escrow logic** within our contract that determines how players can **trade / sell** our zombies. If we conform to the spec, someone else could build an exchange platform for crypto-tradable ERC721 assets, and our ERC721 zombies would be usable on that platform. So there are clear benefits to using a token standard instead of rolling your own trading logic.

#### What is the ERC-721 token?
ERC-721 is basically a template or a guideline that other developers agree to follow. It is a widely used standard. Being a widely used standard also means being compatible with a  wide range of applications. 

*   **ERC-721** is a standard for **“non-fungible”** tokens. A non-fungible token is the type of token that is **unique**. These are cryptographic **assets** on a blockchain that has a unique code and metadata differentiating them from one another.
*   The `ERC-721` standard non-fungible tokens can be seen in **blockchain-based games** where each asset is unique and players can **trade it and sell or buy items** with it.
*   One of the examples that use the `ERC-721 non-fungible tokens` standard is the **Cryptokitties** game which is basically a game developed on the blockchain which allows users to sell, buy and breed virtual cats. 
*   Some other examples include **Ethermon, MyCrypto, Cryptodoggies**, etc. The assets bought with a non-fungible token are unique and only belong to the owner. It can’t be traded. **It’s like a property that one owns** (a house).
*   The `ERC-721` standard tokens are created the same way as other kinds of tokens and can be created as many as the user wishes but the only difference is that the tokens have a unique value and unique metadata, making it non-fungible.

#### Characteristics of ERC-721 Tokens:

*   ERC-721 tokens are the standards for **non-fungible tokens (NFTs).**
*   These tokens **cannot be exchanged** for anything of equal value since they are **unique** in themselves, representing a **unique asset.**
*   Each **NFT** token is linked to different owners and has its **own tokenID or metadata** that makes them unique.
*   The most popular application areas of ERC-721 tokens are NFTs in **gaming** ( such as cryptokitties).

##### The following are the set of functions and events defined in the ERC-721 standard:

ERC-721 defines some functions with compliance to ERC-20. This makes it easier for existing wallets to display simple token information.
##### ERC20-like Functions:

*   `name`: Used to define the token’s name, which other contracts and applications can identify.
*   `symbol`: Used to define token’s shorthand name or symbol.
*   `totalSupply`: This function is used to define the total number of tokens on the blockchain; the supply doesn’t have to be constant.
*   `balanceOf`: Returns number of NFTs owned by an address.
##### Ownership Functions

*   `ownerOf`: This function returns the address of the owner of a token. As each ERC-721 token is unique and non-fungible, they are represented on the blockchain by an ID. Other users, contracts, apps can use this ID to determine the owner of the token.
*   `approve`: This function grants or approves another entity the permission to transfer tokens on the owner’s behalf.
*   `takeOwnership`: This is an optional function that acts like a withdraw function since an outside party can call it to take tokens out of another user’s account. Therefore, takeOwnership can be used when a user has been approved to own a certain amount of tokens and wishes to withdraw said tokens from another user’s balance.
*   `transfer`: This is another type of transfer function; it allows the owner to transfer the token to another user, just like other digital tokens/coins.
*   `tokenOfOwnerByIndex`: This is an optional but recommended function. Each owner can own more than one NFT at the same time. Its unique ID identifies every NFT, and eventually, it can become difficult to keep track of IDs. So the contract stores these IDs in an array and the tokenOfOwnerByIndex function lets us retrieve this information from the array.

##### Metadata Function

*   `tokenMetadata`: This optional feature is an interface that lets us discover a token’s metadata or a link to its data.


##### Events

*   `Transfer`: This event is fired when the ownership of the token changes from one individual to another. It emits the information on which account transferred the token, which account received the token, and which token (by ID) was transferred.
*   `Approve`: This event is fired when a user approves another user to take ownership of the token, i.e., it is fired whenever approve function is executed. It emits the information on which account currently owns the token, which account is approved to take ownership of the token in the future, and which token (by ID) is approved to have its ownership transferred.

#### Use cases of Non-Fungible Tokens (NFTs):

*   **Digital art (or physical art)**: Art pieces are the most popular use cases of NFTs. Digital art auctions were the first application of NFTs and continue to grow.
*   **Gaming**: Providing in-game purchases and collectibles of games.
*   **Real estate**: Tokenizing properties and smart contracts and carry buying and selling.
*   **Finance**: Financial instruments like loans, futures, and other responsibilities. 
*   **Software** titles: Software licenses to ensure anti-piracy and privacy.
*   **Concert tickets/Sports match tickets**: To ensure that no fraud happens in ticket selling and fans can have a single place to view past experiences.
*   **KYC compliance**: Creating a token for a specific user’s KYC.

Let's take a look at the ERC721 standard:
```js
contract ERC721 {
  event Transfer(address indexed _from, address indexed _to, uint256 indexed _tokenId);
  event Approval(address indexed _owner, address indexed _approved, uint256 indexed _tokenId);

  function balanceOf(address _owner) external view returns (uint256);
  function ownerOf(uint256 _tokenId) external view returns (address);
  function transferFrom(address _from, address _to, uint256 _tokenId) external payable;
  function approve(address _approved, uint256 _tokenId) external payable;
}
```

#### Implementing a token contract

When **implementing** a token contract, the first thing we do is copy the interface to its own Solidity file and import it, `import "./erc721.sol";`. Then we have our contract inherit from it, and we override each method with a function definition.

But wait — `ZombieOwnership` is already inheriting from `ZombieAttack` — how can it also inherit from ERC721?

Luckily in Solidity, your contract can inherit from multiple contracts as follows:
```js
contract SatoshiNakamoto is NickSzabo, HalFinney {
  // Omg, the secrets of the universe revealed!
}
```
As you can see, when using multiple inheritance, you just separate the multiple contracts you're inheriting from with a comma, ,. In this case, our contract is inheriting from **NickSzabo and HalFinney**.

### Chapter 2
#### ERC721: balanceOf 
```js
    function balanceOf(address _owner) external view returns (uint256 _balance);
```
This function simply takes an **address**, and returns how many tokens that address owns.
##### Example

```js
     function balanceOf(address _owner) external view returns (uint256) {
          // 1. Return the number of zombies `_owner` has here
       return ownerZombieCount[_owner];
        }
 ```
 #### ERC721: ownerOf
```js
    function ownerOf(uint256 _tokenId) external view returns (address _owner);
```
This function takes a **token ID** (in our case, a Zombie ID), and returns the address of the **person** who owns it.
##### Example
```js
     function ownerOf(uint256 _tokenId) external view returns (address) {
         // 2. Return the owner of `_tokenId` here
      return zombieToOwner[_tokenId];
         }
  ```
  
 ### Chapter 3
 #### ERC721: Transfer Logic
 In ERC721 implementation, transfering ownership from one person to another.
 Note that the ERC721 spec has 2 different ways to transfer tokens:

1.  `function transferFrom(address _from, address _to, uint256 _tokenId) external payable;` -   The first way is the token's owner calls `transferFrom` with his address as the `_from` parameter, the address he wants to transfer to as the `_to` parameter, and the `_tokenId` of the token he wants to transfer.
2.  `function approve(address _approved, uint256 _tokenId) external payable;` - The second way is the token's owner first calls `approve` with the `address` he wants to **transfer to**, and the `_tokenID` . The contract then stores who is approved to take a token, usually in a `mapping (uint256 => address)`. Then, when the owner or the approved address calls `transferFrom`, the contract checks if that `msg.sender` is the owner or is approved by the owner to take the token, and if so it transfers the token to him.

Notice that both methods contain the same transfer logic. In one case the sender of the token calls the `transferFrom` function; in the other the owner or the approved receiver of the token calls it.

So it makes sense for us to abstract this logic into its own private function, `_transfer`, which is then called by `transferFrom`.

##### Example 
Implementing `_transfer` and `transferform` functions:
 
```js
pragma solidity >=0.5.0 <0.6.0;
import "./zombieattack.sol";
import "./erc721.sol";

contract ZombieOwnership is ZombieAttack, ERC721 {

  mapping (uint => address) zombieApprovals;

  function balanceOf(address _owner) external view returns (uint256) {
    return ownerZombieCount[_owner];
  }

  function ownerOf(uint256 _tokenId) external view returns (address) {
    return zombieToOwner[_tokenId];
  }

  function _transfer(address _from, address _to, uint256 _tokenId) private {
    ownerZombieCount[_to]++;
    ownerZombieCount[_from]--;
    zombieToOwner[_tokenId] = _to;
    emit Transfer(_from, _to, _tokenId);
  }

  function transferFrom(address _from, address _to, uint256 _tokenId) external payable {
    require (zombieToOwner[_tokenId] == msg.sender || zombieApprovals[_tokenId] == msg.sender);
    _transfer(_from, _to, _tokenId);
  }
 ```
 
 ### Chapter 4
 #### ERC721: Approve

Now, let's implement `approve`.

Remember, with `approve` the transfer happens in 2 steps:

1.  You, the owner, call `approve` and give it the **_approved address** of the new owner, and the **_tokenId** you want them to take.
2.  The new owner calls `transferFrom` with the **_tokenId**. Next, the contract checks to make sure the new owner has been already approved, and then transfers them the token.

Because this happens in 2 function calls, we need to use the zombieApprovals data structure to store who's been approved for what in between function calls.

##### Example

```js
    // 1. Add function modifier here
     function approve(address _approved, uint256 _tokenId) external payable onlyOwnerOf(_tokenId) {
    // 2. Define function here
    zombieApprovals[_tokenId] = _approved;
    }
```

There is one more thing to do- there's an `Approval` event in the `ERC721 spec`. So we should fire this event at the end of the `approve` function.
##### Example 

```js
    function approve(address _approved, uint256 _tokenId) external payable onlyOwnerOf(_tokenId) {
        zombieApprovals[_tokenId] = _approved;
         //Fire the Approval event here
        emit Approval(msg.sender, _approved, _tokenId);
    }
 ```