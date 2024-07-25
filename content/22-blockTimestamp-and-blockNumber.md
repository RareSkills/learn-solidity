## Block.timestamp and Block.number

We’ve been able to do some cool stuff up to this point, but we haven’t been able to track the passage of time.

You can get the unix timestamp on the block with the **block.timestamp.** Let’s try it

```solidity

contract WhatTimeIsIt {

    function timestamp() 
        public 
        view 
        returns (uint256) {
            return block.timestamp;
    }
}

```

Try it out in Remix.

The number that comes back is the number of seconds since January 1, 1970 UTC, the traditional unix time. Remember, this is **seconds** not **miliseconds** as your linux desktop or other programming languages might respond with.

Ethereum progresses with blocks, and whichever timestamp you get back is what the validator put into the block when they produced it. Since blocks are produced every 12 seconds, the block.timestamp will roughly increment by that amount. You shouldn’t trust block.timestamp on the order of second intervals. There’s too much variation. Over the course of minutes however, it is quite reliable.

If you want to ensure someone doesn’t call a function more than once per day, you can use the following construction

```solidity

contract ExampleContract {

    uint256 public lastCall;

    function hasCooldown() 
        public {
            uint256 day = 60 * 60 * 24;
            require(
                block.timestamp > lastCall + day, 
                "hasn't been a day"
            );
            lastCall = block.timestamp;
    }
}

```

Solidity has a much nicer way to represent time instead of multiplying seconds like that.

```solidity

contract ExampleContract {

    uint256 public lastCall;

    function hasCooldown() 
        public {
            require(
                block.timestamp > lastCall + 1 days, 
                "hasn't been a day"
            );
            lastCall = block.timestamp; // update the last time the function was called
    }
}

```

In fact, seconds, minutes, hours, days, and weeks are all valid units of time, which are just handy shortcuts for multiplying out the number of seconds you need. In case you were wondering, seconds doesn’t change the value, but it does provide readability if you intend to use seconds as a measure.

**block.number**

You can also know what block number you are on with this variable. Hopefully it’s obvious what it does. Some people mistakenly multiply the average blocktime by the block.number to measure the passage of time. Don’t do that.

**Don’t use block.number to track time, only to enforce ordering of transactions.**

When do you need to enforce transaction order? Not often. So if you aren’t sure, use block.timestamp.

Etherscan shows the current blocknumber, if you want to have an idea of how large it currently is.

![https://static.wixstatic.com/media/61a666_a8d6fe27dfa944d696b31610c4b138b4~mv2.png/v1/fill/w_939,h_1250,al_c,q_95,enc_auto/block_timestamp_number.png](https://static.wixstatic.com/media/61a666_a8d6fe27dfa944d696b31610c4b138b4~mv2.png/v1/fill/w_939,h_1250,al_c,q_95,enc_auto/block_timestamp_number.png)

```solidity

contract ExampleContract {

    function whatBlockIsIt() 
        external 
        view 
        returns (uint256) {
            return block.number;
    }
}

```

The code above will tell you which block the transaction happened on. In this case, it will update dynamically.

If you want to enforce that a function is called after another one, that is, in a later block, you can use the following construction.

```solidity

contract ExampleContract {

    // defaults to zero
    uint256 private calledAt;

    function callMeFirst() 
        external {
            calledAt = block.number;
    }

    function callMeSecond() 
        external {
            require(
                calledAt != 0 && block.number > calledAt, 
                "callMeFirst() not called"
            );
    }
}

```

**Practice Problems**

[OneWeekLockup](https://github.com/RareSkills/Solidity-Exercises/tree/main/OneWeekLockup)

[IdiotBetting](https://github.com/RareSkills/Solidity-Exercises/tree/main/IdiotBetting)

[TimeLockEscrow](https://github.com/RareSkills/Solidity-Exercises/tree/main/TimelockEscrow)

[ReducingPayout](https://github.com/RareSkills/Solidity-Exercises/tree/main/ReducingPayout)

[BlockNumber](https://github.com/RareSkills/Solidity-Exercises/tree/main/BlockNumber)
