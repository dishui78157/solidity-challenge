# Calyptus Solidity Challenge 13
*This contract implements a lottery system where users can register by paying 1 ETH each, and the owner can choose a winner and transfer the lottery amount. What's wrong with deploying it on the mainnet?
*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Lottery {
    address public owner;
    uint public id;
    mapping(uint => address) public participantsById;
    uint public winnerId;
    address public winner;

    constructor() {
        owner = msg.sender;
    }

    function register() public payable {
        require(msg.value == 1 ether, "Enter correct amount!");
        participantsById[id] = msg.sender;
        id++;
    }

    function chooseWinner() public {
        require(msg.sender == owner, "Only owner can choose winner!");
        require(id == 10, "Not enough participants!");
        winnerId = uint(keccak256(abi.encodePacked(block.timestamp, block.number))) % 10 + 1;
        winner = participantsById[winnerId];
        payable(winner).transfer(address(this).balance);
    }
}
`
# Answer
1. Deployer could get their own winner id by monitoring block state then running check
2. Bad RNG algo
3. Id 0 loses b/c check always btwn 1 & 10
4. Require fails w > 11 user reg, no guard on register 👉 DoS
5. mod value doesn’t match poss max users
6. -Register() should revert after the registration limit is reached

The chooseWinner function is vulnerable to a block timestamp manipulation attack.

The block.timestamp value can be manipulated by miners to a certain degree. This can be used to influence the outcome of the random number generation in the chooseWinner function.


The code defines a contract named Lottery. This contract implements a simple lottery system where users can register by paying 1 ETH each, and the contract owner can choose a winner who receives the total amount collected.

The contract has several state variables:

- owner is the address of the contract owner, which is set to the address that deploys the contract.
- id is a counter used to assign a unique ID to each participant.
- participantsById is a mapping that stores the address of each participant, indexed by their ID.
- winnerId is the ID of the chosen winner.
- winner is the address of the chosen winner.
The contract includes a constructor function that sets the owner to the address that deploys the contract.

The register function allows users to register for the lottery by sending 1 ETH. The function checks that the correct amount has been sent using a require statement. If the correct amount is sent, the function stores the sender's address in the participantsById mapping and increments the id.

The chooseWinner function can only be called by the contract owner and only when there are 10 participants. It chooses a winner by generating a pseudo-random number between 1 and 10 using the keccak256 hash of the current block timestamp and number. The function then sets the winnerId and winner variables and transfers the contract's balance to the winner.

However, it's important to note that the randomness used in this contract is not truly random and can be influenced by miners. This is a known security risk in Ethereum and could be exploited in a real-world lottery contract.