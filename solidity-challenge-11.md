# Calyptus Solidity Challenge 11
*Which of these functions consumes less gas? double(), or shiftLeft() ?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract Test {
    function double(uint8 x) public pure returns (uint8) {
        return x * 2;
    }

    function shiftLeft(uint8 x) public pure returns (uint8) {
        return x << 1;
    }
}
`
# Answer
The shiftLeft function consumes less gas.
The double function consumes 832 gas.
The shiftLeft function consumes 620 gas. 


In Solidity, the gas cost of basic operations like multiplication (*) and bit shifting (<<) is the same. Both operations have a gas cost of 3 gas according to the Ethereum Yellow Paper.

So, in the above contract, the double and shiftLeft functions should consume approximately the same amount of gas, assuming the input x is the same for both functions.

However, the exact gas cost of a function call in Solidity can depend on other factors as well, such as the current state of the EVM, the gas price, and the amount of data being processed.

It's always a good idea to test functions using a tool like the Remix IDE to get a more accurate estimate of gas usage.