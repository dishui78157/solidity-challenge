# Calyptus Solidity Challenge 36

*What does C.test() return?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract A {
    function test() virtual public pure returns (uint) {
        return 123;
    }
}

contract B {
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
The C.test() function in this Solidity code will return 456.

Here's why:

The C contract is inheriting from both A and B contracts. Both A and B have a function named test(). In contract C, the test() function is overriding the test() functions from both A and B. Inside C's test() function, it's calling super.test(), which refers to the immediate parent contract's test() function.

In Solidity, the order of inheritance is from right to left, so B is the immediate parent of C in this case. Therefore, super.test() will call B.test(), which returns 456. Hence, C.test() will return 456.