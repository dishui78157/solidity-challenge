# Calyptus Solidity Challenge 10
*What does C.test() return?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract A {
    function test() virtual public pure returns (uint) {
        return 123;
    }
}

contract B  {
    function test() virtual public pure returns (uint) {
        return 456;
    }
}

contract C is A, B {
    function test() override(A, B) public pure returns (uint) {
        return super.test();
    }
}
`
# Answer
The test function returns 123.
The override keyword is used to override the test function in contract C.
The super keyword is used to call the test function in contract A.
the inheritance order is from right to left, so it call B, then A. 