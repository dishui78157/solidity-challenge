# Calyptus solidity challenge 37
*Which among the following modifiers `onlyOwner1()` and `onlyOwner2()` is  more gas efficient way of writing modifiers? Why?*
`modifer onlyOwner1(){
    require(owner() = msg.sender, "You're notg the owner");
    _;
}
modifier onlyOwner2() {
    isOwner();
    _;
}
function isOwner() internal view virtual {
    require(owner() = msg.sender, "You're not the owner");
}
`

# Answer #
`onlyOwner1()` is more efficient since it does not call a function. It is a direct comparison. `onlyOwner2()` calls a function which is more expensive than a direct comparison.

however, @calyptus says:
onlyOwner2() reduces deployment cost as it reduces the bytecode size. Modifiers code is copied in all instances where it’s used, increasing bytecode size. By doing a refractor to the internal function, one can reduce bytecode size significantly at the cost of one JUMP.
