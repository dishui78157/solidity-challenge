# Calyptus Solidity Challenge 8
*What value does the function test() return? Explain your calculation.*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Test {
    uint public x = 10;
    uint public y = 12;

    function test() public view returns (uint) {
    
        return (x & y) ^ (x | y );
    }

}`

# Answer
The test function returns 6.
x & y is 10 & 12, which is 8.
x | y is 10 | 12, which is 14.
8 ^ 14 is 6.
