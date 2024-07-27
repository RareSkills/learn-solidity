# Nested Arrays

Nested arrays are rarely used in practice, but we include them here for the sake of completeness.

Nested arrays, as the name suggests, refer to arrays that are contained within another array.

In this example, the function is receiving a rectangular grid.

```solidity

contract ExampleContract {

    function containsAThree(uint256[][] calldata nestedArray) 
        public 
        pure 
        returns (bool) {
            for (uint256 i = 0; i < nestedArray.length; i++) {
                for (uint256 j = 0; j < nestedArray[i].length; j++) {
                    if (nestedArray[i][j] == 3) {
                        return true;
                    }
                }
            }
            return false;
    }
}

```

Here it is running in remix.

![https://static.wixstatic.com/media/c0c19a_b9ca99cdc4384a34b4ad9c56ae28e8b9~mv2.png/v1/fill/w_939,h_494,al_c,q_95,enc_auto/nested%20arrays.png](https://static.wixstatic.com/media/c0c19a_b9ca99cdc4384a34b4ad9c56ae28e8b9~mv2.png/v1/fill/w_939,h_494,al_c,q_95,enc_auto/nested%20arrays.png)

You can also get a 1D array from a 2D array

```solidity

contract ExampleContract {

    // [[1,2],[3,4],[5,6]] becomes [1,2]
    function getRow(uint256[][] calldata nestedArray) 
        public 
        pure 
        returns(uint256[] memory) {
            return nestedArray[0];
    }
}

```

To declare arrays of a fixed size, use the following syntax

```solidity

contract ExampleContract {

    // ACCEPTED: [[1,2],[3,4],[5,6]]
    // REJECTED: [[1,2,3],[4,5,6]]
    function fixedSize(uint256[2][3] calldata nestedArray) 
        public 
        pure 
        returns (uint256) {
            return 0; // just for the sake of compilation
    }
}

```

What may be confusing is that when you access a specific item in an array, the order may feel backwards from other languages, but it makes sense if you think about it.

```solidity

contract ExampleContract {

    // ACCEPTED: [[1,2],[3,4],[5,6]] -> returns 6
    function getLast(uint256[2][3] calldata nestedArray) 
        public 
        pure 
        returns (uint256) {
            return nestedArray[2][1];

            // nestedArray[2] -> [5,6] then get the 1st index item -> 6
    }
}

```

Just like 1D arrays, if you access an out-of-bound area, the transaction will revert.

Note that nested arrays are extremely rare in practice. If you feel like skipping this section, feel free to.

**Problems**

[NestedArray](https://github.com/RareSkills/Solidity-Exercises/tree/main/NestedArray)

[TicTacToe](https://github.com/RareSkills/Solidity-Exercises/tree/main/TicTacToe)
