# Calyptus Solidity Challenge 24
*The following smart contract intends to calculate the sum of the first 200,000 integers. Is there something wrong with this contract? Explain your answer.*

`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract SumOfNumbers {
    uint public num;

    function add() public {
        for(uint i = 0; i < 200000; i++) {
            num += i;
        }
    }
}
`

# Answer
Yes, there is a problem with this contract. 
1. very costly to run, Due to the number of reads and writes to storage, the contract could run out of gas.
2. Gas limit is 30 million, because it writes to storage it will run out of gas.
3. Unoptimized and too expensive. Each SLOAD costs at least 100 gas, each SSTORE at least 2900 gas. Total gas usage > 30,000,000 gas, which is the block limit. 

On remix:
transaction cost	93443138 gas 
execution cost	93422074 gas 

The issue lies in the add function. It attempts to calculate the sum of the first 200,000 integers using a loop. In Ethereum, each operation costs a certain amount of gas, and there is a limit to the amount of gas that can be used in a single transaction (the block gas limit).

The loop in the add function will consume a large amount of gas, especially as i approaches 200,000. It's highly likely that the gas consumed by this function will exceed the block gas limit, causing the transaction to fail.

A better approach would be to calculate the sum of the first N integers using the formula N * (N + 1) / 2. This formula calculates the sum directly, without the need for a loop, and will consume significantly less gas:
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract SumOfNumbers {
    uint public num;

    function add(uint n) public {
        num = n * (n + 1) / 2;
    }
}
`
on remix:
transaction cost	44375 gas 
execution cost	23147 gas 

In this revised contract, you would call the add function with the argument 200,000 to calculate the sum of the first 200,000 integers.
