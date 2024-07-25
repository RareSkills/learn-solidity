## Setup your Environment

If you have not yet set up cURL on your system you can visit the following link to do so:

[cURL setup](https://help.ubidots.com/en/articles/2165289-learn-how-to-install-run-curl-on-windows-macosx-linux)

```solidity

# install foundry
curl -L https://foundry.paradigm.xyz | bash

# clone the practice problems
git clone https://github.com/rareSkills/solidity-exercises.git

# go to the first problem
cd solidity-exercises/EverythingWorks

# test the setup
forge test
```

You should see output that looks like the following

```solidity

[⠆] Compiling...
[⠘] Compiling 1 files with 0.8.19
[⠃] Solc 0.8.19 finished in 457.65ms
Compiler run successful

Running 1 test for test/Everythingworks.t.sol:EverythingWorksTest
[PASS] testIncrement() (gas: 302)
Test result: ok. 1 passed; 0 failed; finished in 3.47ms
```

**What exactly is “forge” and “foundry” here?**

You can think of it like gulp or webpack for JavaScript or maven for Java or tox for Python. Foundry is a development framework to make testing, development, and deployment easier. Without a doubt, it is the most popular framework in 2023, and absolutely worth knowing as a Solidity developer.

One thing that is really cool about it is that you can write unit tests in Solidity, so that makes testing easier. Previous tools used JavaScript, which forced context switching between languages and made casting types a little bit tricky.

**VS Code Extension**

If you haven’t already downloaded the following extension, you should!

![Untitled](Free%20Solidity%20Tutorial%20(Latest)%20b6bc0ae51248409cacd0bef4c26c3d8d/Untitled.png)
