# For Loops

Just like if statements, there is nothing surprising about `for` loops. Here is the code to add up all the numbers from 1 to 99

```solidity

contract ExampleContract {
    function addNumbers() 
        public 
        pure 
        returns (uint256) {
            uint256 sum = 0;
            for (uint256 i = 0; i < 100; i++) {
                sum = sum + i;
            }
            return sum;
    }
}

```

Solidity also supports the `+=` operator if you prefer to do it that way.

```solidity

contract ExampleContract {
    function addNumbers() 
        public 
        pure 
        returns (uint256) {
            uint256 sum = 0;
            for (uint256 i = 0; i < 100; i++) {
                sum += i;
            }
            return sum;
    }
}

```

Solidity also has while loops and do while loops but these are so rarely used that it isn’t worth mentioning them at this point.

A very natural use-case for for loops is iterating over an array. But we haven’t introduced arrays yet, so we’ll explain it at that point.

Like other languages, you can do an early return from a function inside a for loop. This code will loop from 2 to the number until it finds a prime factor.

```solidity

contract ExampleContract {
    function findPrimeFactor(uint256 x) 
        public 
        pure 
        returns (uint256) {
            // start at 2, 1 is not a prime factor
            // use <= because x might be prime
            for (uint256 i = 2; i <= x; i++) {
                if (x % i == 0) {
                    return i;
                }
            }
    }
}

```

**Practice Problems**

[IsPrime](https://github.com/RareSkills/Solidity-Exercises/tree/main/IsPrime)

[Fibonacci](https://github.com/RareSkills/Solidity-Exercises/tree/main/Fibonacci)
