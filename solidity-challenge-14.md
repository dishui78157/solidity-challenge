# Calyptus Solidity Challenge 14
*An old man has stored his wealth in this smart contract and revealed the secret password to his grandson to unlock the funds. The grandson is just about to transfer the ETH to his own account by calling inherit(). How would you hack this?*
`
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract InheritWealth {
    bytes32 constant public passwordHash = 0x03C6FcED478cBbC9a4FAB34eF9f40767739D1Ff717F6ADBEf982297579C20306;

    function inherit(string memory password) public {
        require(passwordHash == keccak256(abi.encodePacked(password)), "Wrong password!");
       
        payable(msg.sender).transfer(address(this).balance);
    }
}
`

# Answer
perform Front-running attack

Front running is a type of attack that can occur on blockchain networks, particularly those that operate on a public ledger like Ethereum.

In a front running attack, a malicious actor monitors the blockchain or the transaction pool (also known as the "mempool") for pending transactions. Upon identifying a profitable transaction that has not yet been confirmed, the attacker creates a similar transaction but with a higher gas price.

Miners prioritize transactions with higher gas prices for inclusion in the blocks they mine. So, the attacker's transaction is likely to be confirmed before the original transaction. This is the "front running" — jumping ahead in line by offering to pay more.

For example, in a decentralized exchange, an attacker could see a large pending order to buy a certain token. Predicting that this order will increase the token's price once it's executed, the attacker quickly places their own buy order with a higher gas price. Their order is executed first, they buy the token at the lower price, then sell it at the higher price once the original order goes through, profiting from the price difference.

Front running is a significant issue in blockchain networks and various strategies are being explored to mitigate it, such as the introduction of privacy techniques, off-chain transactions, and improved consensus algorithms.


The provided Solidity code defines a contract named InheritWealth. This contract is designed to hold funds that can only be transferred out by someone who knows a specific password.

The contract has a state variable passwordHash which is a constant and public. This hash is presumably the keccak256 hash of a secret password. The actual password is not stored in the contract for security reasons, only its hash is stored.

The contract includes a single function inherit. This function is public, meaning it can be called by anyone. It takes a string argument password.

When inherit is called, it first checks if the keccak256 hash of the provided password matches the stored passwordHash. This is done using the require function, which reverts the transaction if the condition is not met. If the hashes match, it means the correct password was provided.

If the correct password is provided, the function then transfers the entire balance of the contract to the address that called the function. This is done using the transfer function of the payable address of the caller (msg.sender).

The challenge here is to find a way to call the inherit function with the correct password, given only the hash of the password. This would typically involve some form of hash cracking, which can be computationally intensive and is not guaranteed to succeed, especially if the password is complex.

Hacking this contract to transfer the funds without knowing the password would be extremely difficult. The keccak256 hash function, used by Ethereum, is a one-way function, meaning it's computationally infeasible to retrieve the original input given only the output hash.

However, if the password used is weak (a common word, phrase, or number sequence), a brute force attack could potentially be successful. This would involve hashing a large number of potential passwords and comparing them to the passwordHash until a match is found.

It's important to note that attempting such an attack would require a significant amount of computational resources and may not be feasible depending on the strength of the password. Furthermore, such an action would be illegal and unethical.

In a real-world scenario, the best course of action would be to obtain the password legitimately, for example, by asking the old man or his grandson.