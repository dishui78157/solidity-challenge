#  Calyptus solidity challenge 3
*Can you spot the issue in this Solidity implementation? 🔎*
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract ChallengeContract {
    uint256 public counter;
    function increment() public {
        counter = counter + 1;
        require(counter > 0, "Counter must be positive");
    }
}
`
# Answer
1. the require statement should be before the counter increment. since it can overflow and become zero.
2. the counter is of type uint256 so its always positive. The require statement is redundant.
