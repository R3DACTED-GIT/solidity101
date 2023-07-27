# Additional ERC20 Features

## totalSupply:
The totalSupply function is an optional ERC20 function used to get the total supply of tokens in circulation. It allows external applications and user interfaces to query the total number of tokens available in the token contract.

### Example: Implementing the totalSupply Function
```
solidity

contract MyToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balances;

    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply;
        balances[msg.sender] = _initialSupply;
    }

    function totalSupply() external view returns (uint256) {
        return totalSupply;
    }
}
```
In this example, we have added the totalSupply function to the ERC20 token contract. The function is declared as an external view function, meaning it can be called from outside the contract, and it does not modify the contract's state.

## decimals:
The decimals function is an optional ERC20 function that provides the number of decimal places used for token representation. For example, if decimals is set to 18, the smallest unit of the token will be 1 wei (0.000000000000000001 tokens).

### Example: Implementing the decimals Function
```
solidity

contract MyToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balances;

    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply;
        balances[msg.sender] = _initialSupply;
    }

    function decimals() external view returns (uint8) {
        return decimals;
    }
}
```
In this example, we have added the decimals function to the ERC20 token contract. The function is declared as an external view function and returns the decimals value.

## Adding Custom Functionality to the ERC20 Contract:
Beyond the standard ERC20 functions, you can add custom functionality to the ERC20 contract to suit the specific needs of your token. For example, you might want to implement functions for token minting, token burning, token locking, or token freezing.

### Example: Custom Function for Token Minting

```
solidity
contract MyToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balances;

    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function.");
        _;
    }

    constructor(string memory _name, string memory _symbol, uint8 _decimals, uint256 _initialSupply) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply;
        balances[msg.sender] = _initialSupply;
        owner = msg.sender;
    }

    function mint(address recipient, uint256 amount) external onlyOwner {
        require(recipient != address(0), "Invalid recipient address");

        // Perform minting by increasing the total supply and the recipient's balance
        totalSupply += amount;
        balances[recipient] += amount;

        // Emit the Transfer event to log the minting
        emit Transfer(address(0), recipient, amount);
    }

    // Additional custom functions...
}
```
In this example, we have added a custom function called mint to the ERC20 token contract. The mint function can only be called by the contract owner (onlyOwner modifier) and allows the owner to create new tokens by increasing the total supply and the balance of a specified recipient.

# Conclusion:

Implementing additional optional ERC20 functions like totalSupply and decimals provides valuable information about the token's supply and the token's divisibility. Additionally, custom functionality allows developers to tailor the ERC20 contract to specific use cases, such as minting or burning tokens. By extending the standard ERC20 functions and adding custom features, developers can create more versatile and feature-rich ERC20 token contracts to meet the requirements of their projects.
