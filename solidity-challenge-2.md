# Calyptus solidity challenge 2
*The following Solidity contract contains two functions foo and bar that perform the same task, but one of them uses more gas than the other. 

Identify the function that uses less gas and explain why. 🤔*
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Sum {
    function foo(uint[] memory data) public pure returns (uint) {
        uint sum = 0;
        for (uint i = 0; i < data.length; i++) {
            sum += data[i];
        }
        return sum;
    }

    function bar(uint[] memory data) public pure returns (uint) {
        uint sum = 0;
        for (uint i = 0; i < data.length; ++i) {
            sum += data[i];
        }
        return sum;
    }
}
`
# Answer
foo cost 2657 gas, bar cost 2664 gas, why?
the only difference between foo and bar is the increment operation in the for loop. foo uses i++ and bar uses ++i. In solidity, the pre-increment (++i) and the post-increment(i++) in a for loop have no significant difference in terms of gas usage when the result of the operation is not used in the same statement. 

Therefore both functions should use the same amount of gas.  
The little difference in gas usage might be due to other factors such as the state of the EVM at the time of execution, the gas price, or the length of the input array.  