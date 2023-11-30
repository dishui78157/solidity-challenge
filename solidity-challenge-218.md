solidity-challenge-218.md

# Calyptus Solidity Challenge 218

*
Consider a scenario where Contract A is utilized to handle basic data entry tasks, like capturing a user's age 👨‍💻

Identify the shortcomings in the AgeUpdater contract and suggest a preferred alternative update function👇
*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract UserAgeManager {
    uint256 public userAge;

    // Sets the age of a user.  Age must be greater than 0 and less than 150.
    function setUserAge(uint256 _age) external {
        require(_age > 0 && _age < 150, "Age must be between 1 and 149");
        userAge = _age;
    }
}

// Interface for the UserAgeManager contract. 
interface IUserAgeManager {
    function setUserAge(uint256 _age) external;
}

// Contract B is designed to interact with UserAgeManager.
contract AgeUpdater {
    // Updates user age using the interface method. 

    function updateUserAgeV1(IUserAgeManager ageManager, uint256 _age) external {
        ageManager.setUserAge(_age);
    }

    // Updates user age using low-level call. 
    function updateUserAgeV2(address ageManager, uint256 _age) external {
        (bool success, ) = ageManager.call(abi.encodeWithSignature("setUserAge(uint256)", _age));
    }
}
`
# Answer
The AgeUpdater contract has a couple of potential issues:

accessor.eth
@ioevno
·
Nov 21
place the interface in a separate file so the functions are not calling externals internally.
Quote Tweet
Calyptus
@CalyptusFinance
· Nov 24
Exactly!! Otherwise the call can fail silently!

1. No Access Control: Both updateUserAgeV1 and updateUserAgeV2 functions are external and do not have any access control mechanisms. This means any address can call these functions and update the user's age. This could be a problem if only specific addresses (like the user themselves or an admin) should be able to update the age.

2. Low-Level Call: The updateUserAgeV2 function uses a low-level .call() function to interact with the UserAgeManager contract. This is generally discouraged because it can lead to unexpected behavior if the called function reverts or if the function signature is incorrect.

Here's a revised version of the AgeUpdater contract with an access control mechanism and without the low-level call:
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract UserAgeManager {
    uint256 public userAge;
    address public user;

    constructor(address _user) {
        user = _user;
    }

    function setUserAge(uint256 _age) external {
        require(msg.sender == user, "Only the user can update their age");
        require(_age > 0 && _age < 150, "Age must be between 1 and 149");
        userAge = _age;
    }
}

interface IUserAgeManager {
    function setUserAge(uint256 _age) external;
}

contract AgeUpdater {
    function updateUserAge(IUserAgeManager ageManager, uint256 _age) external {
        ageManager.setUserAge(_age);
    }
}
`
In this revised version, the UserAgeManager contract now has a user state variable, and the setUserAge function checks that the caller is the user before updating the age. The AgeUpdater contract now only has one function to update the age, which calls the setUserAge function through the IUserAgeManager interface.
