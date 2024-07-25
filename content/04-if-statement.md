## If Statements

If statements behave exactly the same as other languages

```solidity

contract ExampleIfStatement {
    function isAMultipleOfTen(uint256 x) 
        public 
        pure 
        returns (bool) {
            if (x % 10 == 0) {
                return true;
            } else {
                return false;
            }
    }
}

```

The argument inside the if statement must be a boolean. Note that our code above is equivalent to the following.

```solidity

contract ExampleIfStatement {
    function isAMultipleOfTen(uint256 x) 
        public 
        pure 
        returns (bool) {
            bool isMul = x % 10 == 0;
            if (isMul) {
                return true;
            } else {
                return false;
            }
    }
}

```

Unlike dynamic languages such as Python or JavaScript, you cannot do the following

```solidity

function isNotZero(uint256 x) 
    public 
    pure 
    returns (bool) {
        if (x) {
            return true;
        } else {
            return false;
        }
}

```

Solidity also supports the “else if” construction, but we will assume you are already familiar with what that looks like.

Solidity does not have a **switch** statement like Java and C do.

**Practice Problems**

[IfStatement](https://github.com/RareSkills/Solidity-Exercises/tree/main/IfStatement)
