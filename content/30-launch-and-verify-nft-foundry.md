# Launch and verify an NFT with Foundry

In the video tutorial above, you put an NFT on Opensea using the Remix environment.

This is fine, but Remix is not ideal for production use.

In this chapter, we will show you how to

1. Set up Foundry with the NFT
2. Deploy it to the Sepolia testnet and verify it on Etherscan

If you’ve been doing the exercises, you should already have foundry installed, so let’s set it up.

Create a new folder; call it foundry-nft, cd to it, and run "forge init" in the empty folder.

**Copy and Paste the code**

Here’s the code for creating the NFT. Rename the Counter.sol file in the "src" directory to FoundryNFT.sol and paste this code.

```solidity

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

import "openzeppelin-contracts/contracts/token/ERC721/ERC721.sol";

contract FoundryNFT is ERC721 {
    uint256 public totalSupply = 0;

    constructor() ERC721("FoundryNFT", "FNFT") {}

    function mint() 
            external 
            payable {
                    // you can add a price check here if you like
                    totalSupply++;
                    _mint(msg.sender, totalSupply);
    }
}
```

**Installing Openzeppelin**

We import and inherit the Openzepplin ERC721 contract, so to install it use the following:

```solidity

forge install Openzeppelin/openzeppelin-contracts --no-commit
```

**Using remappings**

Coming from a Remix background, you’ll notice that the OpenZeppelin import path here differs. Remix doesn’t store libraries in the same location Foundry does. However, Foundry provides remappings to allow file lookups on import to be redirected to a different location.

To see all the available remappings, we run **forge remappings**.

We get this:

```solidity

ds-test/=lib/forge-std/lib/ds-test/src/
forge-std/=lib/forge-std/src/
openzeppelin-contracts/=lib/openzeppelin-contracts/
```

We can see the OpenZepplin remapping, so we don’t need to specify the full path to the OpenZeppelin contracts. We can use **openzeppelin-contracts/contracts/token/ERC721/ERC721.sol** instead of specifying the "lib/" directory first.

**Not using remappings**

If we were not using forge remappings, we would have to specify the full path to our files or directories.

For example, for the ERC721.sol file, we would do something like this to import it.

```solidity

import "lib/openzeppelin-contracts/contracts/token/ERC721/ERC721.sol";
```

To confirm this is still valid, run **forge build** and see if it compiles.

And it does

```solidity

[⠰] Compiling...
[⠃] Compiling 1 files with 0.8.17
[⠊] Solc 0.8.17 finished in 1.38s
Compiler run successful
```

Now undo the changes back to how it was.

### **Generating the remappings.txt file in one step**

### ****

This can be done with the following operation

```solidity

forge remappings > remappings.txt
```

**Change the file names**

We can further configure where the remappings point by creating a remappings.txt file in the project root directory and adding this line **@openzeppelin=lib/openzeppelin-contracts/contracts** to the file.

After doing this, we can use **import "@openzeppelin/token/ERC721/ERC721.sol"**; to import the ERC721 token instead of explicitly writing the full file path.

This remapping can be configured in the remappings.txt file if it points to the right path.

**Getting an Etherscan key**

When we deploy our NFT, we need an Etherscan API to verify the contract. This will let us easily connect to Etherscan to verify the smart contract using forge without going to the Etherscan website and following the process.

Head to [Etherscan](https://etherscan.io/), log in, and create an API key.

![https://static.wixstatic.com/media/c0c19a_067cda25e2fc45f0ab63891d4cba805a~mv2.png/v1/fill/w_939,h_485,al_c,q_95,enc_auto/FFZD9yO.png](https://static.wixstatic.com/media/c0c19a_067cda25e2fc45f0ab63891d4cba805a~mv2.png/v1/fill/w_939,h_485,al_c,q_95,enc_auto/FFZD9yO.png)

![https://static.wixstatic.com/media/c0c19a_82c2ee7c60194c388c184ded78721a82~mv2.png/v1/fill/w_939,h_485,al_c,q_95,enc_auto/Etherscan%20API%20Key.png](https://static.wixstatic.com/media/c0c19a_82c2ee7c60194c388c184ded78721a82~mv2.png/v1/fill/w_939,h_485,al_c,q_95,enc_auto/Etherscan%20API%20Key.png)

We have created U3D9IS6Z5E872VFS7M7AWR1SBA8786ZZ3Y as our API key. We’ll be using this API key later.

### **Creating a throwaway wallet**

**Never use the private keys to a hardware wallet or any wallet that holds significant value.**

To deploy the NFT contract to the testnet, we need a private key with test ether to sign the transaction.

For the sake of simplicity, we have created a throwaway wallet and funded it with some sepolia ether.

Here’s the private key of the wallet; **787ea4ec95ab4f4e66c4c4c387cd0b5fbbec84a9293db485fa5f86f490c157d4.**

****

(This shouldn’t be used as it is considered comprised already.)

### **Put the Etherscan key and private key of the wallet in your environment variables**

### ****

Now that we have both our API key and private key, the next step is to create a .env file in the project’s root directory and add this to it.

```solidity

ETHERSCAN_KEY=U3D9IS6Z5E872VFS7M7AWR1SBA8786ZZ3Y #you can replace this with your API key
PRIVATE_KEY=787ea4ec95ab4f4e66c4c4c387cd0b5fbbec84a9293db485fa5f86f490c157d4
```

**Make sure your .env file is included in your .gitignore so you don’t accidentally publish your private keys!**

****

**Run this script**

****

Run this script to deploy and verify the NFT contract on sepolia.

```solidity

source .env #load the environment variables

forge create --rpc-url https://rpc2.sepolia.org --private-key $PRIVATE_KEY --etherscan-api-key $ETHERSCAN_KEY --verify src/FoundryNFT.sol:FoundryNFT 
```

**View on Etherscan**

We have successfully deployed our NFT contract on the sepolia testnet.

- [The transaction hash](https://sepolia.etherscan.io/tx/0x292d6d15a8e7131306d52ad54b9c09464cc17cd2db94a2a707c4dd494d6382af)
- [The verified contract](https://sepolia.etherscan.io/address/0xfd967dc8e5a7451e7a1f1ee9321fb1b38eb0dcd6#code)
