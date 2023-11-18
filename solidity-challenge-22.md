# Calyptus Solidity Challenge 22
*
If you execute the functions step1, step2 and step3 in respective order, arrange these functions in descending order with respect to the gas consumed by each. Explain your answer.
*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract Number {
    uint public number;

    function step1() public {
        number =  1;
    }

    function step2() public {
        number = 2;
    }

    function step3() public {
        number = 0;
    }
}
`

# Answer
on remix
gas used by step1:  transaction cost: 43342, execution cost: 22278
gas used by step2:  transaction cost: 26220, execution cost: 5156
gas used by step3:  transaction cost: 21464, execution cost: 5200

The gas consumption for the functions step1, step2, and step3 would be as follows in descending order:

step1
step2 and step3 (equal gas consumption)
Explanation:

1. step1: This function sets the number state variable to 1. Since the initial value of number is 0 (the default value for uint in Solidity), this operation changes the value from 0 to a non-zero value. In Ethereum, setting a storage value from 0 to non-zero consumes more gas (20,000 gas units) due to the "SSTORE" opcode cost.

2. step2 and step3: Both these functions change the number state variable from a non-zero value to another value. In Ethereum, changing a storage value from non-zero to non-zero or non-zero to zero consumes less gas (5,000 gas units) compared to changing from zero to non-zero. Therefore, step2 and step3 consume equal and less gas compared to step1.

Please note that these are the base costs and the actual gas costs would be slightly higher due to the additional gas costs for the transaction and execution overhead.