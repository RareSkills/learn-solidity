## Introduction to arrays and strings

In this section we will introduce the array data structure and the string data structure. These behave differently from the solidity datatypes we discussed earlier, so we will discuss them here.

Syntax for declaring arrays

Let’s look at a function that takes an array and returns an array. There is quite a bit to unpack here!

First, it should be clear that the syntax for declaring an array of numbers is uint256[]. We’ll get to “calldata” and “memory” in a moment.

```solidity

contract ExampleContract {
    function useArrayForUint256(uint256[] calldata input) 
        public 
        pure 
        returns (uint256[] memory) {
            return input;
    }
}

```

If you wanted an array of addresses or booleans, it would be the following:

```solidity

contract ExampleContract {
    function booleanArrayExample(bool[] calldata input) 
        public 
        pure 
        returns (bool[] memory) {
            return input;
    }

    function addressArrayExample(address[] calldata input) 
        public 
        pure 
        returns (address[] memory) {
            return input;
    }
}

```

So what is this “calldata” and “memory” **bit? First off, if you don’t include them, the code won’t compile. Here are two examples of code that doesn’t compile.

```solidity

contract BadContract1 {

    // argument is missing calldata
    function useArrayForUint256(uint256[] input) 
        public 
        pure 
        returns (uint256[] memory) {
            return input;
    }
}

```

```solidity

contract BadContract2 {

    // return type is missing memory
    function useArrayForUint256(uint256[] calldata input) 
        public 
        pure 
        returns (uint256[]) {
            return input;
    }
}

```

So what is calldata and memory?

If you are familiar with C or C++, this concept will be intuitive. Memory in Solidity is like the heap in C, C++, or Rust. Arrays can have unlimited size, so storing them on the execution stack (don’t worry if you don’t know what that is), could lead to a *stackoverflow* error (not to be confused with the famous forum!).

Calldata is something unique to Solidity. It is the actual “transaction data” that is sent when someone transmits a transaction to the blockchain.

Calldata means “refer to the data in the Ethereum transaction itself.” This is a fairly advanced concept, so don’t worry if you don’t fully understand it for now.

When in doubt: the function arguments for arrays and strings should be calldata and the function arguments for the return type should be memory.

There are some exceptions to using “calldata” in a function argument, but the return type for an array should always be memory, never calldata, or the code won’t compile. To avoid bombarding you with information, we will talk about the exceptions to calldata later.

Here is how to use arrays of numbers with Remix.

![Untitled 1](https://github.com/user-attachments/assets/a399da3e-cf19-494e-ad4e-6d5dc568722b)

**Arrays are zero indexed like every other language**

No surprises here.

```solidity

contract ExampleContract {
    function returnFirstElement(uint256[] calldata myArray) 
        public 
        pure 
        returns (uint256) {
            uint256 first = myArray[0];
            return first;
    }
}

```

Note that the return type is uint256, because we are returning a number, not an array.

Note that if the array was empty, the transaction will revert.

**To get the length of an array, use .length**

This is the same as JavaScript.

```solidity

contract ExampleContract {
    function returnFirstElement(uint256[] calldata myArray) 
        public 
        pure 
        returns (uint256) {
            uint256 len = myArray.length;
            return len;
    }
}

```

This is also how you can loop over an array.

```solidity

contract ExampleContract {
    function productOfarray(uint256[] calldata myArray) 
        public 
        pure 
        returns (uint256) {
            uint256 product = 1;
            for (uint256 i = 0; i < myArray.length; i++) {
                product *= myArray[i];
            }
            return product;
    }
}

```

**Arrays can be declared to have a fixed length**

In the previous examples, the square brackets had nothing inside of them during declaration. If you want to force an array to have a fixed size, you can put the size inside of the square brackets.

```solidity

contract ExampleContract {
    function productOfarray(uint256[5] calldata myArray) 
        public 
        pure 
        returns (uint256) {
            uint256 last = myArray[4];
            return last;
    }
}

```

If the function is passed an array of any size other than 5, it will revert.

**Strings**

Strings behave very similar to arrays. In fact, they are arrays under the hood (but with some differences). Here is a function that returns the string you passed it.

```solidity

contract ExampleContract {
    function echo(string calldata input) 
        public 
        pure 
        returns (string memory) {
            return input;
    }
}

```

And here is *hello world* finally.

```solidity

contract ExampleContract {
    function helloWorld() 
        public 
        pure 
        returns (string memory) {
            return "Hello, world!";
    }
}

```

**Concatenating strings**

Funnily enough, solidity did not support string concatenation until February 2022 when Solidity 0.8.12 was released. If you want to do string concatenation in Solidity, make sure the pragma at the top of the file is at least 0.8.12

```solidity

pragma solidity ^0.8.12;
contract ExampleContract {
    function useArrays(string calldata user) 
        public 
        pure 
        returns(string memory) {
            return string.concat("hello ", user);
    }
}

```

There is a reason support for concatenation was added so late, smart contracts usually deal with numbers, not strings.

**Strings cannot be indexed**

In languages like JavaScript or Python, you can index a string like you would an array and get a character back. Solidity cannot do this. The following code won’t compile

```solidity

pragma solidity ^0.8.12;
contract BadContract {
    function useArrays(string calldata input) 
        public 
        pure 
        returns(string memory) {
            return input[0]; // error
    }
}

```

**Strings do not support length**

Solidity does not support getting the length of a string. This is because unicode characters can make the length ambiguous, and solidity represents strings as a byte array, not a sequence of characters.

```solidity

pragma solidity ^0.8.12;
contract StringContract {
    function useArrays(string calldata input) 
        public 
        pure 
        returns(uint256) {
            return input.length; // does not compile
    }
}

```

**What we’ve left out**

- Arrays in Solidity support operations like pop(), but this has side-effects which are more advanced, so we will teach this later.
- Declaring arrays and strings inside a function, as opposed to in the argument or return value, has a different syntax.

**Practice Problems**

[FizzBuzz](https://github.com/RareSkills/Solidity-Exercises/tree/main/FizzBuzz)

[SumArray](https://github.com/RareSkills/Solidity-Exercises/tree/main/SumArray)

[FilterOddNumbers](https://github.com/RareSkills/Solidity-Exercises/tree/main/FilterOddNumbers)

[IsSorted](https://github.com/RareSkills/Solidity-Exercises/tree/main/IsSorted)

[Mean](https://github.com/RareSkills/Solidity-Exercises/tree/main/Mean)
