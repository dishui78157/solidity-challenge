# Calyptus Solidity Challenge 27
*What value does the function test() return? Explain your answer.*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Test {
    uint public x = 10;
    uint public y = 12;

    function test() public view returns(uint) {
        return (x & y ) ^ (x | y);
    }
}
`