# Calyptus Solidity Challenge 26
*Which of the following formats would you prefer to loop through in Solidity and why?*
`
for (uint i = 0; i <10; i++)
for (uint i = 0; i < 10; ++i)
`

# Answer
In Solidity, the difference in gas cost between pre-increment (++i) and post-increment (i++) operations is due to how they are implemented at the EVM (Ethereum Virtual Machine) level.

The post-increment (i++) operation involves more steps: it first makes a copy of the variable, then increments the original variable, and finally returns the copy. This is because i++ returns the value of i before the increment.

On the other hand, the pre-increment (++i) operation simply increments the variable and returns the result, which is more efficient.

However, in a loop condition like for (uint i = 0; i < 10; i++) or for (uint i = 0; i < 10; ++i), the returned value is not used. Therefore, the Solidity compiler optimizes both i++ and ++i to the same code, and they end up costing the same amount of gas.

In other words, while ++i is theoretically more efficient than i++, in practice, you won't see a difference in gas cost in this context due to compiler optimizations.