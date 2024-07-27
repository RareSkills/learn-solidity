# Tuples

We’re going to go off on a slight tangent here to introduce the tuple data type, because it is a prerequisite for upcoming sections.

If you’ve used tuples in a language like Python or Rust, no surprises here. It’s an array of fixed size, but the types inside of it can be a mixture.

Here’s an example of a function that returns a tuple:

```solidity

contract ExampleContract {

    function getTopLeaderboardScore()
        public 
        pure 
        returns (address, uint256) {
            return (
                0xd8da6bf26964af9d7eed9e03e53415d37aa96045, 
                100
            );
    }
}

```

Note that tuples are implied. The keyword “tuple” never appears in Solidity.

Tuples can also be “unpacked” to get the variables inside, like in the following example.

```solidity

contract ExampleContract {

    function getTopLeaderboardScore()
        public 
        pure 
        returns (address, uint256) {
            return (
                0xd8da6bf26964af9d7eed9e03e53415d37aa96045, 
                100
            );
    }

    function highestScoreIsOver9000()
        public 
        pure 
        returns (bool) {
            (address leader, uint256 score) = 
                getTopLeaderboardScore();

            if (score > 9000) {
                return true;
            }
        
            return false;  
    }
}

```

As with other languages, tuples do not need to be of length 2. It can be 3, 4 or even longer.

**Practice Problems**

[Tupledore](https://github.com/RareSkills/Solidity-Exercises/tree/main/Tupledore)
