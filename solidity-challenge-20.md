# Calyptus Solidity Challenge 20
*You're not the current owner of this contract. And the current owner doesn't want to leave the ownership. Is there a way for you to become its owner? How would you do it?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Owner {
    address public owner;

    constructor() {
        owner = msg.sender;
    }

    function changeOwner(address _address) public {
        require(tx.origin == owner);
        owner = _address;
    }
}
`
# Answer
1. If you can get the owner to sign a tx on your malicious proxy contract that calls changeOwner, you can set your own address as owner.  Note administration functions should almost always have the onlyOwner modifier

2. ontract uses tx.origin rather than msg.sender so you can phish him and get him to sign something malicious that leads to your contract taking ownership.

3. Create a token contract imitating a high value NFT or ERC20 and mint the counterfeit token to their address.

Hide the call to changeOwner in a "safe" interaction like transfer.

DM the owner to ask to transfer token back. Maybe claim mistaken tx etc.

Stay safe out there anon 😬

4. phishing:  contract require tx origin to be owner. 
let the owner sign a malicious proxy and call changeOwner

-------
The contract Owner has a function changeOwner that allows the current owner to change the owner of the contract. However, this function has a require statement that checks if tx.origin is the current owner. tx.origin is the original address that started the transaction, which means that only the current owner can call this function and change the ownership.

If you're not the current owner and the current owner doesn't want to leave the ownership, there's no direct way for you to become the owner of this contract under normal circumstances.

However, there's a potential vulnerability in the contract due to the use of tx.origin. If the current owner interacts with a malicious contract, that contract could call the changeOwner function and change the owner to an address controlled by the attacker. Here's an example of how this could be done:
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Malicious {
    Owner ownerContract;

    constructor(Owner _ownerContract) {
        ownerContract = _ownerContract;
    }

    function attack() public {
        ownerContract.changeOwner(msg.sender);
    }
}
`
In this contract, you would set the Owner contract address in the constructor. Then, when the current owner interacts with the Malicious contract (for example, by calling the attack function), the Malicious contract calls the changeOwner function of the Owner contract. Since tx.origin in the changeOwner function call will be the address of the current owner, the require check will pass and the owner will be changed to the address provided in the changeOwner function call (in this case, the address of the attacker).

Please note that this is a theoretical attack and would require the current owner to interact with the Malicious contract, which they may not do if they're aware of the potential risks. Also, such an action would be illegal and unethical.