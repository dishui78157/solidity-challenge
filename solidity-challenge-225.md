solidity-challenge-225.md

# Calyptus Solidity Challenge 225
*
Presenting a smart contract tailored for a unique event ticket giveaway. 

The objective is to distribute complimentary tickets evenly among participants who sign up, but there could be a potential issue in the system 🚨

Can you identify it? 🔍*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract TicketGiveaway {
    mapping(address => bool) public participants;
    address[] public participantList;

    function signUpForTicket() external {
        participants[msg.sender] = true;
        participantList.push(msg.sender);
    }

    function isParticipantSignedUp(address participantAddress) external view returns (bool){
        return participants[participantAddress];
    }

    function getTotalParticipants() external view returns (uint256) {
        return participantList.length;
    }
 }
`

# Answer
The issue is that the contract is not checking whether the participant has already signed up for a ticket. as a result, a participant can sign up for multiple tickets.

*How can I prevent participants from signing up multiple times in the TicketGiveaway contract?*
We can prevent participants from signing up multiple times by adding a require statement in the signUpForTicket function to check if the participant has already signed up. If they have, the function should revert. Here's how we can do it:

`/ SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract TicketGiveaway {
    mapping(address => bool) public participants;
    address[] public participantList;

    function signUpForTicket() external {
        require(!participants[msg.sender], "You have already signed up!");
        participants[msg.sender] = true;
        participantList.push(msg.sender);
    }

    function isParticipantSignedUp(address participantAddress) external view returns (bool){
        return participants[participantAddress];
    }

    function getTotalParticipants() external view returns (uint256) {
        return participantList.length;
    }
}`

*How can I retrieve the list of participants who have signed up for the TicketGiveaway event?*
# answer
In Solidity, it's not possible to return a dynamic array of addresses in a public or external function due to limitations in the EVM. However, we can create a public getter function to retrieve the address of a participant at a specific index. Here's how we can do it:

`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract TicketGiveaway {
    mapping(address => bool) public participants;
    address[] public participantList;

    function signUpForTicket() external {
        require(!participants[msg.sender], "You have already signed up!");
        participants[msg.sender] = true;
        participantList.push(msg.sender);
    }

    function isParticipantSignedUp(address participantAddress) external view returns (bool){
        return participants[participantAddress];
    }

    function getTotalParticipants() external view returns (uint256) {
        return participantList.length;
    }

    function getParticipantAtIndex(uint256 index) external view returns (address) {
        require(index < participantList.length, "Index out of bounds");
        return participantList[index];
    }
}`

*How can I distribute the tickets evenly among the participants who have signed up for the TicketGiveaway event?*

# answer
To distribute tickets evenly among the participants, we would first need to know the total number of tickets available for distribution. Once we have that, we can divide the total number of tickets by the total number of participants to determine how many tickets each participant should receive.

Here's how we can add a function to distribute the tickets in the contract:
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract TicketGiveaway {
    mapping(address => bool) public participants;
    mapping(address => uint256) public tickets;
    address[] public participantList;
    uint256 public totalTickets;

    constructor(uint256 _totalTickets) {
        totalTickets = _totalTickets;
    }

    function signUpForTicket() external {
        require(!participants[msg.sender], "You have already signed up!");
        participants[msg.sender] = true;
        participantList.push(msg.sender);
    }

    function isParticipantSignedUp(address participantAddress) external view returns (bool){
        return participants[participantAddress];
    }

    function getTotalParticipants() external view returns (uint256) {
        return participantList.length;
    }

    function getParticipantAtIndex(uint256 index) external view returns (address) {
        require(index < participantList.length, "Index out of bounds");
        return participantList[index];
    }

    function distributeTickets() external {
        require(participantList.length > 0, "No participants to distribute tickets to");
        uint256 ticketsPerParticipant = totalTickets / participantList.length;
        for (uint256 i = 0; i < participantList.length; i++) {
            tickets[participantList[i]] = ticketsPerParticipant;
        }
    }
}`

*How can I randomly select a winner from the participants in the TicketGiveaway event?*
# answer
In Solidity, generating a truly random number is a complex task due to the deterministic nature of the blockchain. However, for a simple use case like this, we can use the block timestamp and block difficulty as a pseudo-random seed.

Here's how we can add a function to select a random winner:
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract TicketGiveaway {
    // ... existing code ...

    function selectWinner() external view returns (address) {
        require(participantList.length > 0, "No participants to select a winner from");
        uint256 randomIndex = uint256(keccak256(abi.encodePacked(block.timestamp, block.difficulty))) % participantList.length;
        return participantList[randomIndex];
    }
}`
In this updated version, the selectWinner function generates a pseudo-random index by hashing the current block timestamp and block difficulty, converting the hash to a uint256, and taking the modulus of the participant list length. It then returns the address at the randomly selected index in the participantList array.

Please note that this method does not generate a truly random number and should not be used for situations where a high degree of security and unpredictability is required. For such cases, consider using a verified random number generation service like Chainlink VRF.