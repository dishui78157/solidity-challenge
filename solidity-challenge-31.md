# Calyptus Solidity Challenge 31

*Which function among transfer1() and transfer2() is less costly to deploy? why?*

`
function transfer1(uint amount) public {
    require(msg.sender == owner, "You are not the owner");
}

error NotOwner(address sender);

function transfer2(uint amount) public {
    if (msg.sender != owner) {
        revert NotOwner(msg.sender);
    }
}
`
# Answer
In Solidity, the gas cost of deploying a contract is mainly influenced by the bytecode size of the contract—larger contracts cost more to deploy. However, when comparing two functions like transfer1 and transfer2, the cost of deployment is generally not affected by the function's internal logic unless the logic significantly increases the contract's size.

Given the two functions transfer1 and transfer2:

transfer1 uses the require statement for input validation and error handling. It is the traditional way to revert transactions in Solidity if a condition is not met and provides an error message string.
transfer2 uses a custom error NotOwner, which was introduced in Solidity 0.8.4 as a way to save gas when reverting transactions with errors. Custom errors are cheaper than strings because they allow you to revert with less bytecode.
Custom errors like NotOwner are more gas-efficient at runtime because they don't require storing an error string in the contract's bytecode. Instead, they use an error selector, which is much smaller. However, when it comes to the deployment cost of the contract itself, both transfer1 and transfer2 will likely compile to bytecode of very similar size, and the difference in deployment cost will be negligible.

The main difference in gas cost will be observed when the revert condition is triggered. In that case, transfer2 using the custom error NotOwner will typically be less costly than transfer1's string error message.

It's also important to note that while custom errors save gas when they're used (because they avoid the need to store and return a string), the initial introduction of a custom error type in a contract will increase its size slightly. This is because the contract must include the error's signature. Still, this increase is minimal compared to the potential gas savings during the execution of revert conditions.

In summary, for the deployment cost, the difference between transfer1 and transfer2 is minimal and may not be the primary factor. However, transfer2 will likely be more gas-efficient in execution when the revert condition is hit due to the use of a custom error instead of a string for the error message.