# Application Binary Interface Encoding (abi encoding)

We’re going to have to go on what seems like another random tangent before we can introduce our next piece of information.

But I want you to understand what the following things are

abi.encode

abi.decode

abi.encodeWithSignature

To motivate them, let’s create another smart contract, open the “debug” dropdown, and get a certain piece of information.

![https://static.wixstatic.com/media/61a666_57ac790548bd46f293823e0574f3e152~mv2.png/v1/fill/w_939,h_447,al_c,q_95,enc_auto/ABI%20Encoding1.png](https://static.wixstatic.com/media/61a666_57ac790548bd46f293823e0574f3e152~mv2.png/v1/fill/w_939,h_447,al_c,q_95,enc_auto/ABI%20Encoding1.png)

```solidity

contract ExampleContract {

    function meaningOfLifeAndAllExistence()
        public 
        pure 
        returns (uint256) {
            return 42;
    }
}

```

When we copy that, we get

**0x92d62db5**

What exactly is this? This is the **function signature** of “meaningOfLifeAndAllExistence()”. We’ll learn later how this is derived.

Whenever you “call” a smart contract, you are actually sending an ethereum transaction with some data attached so the smart contract knows which function to execute.

Let’s look at the information from another vantage point.

![https://static.wixstatic.com/media/61a666_38d5431035ee47bca02b56df9b3f92f7~mv2.png/v1/crop/x_2,y_0,w_1998,h_882/fill/w_939,h_414,al_c,q_95,enc_auto/ABI%20Encoding2.png](https://static.wixstatic.com/media/61a666_38d5431035ee47bca02b56df9b3f92f7~mv2.png/v1/crop/x_2,y_0,w_1998,h_882/fill/w_939,h_414,al_c,q_95,enc_auto/ABI%20Encoding2.png)

```solidity

contract ExampleContract {

    function meaningOfLifeAndAllExistence()
        public 
        pure 
        returns (bytes memory) {
            return msg.data;
    }
}

```

We’ve changed the return type to “bytes memory” (don’t worry that we haven’t seen that before), and returned a variable called msg.data (also don’t worry that we haven’t seen that before).

The important thing to note is that we get an identical byte sequence back!

So what is happening?

When you call a function in a smart contract, you aren’t actually doing a “function call” per se, you are sending data to the contract with some information about which function should be executed.

Makes sense right? When you fire up your browser wallet and trade ERC20 tokens, there’s no way you can “call a function” on an ERC20 contract remotely. Function calls only happen inside the same execution context. Describing transactions as functions however, is convenient. But we need to look behind the curtain to see exactly what is happening to really understand Solidity.

When you “call a smart contract” you are sending data to the contract with instructions for how to execute.

There are many data encodings, json, xml, protobufs, etc. **Solidity and ethereum use the ABI encoding**.

We won’t get into the specification of ABI here. But what you need to know is that it always looks like a sequence of bytes.

Functions are identified as a sequence of four bytes. Our original byte sequence **(0x92d62db5)** had four bytes in it: 92, d6, 2d, b5.

Remember, a byte is 8 bits, and 8 bits can be a value up to 255 (2^8 - 1). A byte, represented in hex, can go from 0x00 to 0xff. Convert 0xff to decimal, and hopefully this makes it clear.

When a function takes no arguments, sending four bytes that represent the function instructs the smart contract to execute that function.

But what would the data look like if it took an argument?

![https://static.wixstatic.com/media/61a666_2afb4e91b2f0423ba0d719a3d8b382ec~mv2.png/v1/fill/w_939,h_209,al_c,q_95,enc_auto/ABI%20Encoding3.png](https://static.wixstatic.com/media/61a666_2afb4e91b2f0423ba0d719a3d8b382ec~mv2.png/v1/fill/w_939,h_209,al_c,q_95,enc_auto/ABI%20Encoding3.png)

```solidity

contract ExampleContract {
    function takeOneArg(uint256 x)
        public 
        pure 
        returns (bytes memory) {
            // we won't do anything with x
            return msg.data;
    }
}

```

We get

0xf8689fd30000000000000000000000000000000000000000000000000000000000000007

returned to us. The f8689fd3 portion means call function “takeOneArg” and the 7 with a lot of leading zeros means pass the number 7.

It would be horribly confusing if we had to do this by hand.

Thankfully we don’t.

Watch this.

![https://static.wixstatic.com/media/61a666_687ca50bc74449e28295e2641620d6c1~mv2.png/v1/fill/w_939,h_256,al_c,q_95,enc_auto/ABI%20Encoding4.png](https://static.wixstatic.com/media/61a666_687ca50bc74449e28295e2641620d6c1~mv2.png/v1/fill/w_939,h_256,al_c,q_95,enc_auto/ABI%20Encoding4.png)

```solidity

contract ExampleContract {

    function getEncoding(uint x)
        public 
        pure 
        returns (bytes memory) {
            return abi.encodeWithSignature("takeOneArg()", x);
    }
    
    function takeOneArg(uint256 x)
        public 
        pure 
        returns (bytes memory) {
            return msg.data;
    }
}

```

We don’t need to concern ourselves with the specification of ABI encoding now, let’s just get comfortable using it.

Consider the following example.

![https://static.wixstatic.com/media/61a666_ef35fbdaa086479aa76b7ce06edceb08~mv2.png/v1/fill/w_939,h_297,al_c,q_95,enc_auto/ABI%20Encoding5.png](https://static.wixstatic.com/media/61a666_ef35fbdaa086479aa76b7ce06edceb08~mv2.png/v1/fill/w_939,h_297,al_c,q_95,enc_auto/ABI%20Encoding5.png)

```solidity

contract ExampleContract {

    function encodingXY(uint x, uint256 y)
        public 
        pure 
        returns (bytes memory) {
            return abi.encode(x,y);
    }
    
    function getATuple(bytes memory encoding)
        public 
        pure 
        returns (uint256, uint256) {
            (uint256 x, uint256 y) = abi.decode(encoding, 
                (uint256, uint256));
            return(x,y);
    }
}

```

Note that we are using “abi.encode” and “abi.decode”. The “withSignature” bit is when functions are involved, but that is not the case here.

In this example, the variables x and y are abi encoded into

0x0000000000000000000000000000000000000000000000000000000000000005000000000000000000000000000000000000000000000000000000000000000f

The decimal numbers got converted to hex, which is why “5” is still “5” but 15 became “f”.

If we know in advance that this is a pair of uint256, we can “decode” it back to a pair using the function screenshotted above.

The tuple that appears as the second argument in abi.decode is instructions for how to decode the data. If you provide the wrong datatypes or the wrong length of tuple here, you’ll either get the wrong result, or the code will revert.

**Problems**

[Encode](https://github.com/RareSkills/Solidity-Exercises/tree/main/Encoder)

[Decoder](https://github.com/RareSkills/Solidity-Exercises/tree/main/Decoder)
