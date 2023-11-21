# Calyptus Solidity Challenge 30
*Which function among add1() and add2() is more gas efficient? why?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Sum {
    uint public sum;

    function add1(uint[] memory numbers) external {
        for (uint i = 0; i < numbers.length; i++) {
            sum += numbers[i];
        }
    }

    function add2(uint[] calldata numbers) external {
        for (uint i = 0; i < numbers.length; i++) {
            sum += numbers[i];
        }
    }
}
`
# Answer
add1([10,10]) 
    transaction cost: 45933
    execution cost: 24309
add2([10,10]) 
    transaction cost: 45079
    execution cost: 23455


modify the code:
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Sum {
    uint public sum;

    function add1(uint[] memory numbers) external {
        uint temp;
        for (uint i = 0; i < numbers.length; i++) {
            temp += numbers[i];
        }
        sum = temp;
    }

    function add2(uint[] calldata numbers) external {
        uint temp;
        for (uint i = 0; i < numbers.length; i++) {
            temp += numbers[i];
        }
        sum = temp;
    }
}
`

add1([10,10]) 
    transaction cost: 45722
    execution cost: 24098
add2([10,10]) 
    transaction cost: 44868
    execution cost: 23244
is memory more gas efficient than calldata in solidity?

In Solidity, the Ethereum smart contract programming language, memory and calldata are two distinct data location options that indicate where the data will be stored.

- memory is a temporary place to store data. It's mutable and erased between external function calls to your contract. Memory is generally used for variables that are created within a function and it is more costly in terms of gas to write to, but reading from memory is cheaper than reading from storage.

- calldata is an immutable, temporary location used to store function arguments. It is often cheaper than memory because it’s read-only and passed as a reference type in function calls of external visibility. It doesn't require the extra step of copying the data to memory before use, which saves gas when you're just passing arguments to functions.

In terms of gas efficiency:

Reading from calldata is cheaper than reading from memory.
Writing data is only possible to memory (since calldata is read-only), so comparing writing costs doesn't apply.
When you're deciding whether to use memory or calldata, consider the following:

For read-only data that doesn't need to be modified, passed as an argument to external functions, calldata is the most gas-efficient.
If you need to modify the data or it's an internal function that doesn't externalize the data, you'll use memory.
It's also worth mentioning storage, which is the most gas-intensive data location due to its persistent nature on the blockchain. Always minimize the use of storage for writing operations to ensure gas efficiency.

In summary, use calldata for read-only external data to save gas, and use memory for temporary, mutable data within function calls.





