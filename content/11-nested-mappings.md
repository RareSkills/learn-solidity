# Nested Mappings

In most languages, a hashmap can contain another hashmap, and Solidity does this too. However, because mappings are not valid return types, you must supply all the keys the maps require.

Let’s look at an example

```solidity

contract ExampleContract {

    mapping(uint256 => mapping(uint256 => uint256)) public nestedMap;

    function setNestedMap(
        uint256 key1, 
        uint256 key2, 
        uint256 finalValue
    ) 
        public {
            nestedMap[key1][key2] = finalValue;
    }

    function getNestedMap(
        uint256 key1, 
        uint256 key2
    ) 
        public 
        view 
        returns (uint256) {
            return nestedMap[key1][key2];
    }
}

```

Nested maps are quite common in smart contracts, unlike nested arrays. For example, you can do bookkeeping like this

```solidity

contract TrackDebt {

    mapping(address => mapping(address => uint256)) public amountOwed;

    function setAmountOwed(
        address borrower, 
        address lender, 
        uint256 amount
    ) 
        public {
            amountOwed[lender][borrower] = amount;
    }
}

```

Note that the order matters here. In this construction, one lender can have several borrowers. If we had set borrower to be the first key, it would imply a borrower might have mutiple debtors.

The same restrictions that apply to regular mappings apply to nested mappings. You cannot iterate over the keys, declare them inside a function, or return them from a function.

**Public Nested Mappings Don’t Work**

Here’s yet another strange quirk of Solidity. Solidity automatically creates getting functions for variables when you declare them as public. However, the public getter functions allow you to supply the necessary arguments.

Yes. You read that right.

The solution is to make nested mappings private and wrap them in a public function that gets their value. Time to practice this!

**Practice Problems**

[NestedMapping](https://github.com/RareSkills/Solidity-Exercises/tree/main/NestedMapping)

[TripleNestedMapping](https://github.com/RareSkills/Solidity-Exercises/tree/main/TripleNestedMapping)
