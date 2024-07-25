## Constants

Immutable variables can be set once in the constructor, but what if you have a number that you never want to change?

Like other languages, Solidity has the **constant** keyword. This signifies that the value is fixed and never changes.

Let’s say you have an ERC20 token that should never have more than 22 million minted.

This would be the clean way to do it

```solidity

**contract ExampleERC20 {

    uint256 public constant MAX_SUPPLY = 22_000_000;
    // erc20 code

    function mint(unint256 amount) external {
        require(totalSupply() + amount <= MAX_SUPPLY, "max supply exceeded");
        balanceOf[msg.sender] += amount;

        emit Transfer(address(0), msg.sender, amount);
    }
    // rest of the erc20 code
}**
```

Note that 22000000 was written as 22_000_000. They mean the same thing, but the latter is more readable. Underscores in numbers are simply ignored.
