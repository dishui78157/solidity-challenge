# Calyptus solidity challenge 5
In which slot is password stored? Start counting from slot 0. explain your calculation. 
Slot 0?
Slot 1?
Slot 2?
Slot 3?
Slot 4?
Slot 5?
Slot 6?*

`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract FindSlot {
    uint16 public tokenId;
    address public admin;
    uint128 public saleStart;
    bool public saleStarted;
    uint16 public totalSupply;
    address [2] public buyers;
    byte32 private password;
    bool public isPaused;
}
`
# Answer
In Solidity, the storage layout of a contract is organized by slots. Each slot is 32 bytes. Variables are packed tightly, from right to left, into storage slots.

Here's how the storage slots would be allocated for your contract:

- Slot 0: tokenId (2 bytes), saleStart (16 bytes), totalSupply (2 bytes), saleStarted (1 byte), isPaused (1 byte). These variables fit into one slot because they total 22 bytes, which is less than the 32 bytes per slot. The remaining 10 bytes in this slot are unused.

- Slot 1: admin (20 bytes). Address types take up 20 bytes, so admin gets its own slot.

- Slot 2: buyers[0] (20 bytes). The first element of the buyers array gets its own slot.

- Slot 3: buyers[1] (20 bytes). The second element of the buyers array gets its own slot.

- Slot 4: password (32 bytes). The password variable is a byte32, which takes up a full slot.

So, the password variable is stored in slot 4.
