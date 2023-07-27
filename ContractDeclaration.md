# Contract Declaration
In the context of Solidity, contract declaration refers to the process of defining a contract and its structure. A contract is a fundamental building block of smart contracts on the Ethereum blockchain. It encapsulates the code and data that define the rules and functionality of a decentralized application (DApp).

# A contract declaration in Solidity typically includes several essential components:

## Contract Name: The name of the contract, which serves as its identifier.
```
// Example 1: Simple contract name declaration
contract SimpleContract {
    // Contract code...
}
```
>Camel case is when a compound word or phrase has no spaces or punctuation. Instead, each separate word is indicated with either a lowercase or uppercase letter. Several companies use camel case in their names or for their products and systems. A few examples are: iPhone, FedEx, 
```
// Example 2: Contract name with CamelCase convention
contract myTokenContract {
    // Contract code...
}
```
## State Variables: Variables declared at the contract level that represent the contract's state. These variables store data and are accessible across various functions within the contract.
```
// Example 1: State variable to store an integer value
contract SimpleStorage {
    uint256 public data;
}
```
```
// Example 2: State variables for token contract
contract MyToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balances;
}
```
### List of variables 
- uint: Unsigned integer (positive whole numbers).
- int: Signed integer (positive or negative whole numbers).
- bool: Boolean (true or false).
- address: Ethereum address (20-byte identifier).
- string: String of characters.
- bytes: Byte array.
- bytes32: Fixed-size byte array (32 bytes).
- mapping: A key-value data structure used to create associative arrays or dictionaries.
- enum: A user-defined data type for representing a finite set of values.
- struct: A user-defined data type for creating complex data structures.
- array: A list of elements of the same type, denoted as type[] or type[n].
- address[]: An array of Ethereum addresses.
- uint[]: An array of unsigned integers.
- mapping(address => uint): A mapping associating Ethereum addresses with unsigned integers.
- mapping(address => uint[]): A mapping associating Ethereum addresses with arrays of unsigned integers.
- mapping(bytes32 => address): A mapping associating bytes32 keys with Ethereum addresses.
- mapping(string => uint256): A mapping associating string keys with unsigned integers.
- bytes32[]: An array of fixed-size byte arrays (bytes32).
- mapping(uint256 => string): A mapping associating unsigned integers with strings.
- mapping(enum => uint256): A mapping associating enum values with unsigned integers.
Note: Some variables, like uint and int, have different sizes, such as uint8, uint16, uint256, int8, int16, int256, etc. The choice of the appropriate variable type depends on the specific use case and the range of values the variable needs to represent.

These are just a few examples of the many variable types and combinations possible in Solidity. The appropriate choice of variables depends on the requirements and complexity of the smart contract you are developing. Always ensure that you choose the correct data types to optimize gas costs and avoid potential errors in your smart contract code.
</sup>

## Functions: Functions define the behavior and operations that the contract can perform. They can modify the contract's state, interact with other contracts, or perform calculations.
In the contract declaration part before the constructor, the most commonly used functions are:

- State-changing Functions: Functions that modify the contract's state.
- View Functions: Functions that do not modify the contract's state and only read data.
- Pure Functions: Functions that do not read or modify the contract's state and return a value.
- Fallback Function: A special function executed when the contract receives a transaction with no matching function.
Now, let's take two examples of functions from this list:

### Example 1: State-changing Function
```
// Example: A simple contract to manage a user's balance
contract UserBalance {
    mapping(address => uint256) private balances;

    // State-changing function to deposit tokens
    function depositTokens(uint256 amount) external {
        balances[msg.sender] += amount;
    }
    // State-changing function to withdraw tokens
    function withdrawTokens(uint256 amount) external {
        require(balances[msg.sender] >= amount, "Insufficient balance");
        balances[msg.sender] -= amount;
        // Additional code to transfer tokens to the user's wallet
    }
}
```
In this example, the depositTokens and withdrawTokens functions are state-changing functions that modify the contract's state. The depositTokens function allows users to deposit tokens into the contract, while the withdrawTokens function allows users to withdraw tokens from their balance.

### Example 2: View Function
```
// Example: A contract to get the square of a number without modifying the state
contract MathOperations {
    // View function to calculate the square of a number
    function square(uint256 x) external view returns (uint256) {
        return x * x;
    }
}
```
In this example, the square function is a view function as indicated by the view keyword. It calculates and returns the square of the input number x without modifying the contract's state. View functions are useful when you need to perform calculations or retrieve data without making any changes to the contract's state.

These examples demonstrate two types of functions in Solidity contracts: state-changing functions that modify the contract's state and view functions that read data without modifying the state. Keep in mind that the full contract often includes additional functions and logic to implement the desired behavior and functionalities.

## Modifiers: Modifiers are used to change the behavior of functions in the contract. They can be applied to functions to add conditions or restrictions before the function execution.
Modifiers are a powerful feature in Solidity that allow you to change the behavior of functions by adding conditions or restrictions before the function execution. They help to enforce access controls, validate inputs, or perform other checks to ensure that the function can be executed safely. Modifiers are defined separately and can be applied to multiple functions within the contract, promoting code reuse and maintainability.

```
modifier ModifierName(parameter1, parameter2, ...) {
    // Modifier logic, conditions, or checks
    _; // This underscore represents the location where the modified function's code will be executed.
}

function FunctionName() external ModifierName(parameter1, parameter2, ...) {
    // Function logic
}
```
A modifier is declared using the modifier keyword, followed by the modifier name and optional parameters.
The modifier code contains the conditions or checks that must be satisfied for the function to execute successfully. If the conditions are not met, the function's execution will be halted, and an exception will be thrown.
The _ (underscore) is a special placeholder that represents the location where the original function's code will be executed. It acts as a hook between the modifier and the modified function.

### Example 1: Simple Access Control using a Modifier
```
contract OnlyOwner {
    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function.");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function changeOwner(address newOwner) external onlyOwner {
        owner = newOwner;
    }

    function withdraw() external onlyOwner {
        // Withdraw funds from the contract
    }
}
```
In this example, we have a contract OnlyOwner with a state variable owner, representing the contract owner's address. The onlyOwner modifier restricts access to certain functions (changeOwner and withdraw) to be callable only by the contract owner. It does this by comparing the msg.sender (the caller of the function) with the owner address. If the condition is met (caller is the owner), the function code is executed; otherwise, it throws an exception and reverts the transaction.

### Example 2: Restricting Function Execution based on Input
```
contract SafeDivide {
    modifier notZero(uint256 number) {
        require(number != 0, "Cannot divide by zero.");
        _;
    }
    function divide(uint256 numerator, uint256 denominator) external notZero(denominator) returns (uint256) {
        return numerator / denominator;
    }
}
```
In this example, we have a contract SafeDivide with a function divide that performs division. The notZero modifier is used to ensure that the denominator provided to the divide function is not zero. If the denominator is zero, the function execution is halted, and an exception is thrown. This prevents division by zero, which could cause unexpected results or lead to contract failures.

Modifiers in Solidity provide a powerful way to enforce conditions and restrictions on functions, making smart contracts more secure and robust. By using modifiers, you can add reusable checks and ensure that your contract functions are executed under the appropriate conditions, helping to prevent vulnerabilities and bugs in your code.

## Events: Events are used to emit information from the contract. They allow external applications to be notified of specific contract activities, enabling easier tracking and monitoring.
Events are an essential feature in Solidity that allow contracts to communicate and emit information to external applications. They serve as a way to notify off-chain applications, such as user interfaces or other smart contracts, about specific activities or state changes that occur within the contract. Events are useful for logging critical information, facilitating easier tracking, monitoring, and auditing of contract activities.
```
event EventName(parameter1Type indexed parameter1Name, parameter2Type indexed parameter2Name, ...);

function functionName() external {
    // Function logic
    emit EventName(parameter1Value, parameter2Value, ...);
}
```
To declare an event, you use the event keyword, followed by the event name and its parameters.
Events can have one or more indexed parameters. Indexed parameters are useful for filtering events when querying the Ethereum event logs, improving the efficiency of event retrieval.
Inside the function or whenever the event needs to be emitted, you use the emit keyword, followed by the event name and its corresponding parameter values.

### Example 1: Logging a Transaction
```
contract Token {
    mapping(address => uint256) private balances;

    event Transfer(address indexed from, address indexed to, uint256 value);

    function transfer(address recipient, uint256 amount) external {
        require(balances[msg.sender] >= amount, "Insufficient balance");

        balances[msg.sender] -= amount;
        balances[recipient] += amount;

        emit Transfer(msg.sender, recipient, amount);
    }
}
```
In this example, the Token contract contains a transfer function for transferring tokens from one address to another. When the transfer occurs, the Transfer event is emitted with the sender's address (from), the recipient's address (to), and the transferred amount (value). This event can be captured by external applications, such as a DApp frontend, to update user interface elements or perform additional actions based on the token transfer.

### Example 2: Logging a User Registration
```
contract UserRegistry {
    struct User {
        string name;
        uint256 age;
    }

    mapping(address => User) private users;

    event NewUserRegistered(address indexed userAddress, string name, uint256 age);

    function registerUser(string memory name, uint256 age) external {
        User memory newUser = User(name, age);
        users[msg.sender] = newUser;

        emit NewUserRegistered(msg.sender, name, age);
    }
}
```
In this example, the UserRegistry contract allows users to register with their name and age. When a new user registers, the NewUserRegistered event is emitted, providing the user's address (userAddress), name, and age. This event can be captured by external applications to keep track of new user registrations and perform any required processing or notifications.

Events in Solidity play a crucial role in facilitating communication between smart contracts and external applications. They enable easy monitoring and tracking of contract activities, providing essential data for auditing, analytics, and user interfaces. By emitting events, contracts can interact with the broader Ethereum ecosystem, allowing decentralized applications to react and respond to changes happening on the blockchain.

## Constructor Function: The constructor function is a special function that runs only once during contract deployment. It is used for contract initialization, setting initial values for state variables or performing other setup tasks.
The constructor function is a special function in Solidity that runs only once during the contract deployment process. It is automatically executed when you deploy a new instance of the contract. The primary purpose of the constructor is to initialize the contract's state variables, set default values, and perform other setup tasks required for the contract to function correctly.

```
contract MyContract {
    // State variables

    // Constructor function
    constructor() {
        // Initialize state variables and perform setup tasks
    }

    // Other functions and modifiers
}
```
The constructor function does not have a name like regular functions, and it doesn't have a return type (not even void).

In a contract, you can have only one constructor.

The constructor is optional, and if you don't define one, the compiler will automatically provide a default constructor with no logic.

### Example 1: Simple Contract Initialization

```
contract SimpleStorage {
    uint256 public data;

    // Constructor to set an initial value for data
    constructor() {
        data = 100;
    }
}
```
In this example, we have a contract called SimpleStorage with a state variable data. The constructor function is used to initialize the data variable with an initial value of 100.

### Example 2: Setting Multiple Initial Values

contract Employee {
    string public name;
    uint256 public age;
    address public employeeAddress;

    // Constructor to set initial values for name, age, and employeeAddress
    constructor(string memory _name, uint256 _age) {
        name = _name;
        age = _age;
        employeeAddress = msg.sender;
    }
}
```
In this example, the Employee contract has three state variables: name, age, and employeeAddress. The constructor function accepts two parameters (_name and _age) and uses them to set the initial values of the respective state variables. Additionally, it sets the employeeAddress to the address of the account that deploys the contract (i.e., msg.sender).

The constructor function is a critical part of Solidity contracts as it allows for proper contract initialization and setup during deployment. It is used to define the initial state of the contract and can contain any logic necessary to prepare the contract for its intended functionality. Having a well-defined constructor is crucial for ensuring that your smart contracts work as intended and are ready for use upon deployment.

By declaring a contract in Solidity, developers define the structure and behavior of the smart contract, enabling it to interact with users, other contracts, and the Ethereum blockchain. Contracts form the foundation for building decentralized applications, enabling trustless and automated execution of agreements, services, and transactions on the blockchain.
