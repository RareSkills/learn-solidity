## Require

There’s just one more essential Solidity key word, and then we are ready to create our own ERC20 token.

Although we can use an if statement to check if inputs to a function are valid, or the correct msg.sender is calling the function, the elegant way is to use the require statement. The require statement forces the transaction to revert if some condition is not met.

```solidity

contract ExampleContract {
    function mustNotBeFive(
        uint256 x
    ) 
        public 
        pure 
        returns (uint256) {
            require(x != 5, "five is not valid");
            return x * 2;
    }
}

```

Try the above code out in remix.

Note that leaving out the error message is valid, but considered bad practice, because it makes understanding the failure harder.

```solidity

contract ExampleContract {
    function mustNotBeFive(
        uint256 x
    ) 
        public 
        pure 
        returns (uint256) {
            // valid, but bad practice
            require(x != 5);
            return x * 2;
    }
}
```

You can use this construction to ensure msg.sender is who they are supposed to be. But you can practice that in the following problems.

**Practice Problems**

[NotEnough](https://github.com/RareSkills/Solidity-Exercises/tree/main/NotEnough)

[Owner](https://github.com/RareSkills/Solidity-Exercises/tree/main/Owner)
