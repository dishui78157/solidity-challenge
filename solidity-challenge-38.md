# Calyptus Solidity Challenge 38
*
Identify and fix the issues in the following smart contract.
*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract Bank {
    mapping(address => uint) public balance;

    function deposit() public payable {
        balance[msg.sender] += msg.value;
    }

    function withdraw(uint amount) public {
        require(balance[msg.sender] >= amount, "insufficient balance");
        balance[msg.sender] -= amount;
        payable(msg.sender).transfer(amount);
    }
}
`
# Answer
The smart contract is a simple bank contract that allows users to deposit and withdraw Ether. However, it has a potential security vulnerability known as re-entrancy attack.

In the withdraw function, the balance of the sender is updated before the transfer is made. If the call to transfer fails for any reason (for example, if the recipient contract throws an exception), the balance of the sender will have been decreased, but the transfer of Ether will not have occurred. This could lead to funds being locked in the contract.

To fix this, you should perform the transfer first, and then update the balance. Here's the corrected contract:
`// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract Bank {
    mapping(address => uint) public balance;

    function deposit() public payable {
        balance[msg.sender] += msg.value;
    }

    function withdraw(uint amount) public {
        require(balance[msg.sender] >= amount, "Insufficient balance");
        (bool success, ) = payable(msg.sender).call{value: amount}("");
        require(success, "Transfer failed");
        balance[msg.sender] -= amount;
    }
}`

In this corrected version, the withdraw function uses a low-level .call instead of .transfer to send Ether. This is because .transfer only forwards 2300 gas, which may not be enough if the recipient is a contract that performs operations in its fallback function. The .call method forwards all available gas. After the transfer, it checks whether the transfer was successful, and then updates the balance. This order of operations mitigates the risk of a re-entrancy attack.