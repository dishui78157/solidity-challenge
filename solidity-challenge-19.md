# Calyptus Solidity Challenge 19
*The following contract doesn't have a payable, receive or fallback function. Is there a way to send some ether to this contract? Explain.*

`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract Test {
    function getBalance() public view returns(uint) {
        return address(this).balance;
    }
}
`

# Answer
Yes, there is a way to send Ether to this contract.
In Ethereum, a contract can receive Ether in three ways:

1. By having a function marked as payable and calling that function with some Ether.
2. By having a receive or fallback function, which gets called when the contract is sent Ether without data (like a simple send or transfer).
3. As a result of selfdestruct called from another contract. When a contract is destroyed using the selfdestruct function, it can send its remaining Ether to another address.

The provided Test contract does not have a payable function or a receive or fallback function. Therefore, it cannot receive Ether through methods 1 and 2.

However, it could still receive Ether through method 3. If another contract that holds Ether calls selfdestruct(address(this)), the remaining Ether in that contract would be sent to the Test contract, even though the Test contract does not have a payable, receive, or fallback function.

Please note that using selfdestruct is a serious action that permanently destroys a contract and should be used with caution.