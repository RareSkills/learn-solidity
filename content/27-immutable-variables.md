## Immutable variables

If you aren’t ever going to change a variable, it’s best to explicit about your intentions. Solidity has a keyword for this.

```solidity

contract ExampleContract {

    string immutable public name;

    constructor(string memory _name) {
        name = _name;
    }
}
```

**If a variable is set in the constructor and never updated, it should be immutable**

If you try to write to an immutable variable, the code will not compile.

```solidity

contract ExampleContract {

    string immutable public name;

    constructor(string memory _name) {
        name = _name;
    }

    // ERROR: Cannot compile
    function cannotChangeTheName(string calldata _newName) 
            external {
                name = _newName;
    }
}
```

**Practice Problems**

[Immutable](https://github.com/RareSkills/Solidity-Exercises/tree/main/Immutable)
