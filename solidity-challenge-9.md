# Calyptus Solidity Challenge 9
*How many storage slots would this struct use as a state variable? Explain your calculation.*

`struct Transaction {
    address receiver;
    uint8 id;
    bool isApproved;
    uint64 amount;
}`

# Answer
In Solidity, the storage is organized in slots. Each slot is 32 bytes. Variables are packed together as tightly as possible, with the condition that each variable must start at a new slot if it cannot fully fit into the remaining part of the current slot.

Here's how the Transaction struct would be stored:

- receiver: This is an address type, which takes up 20 bytes. It will take up one slot because it can't share a slot with other variables (even though it doesn't fully occupy a slot).

- id: This is a uint8 type, which takes up 1 byte.

- isApproved: This is a bool type, which also takes up 1 byte.

- amount: This is a uint64 type, which takes up 8 bytes.

The id, isApproved, and amount variables together take up 10 bytes, so they can all fit into one slot.

So in total, the Transaction struct would use 2 storage slots.