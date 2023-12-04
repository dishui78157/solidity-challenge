solidity-challenge-217.md

# Calyptus Solidity Challenge 217
*This contract exchanges stablecoins for ETH, and while its business model is questionable, the smart contract itself should not be a concern 📜

What raises questions about the smart contract? 👇*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;
// This oracle smart contract returns the LATEST prices of various assets
interface SimpleOracle {
    function getEthPrice() external returns (uint256 value);
}

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
error couldlNotMint(); 
error oracleMalfunction();

contract StableCoinOneWay is ERC20 {
    SimpleOracle oracle;
    uint256 errorCount;

    constructor() ERC20("Stable", "STB") {}

    function rate() internal returns (uint256 value, bool success) {
        require(errorCount < 10); 
        try oracle.getEthPrice() returns (uint256 v) {
            return (v, true);
        } catch Error(string memory) {
            errorCount++; return (0, false);
        } catch Panic(uint256) {
            errorCount++; return (0, false);
        } catch (bytes memory) {
            errorCount++; return (0, false);
        }
    }
    function mint() public payable {
        (uint256 value, bool success)  = rate();
        if (!success) revert couldlNotMint();
        _mint(msg.sender, (msg.value / value) * 1e18);
    }
}
`