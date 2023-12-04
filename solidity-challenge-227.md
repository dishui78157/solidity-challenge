solidity-challenge-227.md

# Calyptus Solidity Challenge 227
*
Calling all #Blockchain detectives! A challenge awaits in this crypticTreasureContract. Can you identify a potential flaw?
*

`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
// Cryptic Treasure Contract

contract CrypticTreasure {
    bytes32 public constant treasureCode = 0x4d3; // a 64bit hash of the treasure code
    // the encrupted code for the treasure
    uint256 public constant treasureValue = 10 ether;
    // The amount of Ether to be awarded for cracking the code

    constructor() payable {}

    // Function to unlock the treasure by providing the correct code

    function unlockTreasure(string memory code) public {
        require(treasureCode == keccak256(abi.encodePacked(code)), "Incorrect code provided");

        // transfer the treasure to the codebreaker
        (bool sent,) = msg.sender.call{value: treasureValue}("");
        require(sent, "Failed to send Ether");
       
    }
}
`
# Answer
The unlockTreasure function has a couple of potential issues:
1. Re-Entrancy Vulnerability: The contract is vulnerable to a re-entrancy attack because it calls an external contract (with msg.sender.call) before it finishes its state changes. An attacker could recursively call unlockTreasure before the first call is finished, potentially draining the contract's Ether.
2. No Balance check: The contract sends treasureValue (10 ether) to msg.sender without checking if the contract has enough balance.  If the contract's balance is less than treasureValue, this will cause a revert. 

Here's a revised version of the contract that fixes these issues:
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract CrypticTreasure {
    bytes32 public constant treasureCode = 0x4d3;
    uint256 public constant treasureValue = 10 ether;

    constructor() payable {}

    function unlockTreasure(string memory code) public {
        require(keccak256(abi.encodePacked(code)) == treasureCode, "Incorrect code provided");
        require(address(this).balance >= treasureValue, "Not enough Ether in contract");

        // Use transfer instead of call to prevent re-entrancy
        payable(msg.sender).transfer(treasureValue);
    }
}
`
