# Fixed Size Datatypes: Solidity is a typed language.

Solidity is a typed language.

Unlike JavaScript or Python where you can assign a boolean, or a string, or a number, to a variable, each variable can only have one type, and it must be explicitly declared as such.

This applies to functions too. You must explicitly specify the argument type and the return type.

Let’s discuss the most commonly used types now:

- The unsigned integer, or **uint256**
- The boolean variable or **bool**
- The **address** type, which stores Ethereum wallet addresses or smart contract addresses

Solidity has arrays, strings, structs, and other types, but they require a little different treatment, so we’ll discuss them later.

Let’s look at three different functions that return each of these types.

```solidity

contract ExampleContract {
    function getANumber()
        public
        pure
        returns (uint256) {
            uint256 x = 1;
            return x;
    }

    function getABoolean()
        public
        pure
        returns (bool) {
            bool y = true;
            return y;
    }

    function getAnAddress()
        public
        pure
        returns (address) {
            // Vitalik Buterin's address
            address z = 0xd8da6bf26964af9d7eed9e03e53415d37aa96045;
            return z;
    }

    function getAnotherAddress()
        public
        pure
        returns (address) {
            // address of the USDC stablecoin
            address z2 = 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48;
            return z2;
    }
}

```

In these examples, we assigned the value to a variable and then returned it. We can of course directly return the value like so.

```solidity

function getAddress() 
        public 
        pure 
        returns (address) {
    return 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48;
}
```

It’s very important that the function signature matches the return type. The following code will produce an error

```solidity

function getAddressFail()
    public
    pure
    returns (bool) {
        return 0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48;
}
```

**Address**

An address is represented as a hex string that has 40 characters in it, and always starts with 0x. A valid hex string contains the characters [0-9] or [a-f] inclusive.

**Warning**: be careful when typing addresses manually. Solidity will covert 0x1 into an address with the value 0x0000000000000000000000000000000000000001. If you have an address with less than 40 hex characters, it will pad it with leading zeros.

If you create an address with more than 40 characters, it won’t compile.

Note that the 40 characters do not include the leading 0x.

**uint256**

Let’s revisit **uint256**. What exactly does that mean?

The *u* means unsigned. It cannot represent negative numbers. The 256 means it can store numbers up to **256** bits large, or 2^256-1.

Let’s plug that into python to see how big that number is.

```solidity

>>> pow(2,256) - 1
115792089237316195423570985008687907853269984665640564039457584007913129639935
```

That’s a very large number, big enough for pretty much everything you’ll need to do on the blockchain.

This will compile in Solidity

```solidity

function getBiggestNumber()
    public
    pure
    returns (uint256) {
        return 115792089237316195423570985008687907853269984665640564039457584007913129639935;
}

```

But if you make the number bigger, the code won’t compile.

As you can imagine, a **uint128** stores unsigned numbers that are up to 2^128 - 1 in size.

Most of the time, you should only use uint256. The times you would use a smaller type like a uint64 or uint128 is a more advanced subject. Just stick to uint256 for now.

The Boolean type

This one is pretty obvious, it’s just like other languages. A bool variable holds either a true or a false. That’s it.
