# Lottery Contract

The Lottery contract is responsible for managing the lottery process, including ticket purchases, selecting the winner(s), and distributing the prize pool in tokens. It interacts with the ERC20 token contract to handle token transfers for ticket purchases and prize distribution. The lottery can have a fixed ticket price and a defined duration during which participants can buy tickets. At the end of the lottery period, the contract will randomly select the winner(s) from the pool of participants.

## Functions Needed:

- Constructor:
  - Description: The constructor function initializes the lottery with the ERC20 token contract address, the ticket price, the lottery duration, and other necessary parameters.
      - Parameters:
      - address _tokenAddress: The address of the ERC20 token contract.
      - uint256 _ticketPrice: The price of a single lottery ticket in tokens.
      - uint256 _durationInBlocks: The duration of the lottery in Ethereum blocks (e.g., 1 day = 5760 blocks at ~15 seconds per block).

- buyTicket:
  - Description: Allows users to buy lottery tickets by transferring the ticket price in tokens to the lottery contract. Each participant can buy multiple tickets.
   - Function Signature: function buyTicket(uint256 _numberOfTickets) external returns (bool).

- endLottery:
  - Description: Ends the lottery and determines the winner(s). This function can only be called after the lottery duration has elapsed.
   - Function Signature: function endLottery() external returns (bool).

- claimPrize:
  - Description: Allows the winner(s) to claim their prize, transferring the prize amount in tokens from the lottery contract to their address.
   - Function Signature: function claimPrize() external returns (bool).

## Explanation:

- The constructor initializes the lottery contract with essential parameters like the ERC20 token contract address, the ticket price, and the duration of the lottery.
- buyTicket allows users to purchase lottery tickets by transferring the required ticket price in tokens to the lottery contract. The number of tickets to be purchased can be specified.
- endLottery ends the lottery after the predefined duration has passed. This function triggers the random selection of the winner(s) among the participants.
- claimPrize allows the winner(s) to claim their prize, transferring the prize amount in tokens from the lottery contract to their address. This function can be called after the winner(s) have been determined.

## Example Code:


```
pragma solidity ^0.8.0;

import "./LotteryToken.sol";

contract Lottery {
    address public tokenAddress;
    ERC20Token private tokenContract;
    uint256 public ticketPrice;
    uint256 public durationInBlocks;
    uint256 public endBlock;
    address[] public participants;
    address[] public winners;

    constructor(
        address _tokenAddress,
        uint256 _ticketPrice,
        uint256 _durationInBlocks
    ) {
        tokenAddress = _tokenAddress;
        tokenContract = ERC20Token(_tokenAddress);
        ticketPrice = _ticketPrice;
        durationInBlocks = _durationInBlocks;
        endBlock = block.number + _durationInBlocks;
    }

    function buyTicket(uint256 _numberOfTickets) external returns (bool) {
        require(_numberOfTickets > 0, "Lottery: Number of tickets must be greater than zero");
        uint256 totalCost = ticketPrice * _numberOfTickets;
        require(tokenContract.balanceOf(msg.sender) >= totalCost, "Lottery: Insufficient balance");

        for (uint256 i = 0; i < _numberOfTickets; i++) {
            participants.push(msg.sender);
        }

        require(tokenContract.transferFrom(msg.sender, address(this), totalCost), "Lottery: Token transfer failed");

        return true;
    }

    function endLottery() external returns (bool) {
        require(block.number >= endBlock, "Lottery: Lottery is still active");

        uint256 totalParticipants = participants.length;
        if (totalParticipants > 0) {
            uint256 winnerIndex = generateRandomNumber(totalParticipants);
            winners.push(participants[winnerIndex]);
        }

        return true;
    }

    function claimPrize() external returns (bool) {
        require(winners.length > 0, "Lottery: No winner yet");
        address winner = winners[0];
        uint256 prizeAmount = tokenContract.balanceOf(address(this));
        require(prizeAmount > 0, "Lottery: No prize available");

        delete winners;
        require(tokenContract.transfer(winner, prizeAmount), "Lottery: Token transfer failed");

        return true;
    }

    function generateRandomNumber(uint256 _upperBound) private view returns (uint256) {
        return uint256(keccak256(abi.encodePacked(block.difficulty, block.timestamp, participants))) % _upperBound;
    }
}
```
# What happens if nobody wins?
In the provided Lottery contract, if nobody wins (i.e., no participants buy any tickets), the tokens used for purchasing tickets remain in the contract until the lottery is ended and a winner is determined. In this case, the prize pool will be equal to the total amount of tokens that were used by participants to buy tickets.

When the lottery is ended using the endLottery function, the contract checks whether there are any participants (i.e., if the participants array is not empty). If there are no participants (no tickets were purchased), the endLottery function will simply return, and the prize pool will remain in the contract.

Since there are no winners in this scenario, the tokens stay in the contract and do not get distributed to any address. Participants have effectively forfeited their tickets, and the prize pool remains unclaimed.

It's important to consider edge cases like this when designing a lottery contract. Depending on the rules of your specific lottery, you may decide to handle situations like this differently. For example, you could have a mechanism to refund the tickets if there are no participants or carry the prize pool over to the next round if the lottery is recurring.

As always, when building a smart contract, it's crucial to carefully consider the intended behavior and potential edge cases to ensure that the contract functions as intended and is secure.
