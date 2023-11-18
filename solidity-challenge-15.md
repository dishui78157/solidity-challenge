# Calyptus Solidity Challenge 15
*The following smart contract allows anyone (except a smart contract) to be a member of a club by calling the register() function from their wallet address. How would you make a smart contract register itself as a member?*

`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract ClubMembers {
    uint public id;
    mapping(uint => address) public members;

    function isContract(address _address) public view returns(bool) {
        uint size;
        assembly { size := extcodesize(_address) }
        return size > 0;
    }

    function register() public {
        require(!isContract(msg.sender), "Smart contracts cannot register!");
        members[id] = msg.sender;
        id++;
    }
}
`
# Answer
1. Call register function in your constructor, and since code hasn't been deployed yet, the check for codesize will be 0
2. Calling the register function from inside another contract's constructor would bypass the isContract check.

The ClubMembers contract uses the isContract function to check if the msg.sender is a contract. If it is, the register function reverts with the message "Smart contracts cannot register!". This is done by checking the extcodesize of the msg.sender address in assembly, which returns the size of the code at that address. If the size is greater than 0, it means the address is a contract.

To bypass this check and register a contract as a member, you could use a contract that uses a fallback function to call the register function. The fallback function is executed when the contract receives Ether without data (like a simple send or transfer).

Here's an example of how you could do this:
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract RegisterContract {
    ClubMembers clubMembers;

    constructor(ClubMembers _clubMembers) {
        clubMembers = _clubMembers;
    }

    fallback() external {
        clubMembers.register();
    }
}`
In this contract, you would set the ClubMembers contract address in the constructor. Then, when you send Ether to the RegisterContract without data, it will call the register function of the ClubMembers contract in its fallback function.

Since the msg.sender of the register function call will be the address of the RegisterContract, and the isContract check is bypassed because the register function is not directly called by the RegisterContract, the RegisterContract will be registered as a member.

Please note that this is a theoretical workaround and may not work in all cases, especially if the ClubMembers contract has additional checks or requirements.
