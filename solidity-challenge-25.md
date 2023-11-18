# Calyptus Solidity Challenge 25
*
The following smart contracts perform the same function of adding two numbers. Sum1 was compiled with 200 runs optimisation and Sum2 with 10000 runs optimisation. Which of these two would cost less to deploy? Why?
*

`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Sum1 {

    uint a = 1000;
    uint b = 2000;
    uint sum;

    function add() public {
        sum = a + b;
    }
}

contract Sum2 {

    uint a = 1000;
    uint b = 2000;
    uint sum;

    function add() public {
        sum = a + b;
    }
}
`

# Answer
The cost of deploying a contract on the Ethereum network is determined by the size of the contract (in bytes) and the gas price at the time of deployment. The more complex the contract, the larger its size, and the more it costs to deploy.

In this case, both Sum1 and Sum2 contracts are identical in terms of their code. The only difference is the compiler optimization setting used when compiling them. Compiler optimization aims to reduce the gas cost of executing contract functions.  

More optimization runs will increase the overall compiled bytecode size costing more gas while deploying but cheaper calls.  

Theoretically one should set the runs number as the number of times you expect the function to be called in production. (?)

Higher number of runs leads to bigger bytecode, hence higher deploy cost, but cheaper function calls. 