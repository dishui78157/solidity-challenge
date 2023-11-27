# Calyptus Solidity Challenge 33
*
If we call the functions 'changeNum1' and 'changeNum2' by passing the number 5 as an argument, what will be the new values of variables 'num1' and 'num2'?*

`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Number {
    uint num1 = 1;
    uint num2 = 1;

    function changeNum1(uint _num1) public {
        require(_num1 > 10);
        num1 = _num1;
    }

    function changeNum2(uint _num2) public {
        num2 = _num2;
        require(_num2 > 10);
    }
}
`
# Answer
num1 = 1
num2 = 1
however changeNum2 is  more expensive because it has to store the value of _num2 in storage before it can check the require condition.