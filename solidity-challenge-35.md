# Calyptus Solidity Challenge 35
*How many storage slots would this struct use once initialised? Explain your calculation.*

`
struct Transaction {
    address receiver;
    uint8 id;
    bool isApproved;
    uint64 amount;
}
`

# Answer
Looking at the Transaction struct:

address receiver: An address in Solidity is 20 bytes.
uint8 id: A uint8 is 1 byte.
bool isApproved: A bool is 1 byte.
uint64 amount: A uint64 is 8 bytes.

since address uses one storage slot, and the rest of the variables are less than 32 bytes, the struct will use 2 storage slots.

*does the address type share storage slot in solidity?*
#   Answer
In Solidity, the address type does not share a storage slot with other variables if it's the first or only variable in a struct. This is because the address type is 20 bytes, which is more than half of the EVM's 32-byte word size.

However, if an address variable is not the first variable in a struct, and the sum of the sizes of the preceding variables in the struct is less than or equal to 12 bytes (which is 32 bytes - 20 bytes), then the address variable will share a storage slot with those preceding variables.

In the case of the Transaction struct:

`struct Transaction {
    address receiver;
    uint8 id;
    bool isApproved;
    uint64 amount;
}`

The address receiver is the first variable in the struct, so it will not share a storage slot with the other variables. The remaining variables (uint8 id, bool isApproved, uint64 amount) take up 10 bytes in total, which is less than 12 bytes, so they will all be packed into the next storage slot.

Therefore, the Transaction struct will use 2 storage slots in total.

otherwise, the answer is 1 storage slot. as below:


In Solidity, the Ethereum Virtual Machine (EVM) has a word size of 32 bytes, and each word is stored in a separate storage slot. However, Solidity tries to pack smaller data types into a single storage slot to save storage space.

Looking at the Transaction struct:

address receiver: An address in Solidity is 20 bytes.
uint8 id: A uint8 is 1 byte.
bool isApproved: A bool is 1 byte.
uint64 amount: A uint64 is 8 bytes.
Adding these up, we get 30 bytes, which is less than the EVM word size of 32 bytes. Therefore, Solidity will pack all of these variables into a single storage slot.

So, the Transaction struct will use 1 storage slot once initialized.