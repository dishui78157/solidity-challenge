# Solidity Challenge 4
*🔍 The following modifier is used to check if the call is made from an EOA or account.

 What's wrong in this? Comment down your reasons below. 👇*
 `// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
modifier CheckContract(address _address) {
    uint size;
    assembly {
        size := extcodesize(_address)
    }
    require(size == 0, "Only EOA");
    _;
}
 `
# Answer
The CheckContract modifier is designed to check if a given address is an Externally Owned Account (EOA) or a contract account. It does this by checking the size of the code at the given address using the extcodesize assembly function. If the size is 0, it assumes the address is an EOA, otherwise it assumes it's a contract.

However, there's a potential issue with this approach. It's possible for a contract to have a code size of 0, especially immediately after it's created and before its constructor has run. This is because the contract's code is not stored on the blockchain until after the constructor has finished executing.

Therefore, this modifier could incorrectly classify a contract as an EOA if it's checked at the wrong time. This could lead to unexpected behavior if your contract relies on this modifier for access control or other important functionality.

A safer approach might be to use the isContract function provided by OpenZeppelin's Address library, which checks if an address is a contract by attempting to retrieve a small amount of code from the address. If this operation reverts, it assumes the address is an EOA.