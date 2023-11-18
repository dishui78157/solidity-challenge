# Calyptus Solidity Challenge 18
*You're selling an NFT collection of 100 NFTs. 10 users are whitelisted to mint it in the 1st day (presale). Everyone else can buy after 1 day (86400 sec) has passed. Which of these functions would you use in your smart contract for NFT minting and why?*

`function mint1() external {
    require(presaleWhitelist[msg.sender] || timeLapsed > 86400); 
    _mint(msg.sender);
}
function mint2() external {
    require(timeLapsed > 86400 || presaleWhitelist[msg.sender]); 
    _mint(msg.sender);
}
`

# Answer

mint2 is the answer since it is more gas efficient.  The white list only has 10 people, so with mint1, there will be two checks. After day 1,  with mint2, we only need to check the first condition.