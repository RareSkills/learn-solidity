## Unit Testing in Solidity

It's about time you learned how to write unit tests! We've already written a tutorial on unit testing, so no need to repeat it on this page

Read it here: Solidity Unit Testing (please link "Solidity Unit Testing" here: [https://www.rareskills.io/post/foundry-testing-solidity](https://www.rareskills.io/post/foundry-testing-solidity))

Homework:

- Unit test your NFT. Make sure that when you mint, the ownerOf the NFT is the address that minted it. Also check that "balanceOf" for that address becomes 1.
- Check that the balance of the contract went up by the price of the NFT
- When the owner calls withdraw, check that their Ether balance went up by the expected amount
