solidity-challenge-219.md

# Calyptus Solidity Challenge 219
*
Tell us if funds can be stolen from this CRUD smart-contract and how? 
*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

// This contract is intended to manage user profiles
contract UserProfileManager {
    struct UserProfile {
        string name;
        uint256 age;
    }

    mapping(address => UserProfile) private profiles;

    // Function to update the user profile. 
    function updateUserProfile(address profileUpdater) public {
        bytes memory func = abi.encodeWithSignature("crudProfile()");
        (bool success, ) = profileUpdater.delegatecall(func);
        require(success, "Failed to update profile");
    }
}
`
# Answer
The updateUserProfile function in the UserProfileManager contract is vulnerable to an attack because it uses the delegatecall function.

The delegatecall function allows an external contract to execute its code in the context of the calling contract. This means that the external contract has access to the storage, balance, and other properties of the calling contract.

If an attacker can control the profileUpdater address and the crudProfile function in the external contract, they can manipulate the storage of the UserProfileManager contract, potentially changing the state variables or even the contract's logic if the contract has a fallback function.

This could allow an attacker to steal funds if the contract holds any Ether or tokens, or perform other malicious actions.

To mitigate this risk, we should avoid using delegatecall with user-supplied addresses. If we need to interact with other contracts, consider using call or staticcall instead, or ensure that the address we're calling into is trusted and does not contain malicious code.