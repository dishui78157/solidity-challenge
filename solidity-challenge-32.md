# Calyptus Solidity Challenge 32
*Which function among emitNumber1() and emitNumber2() consumes less gas? Why?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract EmitNumbers {
    uint8 count = 10;
    event Number(uint8);

    function emitNumber1() external {
        for (uint8 i; i < count; i++) {
            emit Number(i);
        }
    }

    function emitNumber2() external {
        uint8 _count = count;
        for (uint8 i; i < _count; i++) {
            emit Number(i);
        }
    }
}
`
# Answer
emitNumber1() 
    transaction cost: 37246
    execution cost: 16182
emitNumber2()
    transaction cost: 35963
    execution cost: 14899

emitNumber2() consumes less gas because it uses a local variable _count to store the value of count. This reduces the number of times the contract needs to access the storage variable count, which is more expensive than accessing a local variable.