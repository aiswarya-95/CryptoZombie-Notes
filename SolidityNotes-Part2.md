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
We can use an address to acquire a balance using the **.balance** method and to transfer a balance using the **.transfer** method.