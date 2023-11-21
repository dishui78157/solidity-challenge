# Calyptus Solidity Challenge 28
*In what slot is the variable 'isPassed' stored? How would you re-arrange these variables in an ideal scenario? Explain.*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Result {
    uint16 public rollNum;
    string public name;
    address public account;
    string[5] public subjects;
    uint16 public totalMarks;
    bool public isPassed;
}
`
# Answer
In Solidity, the storage layout of state variables is laid out linearly from slot 0 and upwards. Each slot is 32 bytes. Small types like uint16 and bool only take up a portion of a storage slot, and Solidity will try to pack these small types into a single slot if possible.

In the given contract, the variable isPassed is stored in the 4th slot. Here's the breakdown:

- rollNum (2 bytes) and totalMarks (2 bytes) are packed into the first slot (4 bytes total, 28 bytes still free).
- name is a dynamic type and takes up the second slot.
- account (20 bytes) takes up the third slot.
- subjects is an array and takes up the fourth slot.
- isPassed (1 byte) is stored in the fifth slot.

In an ideal scenario, you would want to arrange the variables to minimize the storage space used. This can be achieved by placing small types together so they can be packed into a single slot. Here's a possible arrangement:
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Result {
    uint16 public rollNum;
    uint16 public totalMarks;
    bool public isPassed;
    address public account;
    string public name;
    string[5] public subjects;
}`
In this arrangement, rollNum, totalMarks, and isPassed are packed into the first slot. account takes up the second slot, name takes up the third slot, and subjects takes up the fourth slot. This arrangement uses one less slot than the original.
