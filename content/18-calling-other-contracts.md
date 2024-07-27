# Calling other Contracts

Everything we’ve been doing up to this point is calling smart contracts directly. But it’s also possible, and in fact, desirable, for smart contracts to be able to communicate with each other.

Let’s give a minimum example of this.

```solidity

contract ExampleContract {
    function askTheMeaningOfLife(address source)
        public 
        returns (uint256) {
            (bool ok, bytes memory result) = source.call(
                abi.encodeWithSignature("meaningOfLifeAndAllExistence()")
            );
            require(ok, "call failed");

            return abi.decode(result, (uint256));
    }
}

contract AnotherContract {
    function meaningOfLifeAndAllExistence()
        public 
        pure 
        returns (uint256) {
            return 42;
    }
}

```

Here’s how you can see it in action

![https://static.wixstatic.com/media/61a666_ad5ca9f4df6844fc8f3c4d7463fdd53e~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/Calling%20other%20contracts1.png](https://static.wixstatic.com/media/61a666_ad5ca9f4df6844fc8f3c4d7463fdd53e~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/Calling%20other%20contracts1.png)

Because we reviewed **tuples, abi encoding**, and **bytes memory** already the only surprising thing here should be **call** and the fact that **askTheMeaningOfLife()** is not a view function.

Why isn’t askTheMeaningOfLife() a view function? If you try compiling it with the view modifier, it won’t compile.

View functions are read only. When you call the function of an arbitrary smart contract, you can’t know if it is read-only or not. Therefore, solidity doesn’t let you specify a function as view if it calls another smart contract.

Also, although we can see that **meaningOfLifeAndAllExistence** in **AnotherContract** returns a uint256, we can’t know that in the general case. It could return a string.

**Functions always return abi encoded bytes**. How does remix know to format strings as strings and numbers as numbers? Behind the scenes, it is doing the **abi.decode** operation that we are doing here.

What is the **bool ok** portion of the tuple? Function calls to other smart contracts can fail, for example if the function reverts. To know if the external call reverted, a boolean is returned. In this implementation, the calling function, askTheMeaningOfLifeAndAllExistence, reverts also, but that is not necessarily a general requirement.

Here’s something interesting. What happens if you call a non-existent smart contract?

Try askTheMeaningOfLife(address source) with 0x4B20993Bc481177ec7E8f571ceCaE8A9e22C02db as the argument.

Don’t be lazy, try it out in the code above!

It reverts, but that’s not because the address isn’t there, but because you tried to decode empty data. If we comment out the decoding part, then the function no longer reverts when we call a non-existent address.

When you open the transaction dropdown in remix, you see the revert explanation here.

![https://static.wixstatic.com/media/61a666_07808a80849d4e6c8938a4459a811a87~mv2.png/v1/fill/w_952,h_69,al_c,q_95,enc_auto/Calling%20other%20contracts2.png](https://static.wixstatic.com/media/61a666_07808a80849d4e6c8938a4459a811a87~mv2.png/v1/fill/w_952,h_69,al_c,q_95,enc_auto/Calling%20other%20contracts2.png)

```solidity

contract ExampleContract {
    function askTheMeaningOfLife(address source)
        public 
        returns (uint256) {
            (bool ok, bytes memory result) = source.call(
                abi.encodeWithSignature("meaningOfLifeAndAllExistence()")
            );
            require(ok, "call failed");

            //return abi.decode(result, (uint256));
            return 0;
    }
}

```

What if the other contract takes arguments? Here is the code to do it.

```solidity

contract ExampleContract {
    function callAdd(address source, uint256 x, uint256 y)
        public 
        returns (uint256) {
            (bool ok, bytes memory result) = source.call(
                abi.encodeWithSignature("add(uint256,uint256)", x, y)
            );
            require(ok, "call failed");

            uint256 sum = abi.decode(result, (uint256));
            return sum;
    }
}

contract Calc {
    function add(uint256 x, uint256 y)
        public 
        returns (uint256) {
            return x + y;
    }
}

```

Be careful to not have spaces in "add(uint256,uint256)"

Ready to put this into practice?

**Practice Problems**

[CrossContract](https://github.com/RareSkills/Solidity-Exercises)
