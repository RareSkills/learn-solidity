## Strings

Strings are dynamically-sized UTF-8 encoded bytes is also a dynamically-sized byte array. Both of which are interchangeable by just using string() to convert strings to bytes and bytes() to convert bytes to strings respectively. This greatly helps us in doing operations on string like we can with other programming languages. However since strings are UTF-8 encoded if such character requires more than one byte then it increases the difficulty in string manipulation.

Because strings are arrays, they need the calldata or memory modifier when passed to functions, and the memory modifier when returned.

To check a string’s length:

```solidity

function stringLength(
    string memory input
) 
    public 
    pure 
    returns (uint256) {
        return bytes(input).length;
        // input.length won't work
}

```

This does NOT mean how many characters are in the string, but how long the byte array is. Unicode characters take up more than one byte.

To access a string’s character:

```solidity

function characterOfString(
    string memory input, 
    uint256 index
) 
    public 
    pure 
    returns (string memory) {
        bytes memory char = new bytes(1);
        char[0] = bytes(input)[index];
        return string(char);
}

```

Keep in mind this will only work if the entire string is ascii. If we pass in unicode characters that take up more than one byte, for example “你好” the code will crash.

Getting a character from a string is a bit harder than just indexing it like javascript or python because we have to initialise a string array of length 1 and then insert the character we want to get to the new string array. This is what the code above is doing.

Solidity support unicode strings:

```solidity

contract Message {
    // no need for a getter function, this is public
    // note the "unicode" modifier
    string public message = unicode"你好👋ℝ𝔸ℝ𝔼𝕊𝕂𝕀𝕃𝕃𝕊";
}
```

Something slightly misleading is solidity is that we use “strings” to represent hex data if the hex modifier is used. The following shoes casting the hexadecimal encoding of “helloworld” to helloworld.

```solidity

contract HexData {
    // not a string!
    bytes hexData = hex"68656C6C6F776F726C64";
    // returns "helloworld"
    function getMessage() 
        public 
        view 
        returns (string memory) {
            return string(hexData);
    }
}

```

Concatenating strings are made easy at solidity 0.8.12 with the addition of string.concat(). Below that version, and string.concat() is not available.
