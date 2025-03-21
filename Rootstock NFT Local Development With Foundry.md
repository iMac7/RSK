# Rootstock NFT Local Development With Foundry

## Requirements
[Foundry](https://book.getfoundry.sh/getting-started/installation)

## Setting up the development environment
Clone the starter kit at this repo
```sh
git clone https://github.com/rsksmart/rootstock-foundry-starterkit.git
```
Navigate into the folder
```sh
cd rootstock-foundry-starterkit
```
![image](https://github.com/user-attachments/assets/20a4e53b-d186-4e63-ab75-66c8d6547e4f)


-/lib holds the project's dependencies such as openzeppelin contracts
-/script holds scripts such as deployment scripts (more on this later)
-/src holds the main contracts for the project
-/test holds test contracts to ensure the ones in /src above work correctly
-Other folders can be ignored for now, some are  auto generated by foundry (/cache, /out, /broadcast)

### Create the contracts

Install openzeppelin-related dependencies
```sh
forge install openzeppelin-contracts-05=OpenZeppelin/openzeppelin-contracts@v2.5.0 openzeppelin-contracts-06=OpenZeppelin/openzeppelin-contracts@v3.4.0 openzeppelin-contracts-08=OpenZeppelin/openzeppelin-contracts@v4.8.3 --no-commit
```
Delete the files in /src and /test folders and we can now start creating the contracts.
Add a new file to the /src folder, we can call that MyNFT.sol . This will contain all the code for the nft contract to be deployed. 

##### src/MyNFT.sol
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;
import {ERC721} from "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import {ERC721URIStorage} from "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";

contract MyNFT is ERC721, ERC721URIStorage, Ownable {
    uint256 private _nextTokenId;

    constructor(address _owner)
        ERC721("MyToken", "MTK")
        Ownable(_owner)
    {}

    function safeMint(address to, string memory cid)
        public
        onlyOwner
        returns (uint256)
    {
        uint256 tokenId = _nextTokenId++;
        _safeMint(to, tokenId);
        string memory uri = string(abi.encodePacked("ipfs://", cid));
        _setTokenURI(tokenId, uri);
        return tokenId;
    }

    // The functions below are overrides required by Solidity.
    function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }

    function supportsInterface(bytes4 interfaceId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (bool)
    {
        return super.supportsInterface(interfaceId);
    }
}
```
Note that the safeMint function takes in the hash of a file already uploaded to a storage service. For local development, proceed with any valid string.

Next we need to add a couple tests to ensure that the contract is working as expected. Go to the /test folder in the root directory and create a new file, call it MyNFT.t.sol .

##### MyNFT.t.sol
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import "../src/MyNFT.sol";

contract MyNFTTest is Test {
  MyNFT nft;

  address owner = address(0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266);
  address recipient = address(0x70997970C51812dc3A010C7d01b50e0d17dc79C8);

  function setUp() public {
    vm.label(owner, "Owner");
    vm.label(recipient, "Recipient");
    vm.startPrank(owner);
    nft = new MyNFT(owner);
    vm.stopPrank();
  }

  // Minting is possible and a token uri is correctly assigned to the minter
  function testMintNFT() public {
    vm.startPrank(owner);

    string memory sampleHash = "sample-metadata-hash";    
    string memory sampleTokenURI = "ipfs://sample-metadata-hash";
    uint256 tokenId = nft.safeMint(recipient, sampleHash);

    assertEq(nft.ownerOf(tokenId), recipient);
    assertEq(nft.tokenURI(tokenId), sampleTokenURI);

    vm.stopPrank();
  }

  // Only the owner can mint NFTs
  function testOnlyOwnerCanMint() public {
    vm.startPrank(recipient);
    string memory sampleHash = "sample-metadata-hash";    
    vm.expectRevert();
    nft.safeMint(recipient, sampleHash);
    vm.stopPrank();
  }
}
```
The test addresses used above come from anvil, a locally running blockchain node which is part of th foundry toolkit. Open a new terminal and run
```sh
anvil
```

![image](https://github.com/user-attachments/assets/265b27ba-9b4f-4b89-adc1-d54f5a5871a1)

We now have a bunch of account addresses and private keys to test the functionality of the contracts.

### Build and test locally

Run tests before deploying the contract locally to anvil.
```sh
forge build
forge test
```
In case of compilation issues, running `forge clean` then `forge build` again might work.

![image](https://github.com/user-attachments/assets/2ad20eeb-500f-4d64-bb6c-19886e369246)


Go to the /script folder and create a new file, Deploy.s.sol .
Create a file .env in the root folder and add a variable PRIVATE_KEY. Go back to the terminal where anvil is running and get a private key to use here.
##### .env
```
PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```
Create the deploy script using the environment variable above.
##### Deploy.s.sol
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

import "forge-std/Script.sol";
import "../src/MyNFT.sol";

contract DeployMyNFT is Script {
  function setUp() public {}

  function run() public {
    uint256 deployerPrivateKey = vm.envUint("PRIVATE_KEY");
    vm.startBroadcast(deployerPrivateKey);

    MyNFT nft = new MyNFT(address(0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266));
    
    vm.stopBroadcast();
  }
}
```
Use Forge from foundry to deploy MyNFT contract. First setup a deployment script, this is a contract which holds reusable code which can be used to perform various functions, in this case, to put our contract on the anvil blockchain (running on the fork-url below 127.0.0.1:8545). The output should give you a contract address to interact with.
```sh
forge script script/Deploy.s.sol --fork-url http://127.0.0.1:8545 --broadcast
```
![image](https://github.com/user-attachments/assets/02643f0e-8189-4fd1-a068-3c812fe2600a)

On the anvil blockchain, the transaction should have registered as well.

![image](https://github.com/user-attachments/assets/7685895c-5337-4ef8-93e6-731f31fa610e)


### Mint the first nft

Cast is another tool that comes in bundled with foundry, used to interact with smart contracts on a blockchain. We can now call the contract's functions with the address we just got above.
Copy your private key from the .env file and add it to your system environment variables. Alternatively, pass it as it is, but this is easier.
```sh
export PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
```

Mint the NFT. Make sure this is the same address used in the deployment script because of the `OnlyOwner` modifier in the contract which only allows the owner to mint.
```sh
cast send <contract_address> "safeMint(address,string)" <owner_address> "sample-metadata-hash" --rpc-url http://127.0.0.1:8545 --private-key $PRIVATE_KEY
```
![image](https://github.com/user-attachments/assets/4afdba00-abb0-42e1-919b-4c5772a6f2d8)

Successfully minted! Let's check if I am indeed the owner of the token.
```sh
cast call <contract_address> "ownerOf(uint256)(address)" 0 --rpc-url http://127.0.0.1:8545
```
![image](https://github.com/user-attachments/assets/ca772193-25e8-4d97-a65e-cd26e251e661)

Confirm if the token uri is right
```sh
cast call <contract_address> "tokenURI(uint256)(string)" 0 --rpc-url http://127.0.0.1:8545
```
![image](https://github.com/user-attachments/assets/d885c6aa-7e8d-453c-a8f8-745be1844cee)

Oops! token uri concatenated twice! Pat yourself on the back if ou caught that :)












