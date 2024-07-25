## Units of Ethereum: wei, gwei, and ether

The following two lines of code are equivalent

```solidity
(bool ok, ) = recipient.call{value: 1 ether}("");

(bool ok, ) = recipient.call{value: 1_000_000_000_000_000_000}("");
```

If you feel like counting, that’s 10^18. Remember, there aren’t floats in Solidity, so “1 Ethereum” is actually 10^18 units of its smallest unit.

The smallest unit of Ether is 1 wei. 10^18 wei is 1 Ether.

Another commonly used unit is gwei. One gwei is 1 billion wei, or 1 billionth of an Ether.

Remix doesn’t let you specify a fraction of an Ether when sending value, so you’ll have to calculate the amount from the fraction and convert it to wei or gwei.

By the way, even though floats are not supported in Solidity, you can specify fractions of an Ether. The solidity compiler is smart enough to understand that a fraction of an Ether isn’t a fraction itself. The following lines of code are equilvalent

```solidity

(bool ok, ) = recipient.call{value: 1 gwei}("");

(bool ok, ) = recipient.call{value: 1_000_000_000 wei}("");

(bool ok, ) = recipient.call{value: 0.000000001 ether}("");
```

By the way, Ether has other names for units, like Finney and Sazbo, but these units are so rarely used, it’s better to just Google them when you need to know their value. The following values must be memorized however:

- wei is the smallest unit of Ether.
- 10^18 wei is 1 Ether
- One gwei is one billion wei, or one billionth of an Ether.
