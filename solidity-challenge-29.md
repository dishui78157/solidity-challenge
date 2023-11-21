# Calyptus Solidity Challenge 29
*Which of the following formats would you choose to increment the variable 'x' in Solidity?*
`
//
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract Test {
    uint public x = 10;
    function increment() public {
    x++;
}
function increment() public {
    x += 1;
}
function increment() public {
    x = x + 1;
}
}

`
# Answer

from remix ide:
x++; gas transaction cost: 26405
         execution cost: 5341
x += 1; gas transaction cost: 26418
            execution cost: 5354
x = x + 1; gas transaction cost: 26426
               execution cost: 5362
All three methods provided in the question will increment the variable x by 1. The choice between them is largely a matter of coding style and personal preference, as they will all have the same effect and similar gas costs.

x++: This is the post-increment operator. It's a common way to increment a variable in many programming languages. It's concise and widely recognized.

x += 1: This is an example of a compound assignment operator. It's also widely recognized and can be more readable to some people, especially when the increment is more than 1.

x = x + 1: This is the most explicit way to increment a variable. It clearly shows that the new value of x is the old value plus 1. However, it's more verbose than the other two methods.

In general, x++ or x += 1 are preferred for their conciseness, but x = x + 1 can be used if you prefer explicitness.