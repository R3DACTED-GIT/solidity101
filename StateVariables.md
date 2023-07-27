# State Variables in Solidity


State variables are an essential concept in Solidity and play a vital role in defining the state of a smart contract. Unlike local variables, which are temporary and exist only during the execution of a function, state variables are persistent and store data on the Ethereum blockchain. They represent the contract's state and preserve information across function calls and transactions.

## Usage of State Variables:
State variables are used to maintain important data and state information in smart contracts. They are accessible from any function within the contract, allowing the contract to keep track of data over time. State variables are typically defined at the contract level, outside of any function, and can have different data types, such as integers, strings, arrays, structs, mappings, and addresses.

## Commonly Used State Variables in ERC20 Tokens:

### Mappings:
Mappings are one of the most frequently used state variables in ERC20 tokens. They provide a way to create a dynamic data structure that associates one value with another. In the context of ERC20 tokens, mappings are commonly used to store token balances associated with user addresses.
```
solidity

contract MyToken {
    mapping(address => uint256) public balances;
}
```
In this example, the balances mapping is used to store the token balances of different users. Each address is associated with a specific balance, allowing the contract to track how many tokens each user holds.

### Arrays:
Arrays are another commonly used state variable, especially when dealing with lists or collections of data. In the context of ERC20 tokens, arrays might be used to keep track of a list of token holders or to store historical data.
```
solidity

contract MyToken {
    address[] public tokenHolders;
}
```
In this example, the tokenHolders array stores a list of addresses representing the holders of the ERC20 tokens.

### uint256 and address:
Simple data types like uint256 (unsigned integer) and address are frequently used to store numerical values and Ethereum addresses, respectively.
```
solidity

contract MyToken {
    uint256 public totalSupply;
    address public owner;
}
```
In this example, totalSupply stores the total number of tokens in circulation, and owner stores the address of the contract owner.

### Structs:
Structs are used to create custom data structures, combining multiple data types into a single variable. They are useful for organizing and storing related data together.
```
solidity

contract MyToken {
    struct TokenInfo {
        string name;
        string symbol;
    }

    TokenInfo public tokenData;
}
```
In this example, TokenInfo is a struct that contains two strings (name and symbol). The tokenData state variable stores the name and symbol of the ERC20 token.

State variables are crucial for preserving data and maintaining the state of smart contracts on the Ethereum blockchain. They are used to represent contract state, store user-specific data, and track information relevant to the contract's functionality. Understanding the various types of state variables and their usage is fundamental for designing and implementing complex smart contracts, such as ERC20 tokens, which rely heavily on persistent data storage.

here are some of the most used:
- Boolean: bool
    - Used to represent true or false values.
      
- Integer: uint, int
  - uint: Used for unsigned integers (positive values only).
  - int: Used for signed integers (positive and negative values).

- Address: address
  - Used to store Ethereum addresses (20-byte hexadecimal values).

- String: string
  - Used to store textual data (UTF-8 encoded strings).

- Bytes: bytes
  - Used to store arbitrary byte arrays.

- Fixed-Point Number: fixed, ufixed
  - fixed: Used for signed fixed-point numbers.
  - ufixed: Used for unsigned fixed-point numbers.

- Array: uint[], address[], string[], etc.
  - Used to store lists or collections of data.

- Mapping: mapping(KeyType => ValueType)
  - Used to create dynamic key-value pairs.

- KeyType: The data type of the keys.

- ValueType: The data type of the values.

- Enum: enum
  - Used to define a custom data type with a finite set of values.

- Struct: struct
  - Used to create custom data structures that combine multiple data types.
