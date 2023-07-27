# Contract Declaration
In the context of Solidity, contract declaration is a crucial aspect of smart contract development. It involves defining the contract's structure, including its state variables, functions, events, and modifiers, that collectively define the behavior and rules of a decentralized application (DApp).

Understanding Contracts:
In the Ethereum blockchain, a contract is essentially a self-contained piece of code that lives on the blockchain and can perform various actions, store data, and interact with other contracts. Contracts are written in Solidity, a programming language specifically designed for creating smart contracts.

Contract Structure:
The structure of a contract includes various elements that determine its functionality. Here are the key components typically found in a contract:

- State Variables: These are variables that represent the contract's state and store data persistently on the blockchain. State variables define the information that the contract needs to maintain throughout its lifetime. For example, in a token contract, the state variable balances may keep track of the token balances of different users.

- Functions: Functions define the behavior and operations that the contract can perform. They can modify the contract's state, interact with other contracts, or perform calculations. Functions can be of various types, such as state-changing functions, view functions, and pure functions.

- Events: Events are used to emit information from the contract. They allow external applications to be notified of specific contract activities, enabling easier tracking and monitoring. Events are often used for logging important actions that occur within the contract.

- Modifiers: Modifiers are used to change the behavior of functions in the contract. They can be applied to functions to add conditions or restrictions before the function execution. Modifiers help enforce access controls, validate inputs, and ensure certain conditions are met before executing a function.

Let's take a simple example of a voting contract:
```
solidity

// Voting contract
contract Voting {
    // State variables
    mapping(address => bool) public hasVoted;
    mapping(string => uint256) public voteCount;

    // Events
    event Voted(address indexed voter, string candidate);

    // Constructor (optional)
    constructor() {
        // Contract initialization tasks (if any)
    }

    // Functions
    function vote(string memory candidate) external {
        require(!hasVoted[msg.sender], "You have already voted.");
        voteCount[candidate]++;
        hasVoted[msg.sender] = true;

        emit Voted(msg.sender, candidate);
    }

    // Modifiers (optional)
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the contract owner can call this function.");
        _;
    }
}
```
In this example, the contract Voting has state variables to keep track of whether a voter has voted (hasVoted) and the vote count for each candidate (voteCount). It also includes a function vote to let users cast their votes and an event Voted to emit information about the voting action.

Contract declaration is a fundamental step in Solidity smart contract development. It involves defining the contract's structure and components that determine how the contract will function and interact with the Ethereum blockchain. Understanding contract declaration is crucial for anyone seeking to create decentralized applications using Solidity.
