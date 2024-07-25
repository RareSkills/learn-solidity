## Receive

It was a bit annoying to have to abi encode a function just to send Ether. Luckily, Solidity has a nice way to handle this.

```solidity

contract TakeMoney {

    receive() external payable {

    }
}
```

Note a couple new things:

- receive is a function, but it doesn’t have the function keyword. This is because it is a “special” function like the constructor, so to emphasize that, the “function” keyword is not included.
- We used a modifier **external** rather than **public**

Up until now, we’ve been using the modifier public whenever we wanted the function to be callable outside the contract. External means it can **only** be called outside the contract. For simplicity, we’ll be using **public** but we’ll get into the distinction between external and public more later. Solidity however, only allows the **receive** function to be external.

It must also be payable. Try deleting the payable keyword and compiling the contract. That won’t succeed.

Now, how can another function send ether to it?

```solidity

contract TakeMoney {
    receive() 
        external 
        payable {

    }

    function viewBalance() 
        public 
        view 
        returns (uint256) {
            return address(this).balance;
    }
}

contract ForwardMoney {
    function payMe() 
        public 
        payable {

    }

    function sendMoney(address luckyAddress) 
        public 
        payable {
            uint256 myBalance = viewBalance();
            luckyAddress.call{value: myBalance}("");
    }

    function viewBalance() 
        public 
        view 
        returns (uint256) {
            return address(this).balance;
    }
}

```

Now plug this into remix to test it out.

This is a lot cleaner. No abi encoding or anything.

This construction is also how we send money to **wallets**. Here is a contract that allows only one address to withdraw ether.

```solidity

contract SaveMoney {
    function withdrawMoney() 
        public 
        payable {
            require(msg.sender == 
                0x5B38Da6a701c568545dCfcB03FcB875f56beddC4, 
                "not the first remix address");
            msg.sender.call{value: viewBalance()}("");
    }

    function viewBalance() 
        public 
        view 
        returns (uint256) {
            return address(this).balance;
    }

    // anyone can send
    receive() 
        external 
        payable {

    }
}

```

It might seem strange that we are doing a “function call” to a wallet, which has no functions.

The convention is a bit confusing. Just remember this.

**All call invocations transfer ether. But zero is a valid amount of ether to transfer.**

In Ethereum, all function calls can transfer ether, even if the value parameter is not explicitly set. However, zero is a valid amount of ether to transfer, and can be useful in situations where no ether needs to be transferred.

**Practice Problems**

[Receive](https://github.com/RareSkills/Solidity-Exercises/tree/main/Receive)

[Donations](https://github.com/RareSkills/Solidity-Exercises/tree/main/Donations)

[Withdraw](https://github.com/RareSkills/Solidity-Exercises/tree/main/Withdraw)

[Distribute](https://github.com/RareSkills/Solidity-Exercises/tree/main/Distribute)

[DistributeV2](https://github.com/RareSkills/Solidity-Exercises/tree/main/DistributeV2)

[BasicBank](https://github.com/RareSkills/Solidity-Exercises/tree/main/BasicBank)

[BasicBankV2](https://github.com/RareSkills/Solidity-Exercises/tree/main/BasicBankV2)
