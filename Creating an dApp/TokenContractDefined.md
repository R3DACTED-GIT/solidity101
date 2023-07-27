# Token Contract (ERC20)

The ERC20 token contract is a smart contract that implements the ERC20 standard, which defines a set of rules and functions for creating a fungible token on the Ethereum blockchain. A fungible token means each unit of the token is identical and interchangeable with any other unit. This contract will be the foundation of our token lottery DApp and will represent the prize for the lottery as well as the currency for purchasing lottery tickets.

## Functions Needed:

### Constructor:
- Description: The constructor function is called only once during contract deployment. It initializes the token with initial values like the token's name, symbol, decimals, and initial total supply.
  - Parameters:
    -string memory _name: The name of the token.
    -string memory _symbol: The symbol or ticker of the token.
    - uint8 _decimals: The number of decimal places the token supports (e.g., 18).
    - uint256 _initialSupply: The initial total supply of tokens.

### totalSupply:
- Description: Returns the total supply of tokens.
    - Function Signature: function totalSupply() external view returns (uint256).

### balanceOf:
- Description: Returns the token balance of a specified address.
    - Function Signature: function balanceOf(address account) external view returns (uint256).

### transfer:
- Description: Transfers tokens from the sender's address to the recipient's address.
    - Function Signature: function transfer(address recipient, uint256 amount) external returns (bool).

### transferFrom:
- Description: Transfers tokens from one address to another on behalf of an approved address.
    - Function Signature: function transferFrom(address sender, address recipient, uint256 amount) external returns (bool).

### approve:
- Description: Approves an address to spend a specified amount of tokens on behalf of the sender.
    - Function Signature: function approve(address spender, uint256 amount) external returns (bool).

### allowance:
- Description: Returns the remaining number of tokens that an approved address can spend on behalf of the token owner.
    - Function Signature: function allowance(address owner, address spender) external view returns (uint256).

### Explanation:

- The constructor sets the initial token parameters, including the name, symbol, decimals, and initial total supply of tokens.
- totalSupply is an external view function that returns the total supply of tokens.
- balanceOf is an external view function that takes an address as input and returns the token balance of that address.
- transfer is an external function that allows the sender to transfer a specified amount of tokens to a recipient.
- transferFrom is an external function that allows a designated address (with approval) to transfer tokens on behalf of the token owner.
- approve is an external function that allows the token owner to approve a specific address to spend a certain amount of tokens on their behalf.
- allowance is an external view function that takes the token owner's address and the spender's address as input and returns the remaining allowance for the spender to spend tokens on behalf of the owner.

### Example Code:
Below is an example implementation of the ERC20 token contract:
```
solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ERC20Token {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balances;
    mapping(address => mapping(address => uint256)) public allowances;

    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply;
        balances[msg.sender] = _initialSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return balances[account];
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(balances[msg.sender] >= amount, "ERC20: insufficient balance");

        balances[msg.sender] -= amount;
        balances[recipient] += amount;

        emit Transfer(msg.sender, recipient, amount);
        return true;
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        allowances[msg.sender][spender] = amount;

        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(balances[sender] >= amount, "ERC20: insufficient balance");
        require(allowances[sender][msg.sender] >= amount, "ERC20: allowance exceeded");

        balances[sender] -= amount;
        balances[recipient] += amount;
        allowances[sender][msg.sender] -= amount;

        emit Transfer(sender, recipient, amount);
        return true;
    }

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}
```
This example code showcases a basic ERC20 token contract that includes the essential functions to operate a standard ERC20 token on the Ethereum blockchain.
