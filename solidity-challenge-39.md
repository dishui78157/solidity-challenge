# Calyptus Solidity Challenge 39
*The following smart contract allows anyone to be a member of a club by calling the register() function from their wallet address. It prohibits smart contracts from registering as members. How would you make a smart contract register itself as a member?*

`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract ClubMembers {
    uint public id;
    mapping(uint => address) public members;

    function isContract(address _address) public view returns (bool) {
        uint size;
        assembly {
            size := extcodesize(_address)
        }
        return size > 0;
    }

    function register() public {
        require(!isContract(msg.sender), "Contracts not allowed to register!");
        members[id] = msg.sender;
        id++;
    }
}`
# Answer
The register function in the ClubMembers contract uses the isContract function to check if the caller is a contract. If it is, the function reverts with the message "Contracts not allowed to register!". This prevents contracts from registering as members.

However, there is a way to bypass this restriction. The isContract function checks the code size of the address calling the function. If the code size is greater than 0, it assumes the caller is a contract. But this check is performed at the time the register function is called.

So, a contract can register itself as a member by creating another contract that calls the register function on its behalf. The new contract should be created in the same transaction as the call to register. This way, when isContract checks the code size of the new contract, it will be 0, because the new contract's code has not yet been stored.

Here's an example of how this could be done:
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract ClubMembers {
    // ... existing code ...

    function registerAsContract(address clubAddress) public {
        ClubMembers club = ClubMembers(clubAddress);
        club.register();
    }
}

contract RegisterContract {
    ClubMembers club;

    constructor(address clubAddress) {
        club = ClubMembers(clubAddress);
        club.register();
    }
}

contract ContractMember {
    function register(address clubAddress) public {
        new RegisterContract(clubAddress);
    }
}`

In this example, the ContractMember contract has a register function that creates a new RegisterContract. The RegisterContract constructor then calls the register function on the ClubMembers contract. Because the RegisterContract is created in the same transaction as the call to register, its code size will be 0 when isContract checks it, allowing it to register as a member.
