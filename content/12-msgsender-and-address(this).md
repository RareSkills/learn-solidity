# msg.sender and address(this)

Remember our previous example of a bad ERC20 token?

Here it is again

```solidity

contract ERC20Token {

    mapping(address => uint256) public balances;

    function setSomeonesBalance(
        address owner, 
        uint256 amount
    ) 
        public {
            balances[owner] = amount;
    }

    function transferTokensBetweenAddresses(
        address sender, 
        address receiver, 
        uint256 amount
    ) 
        public {
            balances[sender] -= amount;   // deduct/debit the sender's balance
            balances[receiver] += amount; // credit the receiver's balance
    }
}

```

The issue is that we have no idea who is calling the function.

Luckily, Solidity has a mechanism to identify who is calling the smart contract: **msg.sender**. msg.sender returns the address of who is invoking the smart contract function.

Try out the following code in remix:

```solidity

contract ExampleContract {
    function whoami()
        public
        view 
        returns (address) {
            address sender = msg.sender;
            return sender;
    }
}
```

It will return the test address you are using in remix.

Now change the test address by hitting the "ACCOUNT" dropdown. Then try the function again. The address returned will be different.

![https://static.wixstatic.com/media/61a666_6911916215e746e4be2a25bdbcf2e9ac~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/msgsender.png](https://static.wixstatic.com/media/61a666_6911916215e746e4be2a25bdbcf2e9ac~mv2.png/v1/fill/w_939,h_610,al_c,q_95,enc_auto/msgsender.png)

By combining msg.sender with an if statement, you can give certain addresses special privileges.

Let’s say we want the default address in remix to be the special address.

```solidity

contract ERC20Token {
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

    function transferTokensBetweenAddresses(
        address sender, 
        address receiver, 
        uint256 amount
    ) 
        public {
            if (msg.sender == banker) {
                balances[sender] -= amount;   // deduct/debit the sender's balance
                balances[receiver] += amount; // credit the reciever's balance
            }
            // do nothing
    }
}

```

The code above lets people view their balances (because balances is a public variable), but only the banker can change it.

By being a little bit clever, we can actually allow people to transfer their balance to someone else without the banker doing it for them. Consider the following example.

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

The function **transfer** can be called by anyone. However, it can only debit (deduct) balances from **msg.sender**. As an exercise for the reader, I encourage you to think about why it is impossible to steal someone else’s balance using **transfer**.

A natural question is, what happens if someone tries to send more amount than they have balance for? If you are using Solidity 0.8.0 or higher, nothing happens. The transaction reverts because you cannot subtract an unsigned number such that it becomes negative.

**tx.origin**

There is another mechanism to get the sender, **tx.origin**. Although it behaves similarly to msg.sender, you should not use it. To avoid bombarding you with too much information right now, we won’t explain the security issues around **tx.origin** yet. But the important point is, do not use tx.origin except in very specific circumstances.

**address(this)**

A smart contract can know its own address with the following code

```solidity

contract ExampleContract {

    function whoami()
        public
        view 
        returns (address) {
            return address(this);
    }
}
```

Try it out in Remix and see the address matches

![https://static.wixstatic.com/media/61a666_055fd70f34e24119a3e16f51820845f3~mv2.png/v1/fill/w_939,h_275,al_c,q_95,enc_auto/addressmsg.png](https://static.wixstatic.com/media/61a666_055fd70f34e24119a3e16f51820845f3~mv2.png/v1/fill/w_939,h_275,al_c,q_95,enc_auto/addressmsg.png)

**Practice Problems**

[WhoCalledMe](https://github.com/RareSkills/Solidity-Exercises/tree/main/WhoCalledMe)
