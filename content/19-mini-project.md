# Token Exchange mini project

**This is challenging**

Believe it or not, you now have enough prerequisite knowledge to build a very simple token exchange smart contract! Here is your mission.

Build two ERC20 contracts: **RareCoin** and **SkillsCoin** (you can change the name if you like). Anyone can mint SkillsCoin, but the only way to obtain RareCoin is to send SkillsCoin to the RareCoin contract. You’ll need to remove the restriction that only the owner can mint SkillsCoin.

Here is the workflow

- **mint()** SkillsCoin to yourself
- **SkillsCoin.approve(address rareCoinAddress, uint256 yourBalanceOfSkillsCoin)** RareCoin to take coins from you.
- **RareCoin.trade()** This will cause RareCoin to **SkillsCoin.transferFrom(address you, address RareCoin, uint256 yourBalanceOfSkillsCoin)** Remember, RareCoin can know its own address with address(this)
- **RareCoin.balanceOf(address you)** should return the amount of coin you originally minted for SkillsCoin.

Remember **ERC20 tokens**(aka contract) **can own other ERC20 tokens**. So when you call RareCoin.trade(), it should call SkillsCoin.transferFrom and transfer your SkillsCoin to itself, I.e. address(this).

If you have the SkillsCoin address stored, it would look something like this

```solidity

function trade(uint256 amount) 
    public {
        // some code
        // you can pass the address of the deployed SkillsCoin contract as a parameter 
        // to the constructor of the RareCoin contract as 'source'
        (bool ok, bytes memory result) = source.call(
            abi.encodeWithSignature(
                "transferFrom(address,address,uint256)", 
                msg.sender, 
                address(this), 
                amount
            )
        );
        // this will fail if there is insufficient approval or balance
        require(ok, "call failed");
        // more code
}

```

Deploy these contracts in Remix and test that they work.

If you are new to Solidity, set aside a couple days for this. A lot of engineers get weirded out by the fact that balances are stored in smart contracts, not wallets, so it takes a bit of time to get used to this. Also, trust me, you will get confused by the cross contract calls.
