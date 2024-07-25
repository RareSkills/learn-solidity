## Inheritance

Implementing an ERC20 contract from scratch each time would no doubt get tiring. Solidity behaves like an object oriented language and allows for inheritance. Here is a minimal example.

```solidity

contract Parent {
    function theMeaningOfLife() 
        public 
        pure 
        returns (uint256) {
            return 42;
    }
}

contract Child is Parent {

}

```

Deploy to Remix, but from the dropdown, choose Child to deploy, not Parent.

![https://static.wixstatic.com/media/61a666_843d766cb40445de9d21657f94bfa237~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/Inheritance1.png](https://static.wixstatic.com/media/61a666_843d766cb40445de9d21657f94bfa237~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/Inheritance1.png)

Even though Child is empty, we see the function in the Child

![https://static.wixstatic.com/media/61a666_fb428cb0a1aa4236950028fcf7a1db70~mv2.png/v1/fill/w_939,h_595,al_c,q_95,enc_auto/Inheritance2.png](https://static.wixstatic.com/media/61a666_fb428cb0a1aa4236950028fcf7a1db70~mv2.png/v1/fill/w_939,h_595,al_c,q_95,enc_auto/Inheritance2.png)

When a “contract” **is** “another contract”, it inherits all it’s functionality.

Like other object oriented programming languages, functions can be overridden. Here is the construction for changing the value.

```solidity

contract Parent {
    function theMeaningOfLife() 
        public 
        pure 
        virtual 
        returns (uint256) {
            return 42;
    }
}

contract Child is Parent {
    function theMeaningOfLife() 
        public 
        pure 
        override 
        returns (uint256) {
            return 43;
    }
}

```

![https://static.wixstatic.com/media/61a666_31b8d5b723244b1d91878f40900d7a97~mv2.png/v1/fill/w_939,h_434,al_c,q_95,enc_auto/Inheritance3.png](https://static.wixstatic.com/media/61a666_31b8d5b723244b1d91878f40900d7a97~mv2.png/v1/fill/w_939,h_434,al_c,q_95,enc_auto/Inheritance3.png)

Note that only **virtual** functions can be overridden. If you try to override a function that isn’t virtual, the code won’t compile.

Also, when a function overrides, it must match exactly, both in name, arguments, and return type.

```solidity

contract Parent {
    function theMeaningOfLife()
        public
        pure
        virtual
        returns (uint256) {
            return 42;
    }
}

contract Child is Parent {

    // INVALID: has different arguments
    function theMeaningOfLife(uint256 x)
        public
        pure
        override
        returns (uint256) {
            return 42 + x;
    }

    // INVALID: has different return type
    function theMeaningOfLife(uint256 x)
        public
        pure
        override
        returns (bool) {
            return true;
    }

    // INVALID: has different name
    function theMeaningOfLif3(uint256 x)
        public
        pure
        override
        returns (uint256) {
            return 42;
    }
}

```

Solidity supports multiple inheritance

```solidity

contract Parent1 {
    function theMeaningOfLife()
        public
        pure
        virtual
        returns (uint256) {
            return 42;
    }
}

contract Parent2 {
    function hackerFavoriteNumber()
        public
        pure
        virtual
        returns (uint256) {
            return 1337;
    }
}

contract Child is Parent1, Parent2 {

}

```

In case you are wondering, if the two parents had a function with the same name, the child must override it or the behavior will be ambiguous. If you end up in this situation, you probably did something wrong in your software design. So let’s not go down that road.

**Private vs Internal**

There are two ways to make a function not accessible from the outside world: giving them a private or internal modifier. The distinction is simple.

Private functions (and variables) cannot be “seen” by the child contracts.

Internal functions and variables can.

```solidity

contract Parent {
    function foo()
        internal
        pure
        virtual
        returns (string memory) {
            return "foo";
    }

    // error! private functions cannot be overriden, 
    // so no point in making them virtual!
    function bar()
        private
        pure
        virtual
        returns (string memory) {
            return "bar";
    }
}

```

**The super keyword**

The super keyword means “call the parent’s function.” Here’s how it can be useful

```solidity

contract Parent {
    function foo() 
        internal 
        pure 
        virtual 
        returns (string memory) {
            return "foo";
    }
}

contract Child is Parent {

        // we have overriden foo and made it public
    function foo() 
        public 
        pure 
        override 
        returns (string memory) {
            return super.foo();
    }
}

```

If we didn’t include the super keyword here, foo() would call itself and go into infinite recursion. Try removing super and running the code in Remix. The transaction will revert because of the infinite recursion (Ethereum doesn’t let code run forever, it forcibly terminates them. The exact mechanism is an intermediate topic for later discussion).

Super means “call the parent’s foo, not mine.” This let’s us get all the functionality of foo without having to copy and paste the code.

**Calling the parent’s constructor**

Solidity won’t let you inherit from a parent contract without initializing it’s constructor. Consider this situation.

```solidity

contract Parent {
    string private name;

    constructor(string memory _name) {
        name = _name;
    }

    function getName() public view virtual returns (string memory) {
        return name;
    }
}

contract Child is Parent {

    // error, name hasn't been set!
    function getName() public view override returns (string memory) {
        return super.getName();
    }
}

```

The fix is to call the parent constructor at the point of inheritance

```solidity

contract Parent {

    string private name;

    constructor(string memory _name) {
        name = _name;
    }

    function getName() 
        public 
        view 
        virtual 
        returns (string memory) {
            return name;
    }
}

//fixed
contract Child is Parent("The Beatles") {
    function getName() 
        public 
        view 
        override 
        returns (string memory) {
            return super.getName();
    }
}

```

Let’s summarize what we’ve learned

- Only virtual functions can be overridden
- Functions that override a parent’s function must have an override modifier
- The overriding function must match exactly, in name, arguments, and return type
- Instead of copying and pasting the parent function’s code, you can use the super keyword
- You can inherit from multiple contracts
- You must explicitly call a parent’s constructor when doing inheritance.

**Create an ERC20 token with little effort**

Inheritance, combined with the import statement, makes it easy for us to leverage libraries created by other people. Deploy this contract in Remix, and you’ll see all the ERC20 functions have been implemented for you.

```solidity

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract Token is ERC20("SomeToken", "symbol") {

}
```

![https://static.wixstatic.com/media/61a666_988ae4e48ca24aab836dd651f226c082~mv2.png/v1/fill/w_939,h_485,al_c,q_95,enc_auto/Inheritance4.png](https://static.wixstatic.com/media/61a666_988ae4e48ca24aab836dd651f226c082~mv2.png/v1/fill/w_939,h_485,al_c,q_95,enc_auto/Inheritance4.png)

**A point of clarification**

There is a world of difference between a smart contract as a solidity object and a smart contract deployed on the blockchain.

**You cannot inherit contract deployed on the blockchain.**

They are binary blobs living outside of you. Because of the ambiguous terminology, some solidity developers have worried that functions and variables can be inherited and overridden by a malicious contract. **This cannot occur**. Even though we refer to deployed code as a “contract” and solidity code as a “contract” they are not the same thing.

**Practice Problems**

[InheritanceOverride](https://github.com/RareSkills/Solidity-Exercises/tree/main/InheritanceOverride)

[MultiInheritance](https://github.com/RareSkills/Solidity-Exercises/tree/main/MultiInheritance)

[Super](https://github.com/RareSkills/Solidity-Exercises/tree/main/Super)

[AccessModifiers](https://github.com/RareSkills/Solidity-Exercises/tree/main/AccessModifiers)
