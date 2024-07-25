## Storage Variables

Up until this point, all of our functions have just returned values that purely depend on the function arguments. They do not depend on anything other than the immediate input. That’s why they are called **pure** functions. They are not aware of the blockchain state or anything that has happened in the past.

This would be quite problematic if we were keeping track of something, like how much money is owed to a certain person, or how many points they have in a game.

Now we introduce the **storage variable.**

These look like “class variables” in other languages, but don’t really behave like them. You can think of them as **variables that behave like a miniature database.**

Let’s look at an example

```solidity

contract ExampleContract {

    uint256 internal x;

    function setX(
        uint256 newValue
    ) 
        public {
            x = newValue;
    }
    
    function getX() 
        public 
        view 
        returns (uint256) {
            return x;
    }
}

```

We have a lot to unpack here!

**Variables declared outside of functions are storage variables. They keep their value after the transaction ends.**

Note that **getX()** has the modifier **view** instead of **pure**. That’s because it views the blockchain state, I.e. what is stored in the variable x. If you change view to pure in this example, the code will not compile. You can also think of **view** as **read-only**. Also note that the return value of getX has the same type as x, both are uint256.

Second, note that **setX** does not have a view or a pure modifier. That’s because it is a **state changing function**. Functions that change storage variables, or make some other lasting change to the blockchain cannot have the view or pure modifier, this is because they are not **read only** and thus cannot be labelled as view, and certainly not **pure**.

To enforce the point, note that the following code is invalid

```solidity

contract ExampleContract {

    uint256 internal x;

    function setX(
        uint256 newValue
    ) 
        public {
            x = newValue;
    }
    
    // error: this function cannot be pure
    function getX() 
        public 
        pure 
        returns (uint256) {
            return x;
    }
}

```

Note that the variable x itself has the modifier **internal**. This means other smart contracts cannot see the value.

**Just because a variable is internal does not mean it is hidden. It’s still stored on the blockchain and anyone can parse the blockchain to get the value!**

This is where things get confusing.

The following code is also valid, but it’s considered bad practice.

```solidity

contract ExampleContract {

    uint256 x;

    function setX(
        uint256 newValue
    ) 
        public {
            x = newValue;
    }
    
    function getX() 
        public 
        view 
        returns (uint256) {
            return x;
    }
}

```

In this case, we removed the internal modifier to x, and it still compiles. This is considered bad practice because you aren’t being explicit about your intentions for the visibility of X.

The following code is also valid

```solidity

contract ExampleContract {

    uint256 public x;

    function setX(
        uint256 newValue
    ) 
        public {
            x = newValue;
    }
    
    function getX() 
        public 
        view 
        returns (uint256) {
            return x;
    }
}

```

**When a variable is declared public, it means other smart contracts can read the value but not modify it.**

This is confusing because public functions can modify variables, but public variables cannot be modified unless there is a function to change their value.

**Summary**

- Storage variable are declared outside of functions
- Public functions that do not have a view or pure modifier can change storage variables
- Pure functions cannot access storage variables

****

**Practice Problems**

[PublicVariable](https://github.com/RareSkills/Solidity-Exercises/tree/main/PublicVariable)

[BasicStorage](https://github.com/RareSkills/Solidity-Exercises/blob/main/BasicStorage/src/BasicStorage.sol)

[PublicFunction](https://github.com/RareSkills/Solidity-Exercises/tree/main/PublicFunction)

[PureVsView](https://github.com/RareSkills/Solidity-Exercises/tree/main/PureVsView)
