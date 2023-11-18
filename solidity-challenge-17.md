# Calyptus Solidity Challenge 17
*The following example can't be exploited following a basic re-entrancy approach. Why is this so? Can you think of any option to leverage this scenario?*

`
function withdraw(uint amount) external {
    require(amount > 0, "Can't withdraw 0");
    require(balance[msg.sender] >= amount, "Not enough funds");
    (bool success, ) = payable(msg.sender).call{value: amount}("");
    require(success, "Low level call failed. Failed to withdraw Ether");
    balance[msg.sender] -= amount;
}
`



# Answer
The provided withdraw function is resistant to a basic re-entrancy attack because it correctly follows the Checks-Effects-Interactions pattern, which is a best practice in smart contract development to prevent re-entrancy attacks.

Here's how the function works:
1. Checks: The function first checks if the withdrawal amount is greater than 0 and if the sender has enough balance. These checks are done using require statements.

2. Effects: After the checks, the function updates the state of the contract. However, in this case, the state update (balance[msg.sender] -= amount;) is done after the interaction with an external contract, which is not the best practice. The state should be updated before the external call to prevent re-entrancy attacks.

3. Interactions: The function then interacts with an external contract (in this case, the contract at the address of the sender) by sending it Ether. This is done using a low-level .call function.

The reason this function is resistant to a basic re-entrancy attack is that the balance of the sender is updated after the external call. If a re-entrancy attack were to occur (i.e., the called contract calls back into the withdraw function), the balance of the attacker would already be reduced by the amount they are trying to withdraw, so the require(balance[msg.sender] >= amount, "Not enough funds"); check would fail, preventing the re-entrant call from withdrawing more funds.

However, it's important to note that while this function is resistant to a basic re-entrancy attack, it's generally safer to perform all state changes before making any external calls. This is because there might be other vulnerabilities or attack vectors that could be exploited if state changes are done after external calls.

-------
what is a basic re-entrancy approach?
A re-entrancy attack is a common vulnerability in Ethereum smart contracts. It occurs when a contract calls an external contract (or sends Ether to an external address) before it has finished executing. This can allow the external contract to call back into the original contract and modify its state before the original function has finished executing.

Here's a basic example of how a re-entrancy attack might work:

1. Contract A calls a function in Contract B and sends it some Ether.
2. Contract B has a fallback function that is triggered when it receives Ether. This function calls back into Contract A before  Contract A has finished executing.
3. Contract A's state is modified by the call from Contract B.
4. When control returns to Contract A, it continues executing with its state in an unexpected condition, potentially leading to unintended behavior.

A basic re-entrancy approach would involve creating a malicious contract (like Contract B in the example above) that takes advantage of a vulnerable contract by calling back into it during a function execution.

To prevent re-entrancy attacks, contracts should follow the Checks-Effects-Interactions pattern, where they perform any external calls or sends last, after all internal work and state changes have been completed.

