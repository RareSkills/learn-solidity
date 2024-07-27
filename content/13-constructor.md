# Constructor

Going back to our rolling ERC20 example, we did something a little weird, we set the banker variable directly in the contract.

```solidity

contract ERC20 {
    address public banker = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;

    mapping(address => uint256) public balances;

    function setSomeonesBalance(
        address owner, 
        uint256 amount
    ) 
        public {
            if (msg.sender == banker) {
                balances[owner] = amount;
            }
            // do nothing
    }

    function transfer(
        address receiver, 
        uint256 amount
    ) 
        public {
            balances[msg.sender] -= amount;
            balances[receiver] += amount;
    }
}

```

That’s okay, but what if someone wants to deploy the contract and set themselves to be the banker?

Smart contracts have a special function that is called at deployment time called the constructor. This is pretty similar to other object oriented programming languages. Here is what it looks like

```solidity

contract ExampleContract {

    address public banker;

    constructor() {
        deployer = msg.sender;
    }
}
```

Note that it’s “constructor()” and not “function constructor()” and we don’t specify **public** because constructors can’t be modified with things like pure, view, public, and so forth.

If you wanted the banker to be configured by the person deploying the contract, then you could use it as a function argument.

```solidity

contract ExampleContract {

    address public banker;

    constructor(address _banker) {
        banker = _banker;
    }
}
```

By the way, you’ll see this pattern variable = _variable a lot in constructors. Solidity doesn’t require you to do that, but it’s considered conventional.

When deploying a contract on Remix that has constructor arguments, you’ll have to put the arguments into the box that appears next to “deploy.”

![https://static.wixstatic.com/media/61a666_a136b53ad685419582210f02b620e762~mv2.png/v1/crop/x_0,y_0,w_2000,h_973/fill/w_939,h_457,al_c,q_95,enc_auto/Constructor.png](https://static.wixstatic.com/media/61a666_a136b53ad685419582210f02b620e762~mv2.png/v1/crop/x_0,y_0,w_2000,h_973/fill/w_939,h_457,al_c,q_95,enc_auto/Constructor.png)

**Unlike other functions, calldata cannot be used for arrays and strings, you must use memory**

Again, for reasons we cannot get into right now, calldata cannot be used in constructor arguments. I know, it seems like a very weird and random restriction, but it will make sense later, after you understand how Ethereum works under the hood.

Here is how you would set a string during construction time

```solidity

contract ExampleContract {
    string public name;

    // if you use calldata, it won't compile
    constructor(string memory _name) {
        name = _name;
    }
}
```

You may be tempted as a response to just use memory everywhere and not bothering to use calldata. But it is worth trying to remember this for now, because calldata results in cheaper transactions (I.e. lower gas fees for the user).

Also, in case you were wondering, **constructors cannot return values.**

**Problems**

[Deployer](https://github.com/RareSkills/Solidity-Exercises/tree/main/Deployer)
