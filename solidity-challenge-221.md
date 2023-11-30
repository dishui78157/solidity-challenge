solidity-challenge-221.md

# Calyptus Solidity Challenge 221
*
What can go wrong with the following swap?
*
` // SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
contract Exchange {
    uint256 private constant CONVERSION = 1e12;

    function swapDAIForUSDC(uint256 usdcAmount) external pure returns (uint256 a) {
        uint256 daiToTake = usdcAmount / CONVERSION;
        conductSwap(daiToTake, usdcAmount);
    }
}
`
# Answer
The swapDAIForUSDC function in the Exchange contract has a potential issue related to integer division. The function divides usdcAmount by CONVERSION, which is a constant with a value of 1e12. This means that the result of the division will be rounded down to the nearest integer. If usdcAmount is not a multiple of 1e12, the result will be less than the actual value of usdcAmount divided by 1e12.
To fix this issue, we should use the SafeMath library to perform the division. This will ensure that the result is rounded up to the nearest integer. 

1. Division Precision Loss: The usdcAmount is divided by CONVERSION (1e12) to calculate daiToTake. Solidity performs integer division, so the result is rounded down to the nearest whole number. This could lead to precision loss, especially for small amounts of usdcAmount.
In Solidity, division is integer division, which means the result is rounded down to the nearest integer. This can lead to precision loss, especially when dividing smaller numbers.

To handle this, we can multiply before dividing to maintain precision. Here's an example:

`uint256 a = 5;
uint256 b = 2;
uint256 result = (a * 1e18) / b;
`
In this example, a is multiplied by 1e18 (which represents 18 decimal places) before dividing by b. The result is 2.5e18, which represents 2.5 with 18 decimal places of precision.

However, be aware that this can lead to overflow if the numbers are large. To prevent overflow, we can use the SafeMath library, which provides functions for safe arithmetic operations.

Also, remember to adjust the result back to the original scale when using it in further calculations or returning it from a function.

2. No Balance Checks: The function does not check if the contract has enough DAI to swap for the requested USDC amount. If the contract's DAI balance is less than daiToTake, the swap should not be allowed.

3. No Return Value: The function is declared to return a uint256, but it doesn't return anything. This could lead to unexpected behavior when calling this function.

4. Function Visibility: The conductSwap function is called, but its implementation and visibility are not shown. If it's not declared in the same contract and is not external or public, this will cause a compilation error.

5. Pure Function: The swapDAIForUSDC function is declared as pure, which means it's not supposed to change the state. However, a swap operation would typically change the state of the contract (by transferring tokens), so pure is likely incorrect here.

Please note that without the full context of the contract, especially the implementation of conductSwap, it's hard to provide a comprehensive review.