# Emitting Events

Technically, our “ERC20” token is not fully ERC20 compliant. It’s missing an important feature: **events**.

General rule of thumb: **If a function causes a state change, it should be logged.**

****

Why log things? Isn’t it the case that the blockchain already immutably stores every transaction?

This is true, events are not strictly necessary. However, they make auditing past events a lot easier. Rather than combing through a bunch of transactions, the user can filter by the log that they care about and quickly find events (transactions) that might be of interest.

This is how your cryptocurrency wallet can quickly discover your ERC20 balance. It would be pretty annoying to have to look through every transaction that ever occurred on an ERC20 token to discover if you own any. But logs are stored in such a way that this retrieval is efficient.

**Events cannot be seen by other smart contracts**. They are optimized for being queried offchain.

Let’s look at an example.

```solidity

contract ExampleContract {

    event Deposit(address indexed depositor, uint256 amount);

    receive() 
        external 
        payable {
            emit Deposit(msg.sender, msg.value);
    }
}

```

An event can have up to 3 indexed types, but there isn’t a strict limit on the number of unindexed parameters.

If you have a database background, you can think of “indexes” exactly the same way you would about a database index.

By the way, argument names after the datatype is optional. We could have written the event above as

```solidity

event Deposit(address indexed, uint256);
```

with no ill effects, except that perhaps it is a bit less readable.

When should a variable be indexed or not? If you might be interested in finding that value quickly, like “has an address been involved with this token contract” then you should index it. You probably are not interested in the question “has anyone ever transferred exactly 1,370,904 tokens in this contract?”, so don’t index the amount. Here is our ERC20 token with the events added. Note that these events are required by the [specification](https://eips.ethereum.org/EIPS/eip-20).

Pay close attention to where the events have been added, especially the mint function! The convention of address(0) being the source means the tokens came into existence out of nothing, rather than from another address. Recommended reading: [https://www.rareskills.io/post/ethereum-events](https://www.rareskills.io/post/ethereum-events)

```solidity

contract ERC20 {
    string public name;
    string public symbol;

    mapping(address => uint256) public balanceOf;
    address public owner;
    uint8 public decimals;

    uint256 public totalSupply;

    // owner -> spender -> allowance
    // this enables an owner to give allowance to multiple addresses
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(
        address indexed _from, 
        address indexed _to, 
        uint256 _value
    );
    event Approval(
        address indexed _owner, 
        address indexed _spender, 
        uint256 _value
    );

    constructor(
        string memory _name, 
        string memory _symbol
    ) {
        name = _name;
        symbol = _symbol;
        decimals = 18;

        owner = msg.sender;
    }

    function mint(
        address to, 
        uint256 amount
    ) 
        public {
            require(msg.sender == owner, "only owner can create tokens");
            totalSupply += amount;
            balanceOf[owner] += amount;

            emit Transfer(address(0), owner, amount);
    }

    function transfer(
        address to, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            return helperTransfer(msg.sender, to, amount);
    }

    function approve(
        address spender, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            allowance[msg.sender][spender] = amount;
            emit Approval(msg.sender, spender, amount);

            return true;
    }

    function transferFrom(
        address from, 
        address to, 
        uint256 amount
    ) 
        public 
        returns (bool) {
            if (msg.sender != from) {
                require(
                    allowance[from][msg.sender] >= amount, 
                    "not enough allowance"
                );

                allowance[from][msg.sender] -= amount;
            }

            return helperTransfer(from, to, amount);
    }

    function helperTransfer(
        address from, 
        address to, 
        uint256 amount
    ) 
        internal 
        returns (bool) {
            require(balanceOf[from] >= amount, "not enough money");
            require(to != address(0), "cannot send to address(0)");
            balanceOf[from] -= amount;
            balanceOf[to] += amount;

            emit Transfer(from, to, amount);
            return true;
    }
}

```

**Practice Problems**

[Emitter](https://github.com/RareSkills/Solidity-Exercises/tree/main/Emitter)
