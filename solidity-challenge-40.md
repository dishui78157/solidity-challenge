# Calyptus Solidity Challenge 40

*Which function among calculate1() and calculate2() consumes more gas? Why?*    
    `// SPDX-License-Identifier: MIT
    pragma solidity ^0.8.7;
    contract Contract1 {
        uint256 public result;

        function calculate1(uint256 a, uint256 b) public {
            result = a + b;
        }
    }

    contract Contract2 {
        uint256 public result;

        function calculate2(uint256 a, uint256 b) public  (uint256) {
            result = a * b;
        }
    }`

# Answer
calculate1(10,10)
transaction cost: 44214 gas
execution cost: 22870 gas

calculate2(10,10)
transaction cost: 44277 gas
execution cost: 22933 gas

The calculate2() function in Contract2 would consume more gas than calculate1() in Contract1.

Here's why:

Both calculate1() and calculate2() are storing the result of their respective operations in the state variable result. Writing to storage is one of the most gas-intensive operations in Solidity.

However, calculate2() performs a multiplication operation a * b, which consumes slightly more gas than the addition operation a + b performed by calculate1().

Therefore, calculate2() consumes more gas due to the additional gas cost of the multiplication operation.