# Mappings

Mapping, Hashmap, associative array, maps, whatever you want to call it, Solidity has it.

We’ll call it “mapping” because that’s the keyword Solidity uses. Let’s see an example.

```solidity

contract ExampleContract {

    mapping(uint256 => uint256) public myMapping;

    function setMapping(uint256 key, uint256 value) 
        public {
            myMapping[key] = value;
    }

    function getValue(uint256 key) 
        public 
        view 
        returns (uint256) {
            return myMapping[key];
    }
}

```

This does what you think it does. Because myMapping is public, Solidity wraps it with a getter function you can directly access the values with. However, if you want to access the map through a function, you can follow the pattern in **getValue**.

Here is the first surprising thing:

**If you access a mapping with a key that has not been set, you will NOT get a revert. The mapping will just return the “zero value” of the datatype for the value**

In the following example, the mapping will return *false* if you supply a number that hasn’t been set.

```solidity

contract ExampleContract {
    // returns false by default
    mapping(uint256 => bool) public mapToBool;

    // returns 0 by default
    mapping(uint256 => uint256) public mapToUint; 

    // returns 0x0000000000000000000000000000000000000000 by default
    mapping(uint256 => address) public mapToAddress;

}
```

I encourage you to paste this code into remix, then plug in numbers for the key to see the zero values that come back.

![https://static.wixstatic.com/media/61a666_499ad0d908c24c7d8cb892b11941611e~mv2.png/v1/crop/x_2,y_0,w_1998,h_972/fill/w_939,h_457,al_c,q_95,enc_auto/Mappings.png](https://static.wixstatic.com/media/61a666_499ad0d908c24c7d8cb892b11941611e~mv2.png/v1/crop/x_2,y_0,w_1998,h_972/fill/w_939,h_457,al_c,q_95,enc_auto/Mappings.png)

By the way, ERC20 tokens use mappings to store how many tokens someone has! They map an address to how many tokens someone owns.

```solidity

contract ERC20Token {

    mapping(address => uint256) public balances;

    function setSomeonesBalance(address owner, uint256 amount) 
        public {
            balances[owner] = amount;
    }

    function transferTokensBetweenAddresses(
            address sender, 
            address receiver, 
            uint256 amount) 
        public {
            balances[sender] -= amount;   // deduct/debit the sender's balance
            balances[receiver] += amount; // credit the reciever's balance
    }
}

```

This implementation has a flaw that anyone can invoke the public functions and send tokens between addresses willy-nilly, but we’ll fix that later.

Counterintuitively, **ERC20 tokens are not stored in cryptocurrency wallets, they are simply a [uint256](https://www.rareskills.io/post/uint-max-value-solidity) associated with your address in a smart contract**. “ERC20 tokens” are simply a smart contract.

Here is the smart contract for USDC, an ERC20 token: [https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48](https://etherscan.io/token/0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48)

And here is the token for ApeCoin, the currency of the Bored Ape Yacht Club ecosystem

[https://etherscan.io/token/0x4d224452801aced8b2f0aebe155379bb5d594381](https://etherscan.io/token/0x4d224452801aced8b2f0aebe155379bb5d594381)

**Surprise 1: Mappings can only be declared as storage, you cannot declare them inside a function**

This may seem like a very odd restriction, but this has to do with how the Ethereum Virtual machine works. Blockchains in general don’t like hashmaps because of their unpredictable runtime. The following code is invalid.

```solidity

contract BrokenContract {

    function wontWork() 
        public 
        view {
            mapping(uint256 => uint256) someMap; 
            // This won't compile, mappings must be state variables
    }
}

```

**Surprise 2: Mappings cannot be iterated over**

There is no way to iterate over the keys of a mapping. Every key is technically valid, it just defaults to zero.

```solidity

contract BrokenContract {
    mapping(uint256 => uint256) public someMap;

    function wontWork() 
        public 
        view {
            for (uint256 key in someMap) {  
            // Corrected to valid Solidity syntax, though the logic still 
            // won't compile in Solidity
                // do something
            }
    }
}

```

**Surprise 3: Mappings cannot be returned**

The following code is invalid. Maps are not a valid return type for solidity functions.

```solidity

contract BrokenContract {
    mapping(uint256 => uint256) public someMap;

    function wontWork() 
        public 
        view 
        returns (mapping(uint256 => uint256)) {
            return someMap; // This will not compile, as mappings cannot be returned from public functions
    }
}

```

**Problems**

[SpecialNumbers](https://github.com/RareSkills/Solidity-Exercises/tree/main/SpecialNumbers)
