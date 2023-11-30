solidity-challenge-223.md

# Calyptus Solidity Challenge 223
*This TimedDrop contract resets its timer to 30 days with each airdrop call, clearing the signer for the next drop 🪂

If signature is verified, 1000 tokens (AIRDROP_AMOUNT) are minted to the caller.

Any potential exploits for claiming airdrops? *
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract TimeDrop {
    address signer;
    uint256 timer = block.timestamp * 30 days;
    uint256 AIRDROP_AMOUNT = 1000;

    function airdrop(address who, uint8 v, bytes32 r, bytes32 s) external {
        if (block.timestamp > timer) {
            signer = msg.sender;
        }
        require(signer == ecrecover(keccak256(abi.encode(who, AIRDROP_AMOUNT)), v, r, s), "invalid signature");
        
        mint(msg.sender, AIRDROP_AMOUNT);
        timer = block.timestamp + 30 days;
        signer = address(0);
    
    }
}
`
# Answer
The TimeDrop contract has a potential exploit in the airdrop function. The signer is set to msg.sender if the current time is greater than the timer. This means that any address can become the signer if they call the airdrop function after the timer has expired.

Once an address has become the signer, they can create a valid signature for the ecrecover function, because they know the who address and the AIRDROP_AMOUNT. This allows them to pass the require statement and mint tokens to themselves.

To fix this issue, we should remove the condition that sets the signer to msg.sender and instead set the signer to a specific address in the constructor of the contract. This way, only the address that we specify can create valid signatures for the airdrop function.

Here's how we can do it:
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract TimeDrop {
    address signer;
    uint256 timer = block.timestamp + 30 days;
    uint256 AIRDROP_AMOUNT = 1000;

    constructor(address _signer) {
        signer = _signer;
    }

    function airdrop(address who, uint8 v, bytes32 r, bytes32 s) external {
        require(block.timestamp > timer, "Timer has not expired");
        require(signer == ecrecover(keccak256(abi.encode(who, AIRDROP_AMOUNT)), v, r, s), "invalid signature");
        
        mint(msg.sender, AIRDROP_AMOUNT);
        timer = block.timestamp + 30 days;
    }
}
`
In this updated version, the signer is set in the constructor of the contract, and the airdrop function requires that the current time is greater than the timer before it proceeds. This ensures that only the specified signer can create valid signatures for the airdrop function, and that the function can only be called after the timer has expired.