# Payable Functions

Up until this point, we’ve been using tokens to represent value, but what about Ether? Let’s introduce how a smart contract can interact with Ether.

```solidity

contract ExampleContract {
    function payMe() 
        public 
        payable {

    }

    function howMuchEtherIHave() 
        public 
        view 
        returns (uint256) {
            return address(this).balance;
    }
}

```

After you deploy this contract on Remix, you’ll note that the button for interacting with **payMe** turns Red. This means you can specify in the field above how much value (Ether) to send when you call the function.

![https://static.wixstatic.com/media/61a666_207085b0008a42d4b5dc8349cf7c802f~mv2.png/v1/fill/w_939,h_658,al_c,q_95,enc_auto/Payable%20Functions1.png](https://static.wixstatic.com/media/61a666_207085b0008a42d4b5dc8349cf7c802f~mv2.png/v1/fill/w_939,h_658,al_c,q_95,enc_auto/Payable%20Functions1.png)

You can send Ether in units of Wei, Gwei, Finney, or Ether.

One Wei is 1/10^18 Ether, One gwei is one billionth of an Ether, and one Finney is one tenth.

Let’s make this simple and just send one Ether.

When we click “howMuchEtherIHave” we actually get back

1000000000000000000

This doesn’t mean we created a ton of Ether out of thin air. Remember floats are not a thing on the blockchain, so Ether uses the same strategy for decimals that ERC20 tokens do. A unit of Ether is actually one Wei, and what we traditionally think of as one Ether is 10^18 Wei.

By the way, the `.balance` construction works on arbitrary addresses. A smart contract can determine how rich (or poor) you are with the following function

```solidity

function howMuchEtherYouHave() 
    public 
    view 
    returns (uint256) {
        return msg.sender.balance;
}

function howMuchEtherTheyHave(address them) 
    public 
    view 
    returns (uint256) {
        return them.balance;
}

```

10000000000000000000

Unless functions have the **payable** modifier, they will revert if they receive Ether.

Why have this construction? If someone wants to send us Ether, why not accept it?

This has been a subject of debate, but the general idea is that a function should constrained in such a way to have extremely well defined behavior. Anything outside of that should be restricted. The more constrained the behavior, the easier it is to reason about the smart contract’s functionality.

By the way, solidity provides a very convenient keyword for dealing with all the zeros involved with Ether. Both of these functions do the same thing, but one is more readable.

```solidity

function moreThanOneEtherV1() 
    public 
    view 
    returns (bool) {
        if (msg.sender.balance > 1 ether) {
            return true;
        }
        return false;
}

function moreThanOneEtherV2() 
    public 
    view 
    returns (bool) {
        if (msg.sender.balance > 10**18) {
            return true;
        }
        return false;
}

```

It is also valid to make a constructor payable, if you want your smart contract to begin life with privilege and a headstart. But you still need to explicitly send ether at construction time.

Just because a function is payable does not mean that the person calling the function has to send Ether.

```solidity

contract ExampleContract {

    constructor() payable {
        // begin life with money
    }
}
```

**Sending Ether**

It’s clear how to send Ether if you initiate the transaction from Remix, but what if another smart contract wants to send Ether?

You will use the **call function we described earlier, but with an extra “meta argument”. It may look strange at first, but you’ll get used to it.

```solidity

contract ReceiveEther {
    function takeMoney() 
        public 
        payable {

    }

    function myBalance() 
        public 
        view 
        returns (uint256) {
            return address(this).balance;
    }
}

contract SendMoney {
    constructor() 
        payable {

    }

    function sendMoney(address receiveEtherContract) 
        public 
        payable {
            uint256 amount = myBalance();
            (bool ok, ) = receiveEtherContract.call{value: amount}(
                abi.encodeWithSignature("takeMoney()")
            );
            require(ok, "transfer failed");
    }

    function myBalance() 
        public 
        view 
        returns (uint256) {
            return address(this).balance;
    }
}

```

Let’s break down what we are looking at here.

- call has a funny looking json-like object between call and the arguments. This is how ether is sent with a call. The “value” key determines the amount sent. This is zero by default
- We have a tuple with an empty second argument; (bool ok,). This means we are ignoring the return value of takeMoney(). If we don’t care about the return value, we use this construction.
- We still care if the transfer fails. So we have the require(ok) construction in place.

Some experiments

- Deploy both contracts, but supply Ether to SendMoney at construction time. View myBalance on both contracts before and after calling sendMoney
- Remove the “payable” modifier on takeMoney and see what happens (it should revert)
- Use ether when calling sendMoney and note how the balance receiveEther gets increased

**Payable functions cannot be view or pure**

Changing the Ether balance of a smart contract is a “state change” on the blockchain. It’s a permanent alteration that endures even after the transaction is finished, similar to updating a storage variable. Therefore, payable functions cannot be view or pure. The compiler won’t accept that.

**Practice Problems**

[PriceIsRight](https://github.com/RareSkills/Solidity-Exercises/tree/main/PriceIsRight)
