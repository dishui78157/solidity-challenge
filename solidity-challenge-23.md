# Calyptus Solidity Challenge 23
*You've created this smart contract to save some of your money in it and withdraw it at least after 10 days. You used compiler version 0.7.0. Two days after depositing, you got an urgent need for money. Is there a way you can get your money right now?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.7.0;

contract Savings {
    uint public cutoff;

    function deposit() public payable {
        cutoff = block.timestamp + 10 days;
    }

    function extendCutoff(uint seconds) public {
        cutoff += seconds;
    }

    function withdraw() public {
        require(block.timestamp >= cutoff, "Too early");
        payable(msg.sender).transfer(address(this).balance);
    }
}
`
# Answer
1. @0xLawson Use extendCutoff with a value large enough to cause an overflow which would reset the cutoff variable to lower than block.timestamp and allow the withdraw function to pass the require statement. Would only be possible with solidity versions lower than 0.8.0.

2. @0xArav Since default math checking isnt a feature prior to 0.8.0 
We could Overflow the 'cutoff' using extendCutOff function  , so that it's value is lower than current block timestamp and then call withdraw function to get the money.

3. @moopidoopi
uint is short for uint256, which can store unsigned integers from 0 ~ 2^256 - 1. 

What happens when you exceed the max number? In solidity versions under 0.8, the number will wrap around and start counting from 0. (EX: uint 1 + 2^256 -1 = 0).

In 0.8, overflow/underflow checks are implemented in the compiler so it will throw an error if it happens!
