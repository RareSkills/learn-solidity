## Interfaces

I’m sure you thought the way we were calling other contracts with .call and abi.encodeWithSignature was a bit clumsy.

I wanted you to go through that exercise so that you know what is happening under the hood.

Now it’s time to introduce the ergonomic way Solidity enables cross contract calls.

```solidity

// ------------------ V1 -------------------
contract GetSumV1 {

    function getSum(
        address adder, 
        uint256 a, 
        uint256 b
    ) 
        public 
        returns (uint256) {
            (bool ok, bytes memory result) = 
                adder.call(
                    abi.encodeWithSignature(
                        "add(uint256,uint256)", 
                        a, 
                        b
                    )
                );
            require(ok, "call failed");
            uint256 sum = abi.decode(result, (uint256));
            return sum;
    }
}

// ----------------- V2 --------------------
interface IAdder {
    function add(
        uint256, 
        uint256
    ) 
        external 
        view 
        returns (uint256);
}

contract GetSumV2 {
    function getSum(
        IAdder adder, 
        uint256 a, 
        uint256 b
    ) 
        public 
        returns (uint256) {
            return adder.add(a, b);
    }
}

contract Adder {
    function add(
        uint256 a, 
        uint256 b
    ) 
        public 
        view 
        returns (uint256) {
            return a + b;
    }
}

```

V1 and V2 are very similar, but they have a key difference under the hood. We will get to that in a second.

The important fact here is that V2 is much cleaner than V1!

The interface nicely wraps up the abi encoding and decoding for us so that we don’t have to think about it. The interface defines the return type, which defines how the abi decoding will work, and the function signature and arguments define the abi encoding.

Now we can abstract that all away and call another contracts function as if it was just another function call.

Pretty cool right?

Note that the modifier for the interface function is **external** not public. You cannot declare interfaces to be public, only external. You don’t declare the internal functions because outside contracts won’t know anything about it.

I’ve been simplifying things by making everything public up to this point, except where absolutely necessary. But you’ve grown as a Solidity dev since the twenty+ chapters we’ve been though. Therefore, I will now introduce a best practice you should adhere to.

**Unless a function needs to be called from inside a smart contract, it should be external, not public.**

Interfaces don’t allow you to declare functions as public because it’s irrelevant on the outside if the function you are interacting with can be called from within the other smart contract.

Now let’s look at the other key difference.

getSum was a regular public function in V1, but it was a **view** function in V2.

Wait, wasn’t it the case that we couldn’t do that, because we don’t know if the other function will modify the state?

Solidity has a special kind of call, a **staticcall** that behaves like a regular call but forces the transaction to revert if a state change happens.

This is valid solidity.

```solidity

// ------------------ V1 -------------------
contract GetSumV1 {

    // note we changed call to staticcall
    // public --> external
    // added a view modifier
    function getSum(
        address adder, 
        uint256 a, 
        uint256 b
    ) 
        external 
        view 
        returns (uint256) {
            (bool ok, bytes memory result) = 
                adder.staticcall(
                    abi.encodeWithSignature(
                        "add(uint256,uint256)", 
                        a, 
                        b
                    )
                );
            require(ok, "call failed");
            uint256 sum = abi.decode(result, (uint256));
            return sum;
    }
}

contract Adder {

    // view changed to pure
    function add(
        uint256 a, 
        uint256 b
    ) 
        external 
        pure 
        returns (uint256) {
            return a + b;
    }
}

```

I know you are dreading the ABI encoding stuff, but I want to show you what happens behind the scenes when an interface declares a function to be view. It means the underlying action will happen with a staticcall.

The reader is encouraged to do state changing things inside of add to see the transaction revert.

If you have a Java background, this whole bit with interfaces is probably a rather obvious. But it’s important to remember that, behind the scenes, a cross-contract call, with abi encoding, is happening. You aren’t “compiling” another smart contract into your own, like how Java objects would be combined together.
