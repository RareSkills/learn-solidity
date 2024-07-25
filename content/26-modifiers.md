## Modifiers

The onlyOwner construction is so common that we’ll dedicate a section to it.

Consider the following

```solidity

contract Ownable {

    address public owner;

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "onlyOwner");
        _;
    }

    function changeOwner(
        address newOwner
    ) 
        public 
        onlyOwner {
            owner = newOwner;
    }

}

contract HoldFunds is Ownable {
    
    function withdrawFunds() 
        public 
        onlyOwner {
            (bool ok, ) = owner.call{
                value: address(this).balance
            }("");
            require(ok, "transfer failed");
    }

    receive() 
        external 
        payable {

    }
}

```

Let’s look at the star of the show here:

```solidity

modifier onlyOwner() {
    require(msg.sender == owner, "onlyOwner");
    _;
}
```

It simply means “execute the code before the underscore, then execute the function.”

This is a handy way to “modify” the function behavior, hence the name “modifiers.”

Note that even though HoldFunds inherited from Ownable, it didn’t override any functions. Inheritance in Solidity is more often a mechanism for including behavior than for defining some kind of polymorphism (don’t worry if you don’t know what that is).

So in this case, if you want your smart contract to have nice handy functions inside of it, you can import another contract that provides the functionality you need.

It isn’t strictly necessary. You can put all your code into one big contract. But that code would be less readable.

Modifiers can be used for things other than checking ownership, but ownership checks are the most common use case.

**Don’t modify state inside modifiers.** Although solidity allows you to do this, it makes the code harder to reason about. This is considered bad practice.
