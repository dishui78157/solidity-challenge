# Calyptus solidity challenge 1 
*Where is the vulnerability in this code?*
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Owner {
    address public owner;
    uint private code;

    constructor(address _owner, uint _code) {
        owner = _owner;
        code = _code;
    }

    function getOwner() public view returns(address) {
        return owner;
    }


    function changeOwner(address _owner, uint _code) public  {
        require(code == _code, "Please enter the correct code!");
        owner = _owner;
    }
}`
# Answer
The vulnerability in this Solidity contract lies in the changeOwner function. This function is public and does not check if the caller of the function is the current owner. This means that anyone who knows the code can change the owner of the contract, not just the current owner.

Here's a more secure version of the contract:
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Owner {
    address public owner;
    uint private code;

    constructor(address _owner, uint _code) {
        owner = _owner;
        code = _code;
    }

    function getOwner() public view returns(address) {
        return owner;
    }

    function changeOwner(address _owner, uint _code) public  {
        require(msg.sender == owner, "Only the current owner can change the owner");
        require(code == _code, "Please enter the correct code!");
        owner = _owner;
    }
}
`