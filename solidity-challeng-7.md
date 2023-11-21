# Calyptus Solidity Challeng 7
*If we call the functions 'changeNum1' and 'changeNum2' by passing the number 5 as an argument, what will be the new values of variables 'num1' and 'num2'?*

``/ SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract Number {
    uint num1 = 1;
    uint num2 = 1;

    function changeNum1(uint _num1) public {
        require(_num1 > 10);
        num1 =  _num1;
    }

    function changeNum2(uint _num2) public {
        num2 =  _num2;
        require(_num2 > 10);
    }
}
`
# Answer
If you call the functions changeNum1 and changeNum2 by passing the number 5 as an argument, the new values of variables num1 and num2 will not change from their initial values of 1.

Here's why:

- When you call changeNum1(5), the require(_num1 > 10) statement will cause the function to revert because 5 is not greater than 10. So num1 will remain 1.

- When you call changeNum2(5), the num2 = _num2 statement will execute first, changing num2 to 5. But then the require(_num2 > 10) statement will cause the function to revert because 5 is not greater than 10. When a function reverts, all changes to the state are undone, so num2 will go back to its original value of 1.

So, after calling both functions with an argument of 5, num1 and num2 will both still be 1.