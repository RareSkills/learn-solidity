# Immutable variables

If you aren’t ever going to change a variable, it’s best to explicit about your intentions. Solidity has a keyword for this.

```solidity

contract ExampleContract {

    uint256 public immutable value;

    constructor(uint256 _value) {
        value = _value;
    }
}
```

**If a variable is set in the constructor and never updated, it should be immutable**

If you try to write to an immutable variable, the code will not compile.

```solidity

contract ExampleContract {

    uint256 public immutable value;

    constructor(uint256 _value) {
        value = _value;
    }

    // ERROR: Cannot compile
    function cannotChangeTheName(uint256 _newValue)
            external {
                value = _newValue;
    }
}
```

**Practice Problems**

[Immutable](https://github.com/RareSkills/Solidity-Exercises/tree/main/Immutable)
