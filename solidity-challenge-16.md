# Calyptus Solidity Challenge 16
*The following two contracts have similar functions to check if a number is within the range of 0-99. Which of these is more gas efficient?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract ContractOne {
    function Test(uint num) public pure returns(bool) {
        return num <= 99;
    }
}

contract ContractTwo {
    function Test(uint num) public pure returns(bool) {
        return num < 100;
    }
}
`

# Answer
ContractTwo is more gas efficient.

since the Test function in contractTwo has only to use one opscode <, Test in contractOne has to use two opscode < and ISZERO
