# Calyptus Solidity Challenge 21
*Arrange these transactions in ascending order of gas units consumption. Explain your answer*

`
Arrange these transactions in ascending order of gas units consumption.  Explain your answer.

1) Minting an NFT
2) Transferring an NFT
3) Transferring ether
4) Swapping ERC20 tokens on a DEX
5) Transfering USDC
`

# Answer
Arranging the transactions in ascending order of gas units consumption:

1. Transferring ether
2. Transferring an NFT
3. Transfering USDC (ERC20 token)
4. Minting an NFT
5. Swapping ERC20 tokens on a DEX

Explanation:

1. Transferring ether: This is the simplest operation and consumes the least gas. It involves updating the balances of the sender and receiver.

2. Transferring an NFT: This operation involves updating the ownership of the NFT in the contract. It consumes more gas than a simple ether transfer because it involves interacting with a smart contract and updating its state.

3. Transfering USDC (ERC20 token): This operation is similar to transferring an NFT, but it typically consumes more gas because ERC20 token transfers involve updating the balances of the sender and receiver in the token contract, which can involve more computational steps than transferring an NFT.

4. Minting an NFT: This operation involves creating a new NFT and assigning it to an owner. It consumes more gas than transferring an NFT or ERC20 token because it involves writing more data to the blockchain.

5. Swapping ERC20 tokens on a DEX: This is the most complex operation and consumes the most gas. It involves interacting with the DEX contract, updating balances of multiple tokens, and potentially interacting with liquidity pool contracts.

Please note that the actual gas consumption can vary depending on the specific implementations of the NFT, ERC20, and DEX contracts.