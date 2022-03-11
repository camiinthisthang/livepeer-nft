# livepeer-nft

## Purpose

This guide will walk users through uploading and minting a video as an NFT using [Livepeer](https://livepeer.com/), a decentralized video transcoding platform that has a [streaming](https://livepeer.com/docs/api-reference/stream/overview) and [video on demand API](https://livepeer.com/docs/api-reference/vod/upload). 

### Prerequisites: 
- Have node installed on your machine.
- Have a .mp4 video on your machine to upload.
- Have a metamask wallet and Metamask broswer extension.
- Be connected to the Polygon network.

### Upload Your Video
1. Get an API key for Livepeer [here](livepeer.com/dashboard/developers/api-keys). 
2. Head to the directory where your video is stored - I'd suggest Desktop for east. 
3. Run the following command in your terminal to install the Livepeer CLI: ```npm install -g @livepeer/video-nft```
4. Run the following command in your terminal: ```video-nft```
5. Enter your Livepeer API key from step 1
6. Type in the name of the .mp4 you want to upload
7. Name your NFT.
8. Optionally customize the metadata for your video NFT by hitting ```Y``` when prompted
9. Once your video has successfully been uploaded you will get an object with fields, including your IPFS hash containing your metadata:

```json
{
  "videoFileCid": "Qmbo4CREdbLDTFEd3UFo4byTfes6YoN9CvBwSrMhnDqcpZ",
  "nftMetadataCid": "QmXLQqt5zztHNj3ByvvopC7xbhNrdNPDutJQJvaoSBJ9eQ",
  "videoFileUrl": "ipfs://Qmbo4CREdbLDTFEd3UFo4byTfes6YoN9CvBwSrMhnDqcpZ",
  "videoFileGatewayUrl": "https://ipfs.livepeer.com/ipfs/Qmbo4CREdbLDTFEd3UFo4byTfes6YoN9CvBwSrMhnDqcpZ",
  "nftMetadataUrl": "ipfs://QmXLQqt5zztHNj3ByvvopC7xbhNrdNPDutJQJvaoSBJ9eQ",
  "nftMetadataGatewayUrl": "https://ipfs.livepeer.com/ipfs/QmXLQqt5zztHNj3ByvvopC7xbhNrdNPDutJQJvaoSBJ9eQ"
}
```

<img width="700" alt="Screen Shot 2022-03-10 at 7 59 58 PM" src="https://user-images.githubusercontent.com/15346823/157799577-ce9a8634-dbfc-4be6-a3bc-79a4d78fad93.png">

10.  The last line after a successful upload will give you a URL where you can pass in your custom ERC721 contract address or use the contract provided by Livepeer. 
<img width="444" alt="Screen Shot 2022-03-10 at 8 06 39 PM" src="https://user-images.githubusercontent.com/15346823/157800229-7031f3ea-fcae-45a2-a764-cb3c0ca0721c.png">

### Mint your Video NFT

1. After navigating to the URL provided, you can leave the contract field blank and default to Livepeer's contract. 
2. Tried to hit the default contract but got this error:
<img width="472" alt="Screen Shot 2022-03-10 at 8 12 18 PM" src="https://user-images.githubusercontent.com/15346823/157806339-b54dd142-72b5-4353-892c-204ff32606a9.png">


### Deploy Your Own ERC721 Contract - Housekeeping

If you don't want to use Livepeer's contract, you can deploy your own contract. One thing to note is that you must deploy your contract to the Polygon network in order to use the UI to mint your NFT. 

We'll be walking through deploying an ERC721 Contract with [Alchemy](https://www.alchemy.com/), an RPC provider which runs the node to give you access to write to the blockchain. 

1. Sign up for a free account with Alchemy [here](https://www.alchemy.com/).
2. Once signed in, create an app in the dashboard. For network, select Polygon Mumbai net so that you don't have to spend real money deploying your smart contract.

>Once you're confident in your contract, you can repeat these steps and set it to Polygon mainnet.
<img width="726" alt="Screen Shot 2022-03-10 at 8 29 49 PM" src="https://user-images.githubusercontent.com/15346823/157802310-861bd661-8bd5-4410-9e79-91a40844a083.png">
<img width="797" alt="Screen Shot 2022-03-10 at 8 30 52 PM" src="https://user-images.githubusercontent.com/15346823/157802398-127b85a5-e97a-4f45-9970-79d3bb0db0e2.png">
3.  If you havn't already, connect your Metamask wallet to the Mumbai Testnet. To do that, open Metamask and click on the dropdown of networks at the top of the screen. Go down to ```Add Network``` and fill in the following information:

Metamask Network Parameters

Network Name: Mumbai Testnet

New RPC URL: https://rpc-mumbai.maticvigil.com

Chain ID: 80001

Currency Symbol: MATIC

Block Explorer URL: https://polygonscan.com/

<img width="577" alt="Screen Shot 2022-03-10 at 9 00 15 PM" src="https://user-images.githubusercontent.com/15346823/157805423-47bbf0d2-4724-4140-bc48-444822825cc6.png">

Double check that the network was added to your wallet:

<img width="300" alt="Screen Shot 2022-03-10 at 9 00 35 PM" src="https://user-images.githubusercontent.com/15346823/157805503-f392a691-5b83-430a-8500-04928509fbe1.png">

4. Get some fake MATIC from a facuet. In order to deploy your contract to the Polygon blockchain, your wallet address needs to have some MATIC to make the transaction. Because we're on the Mumbai Testnet, we'll use fake MATIC which you can get from [a faucet like this one](https://faucet.polygon.technology/). 
- Copy your wallet address from Metamask
<img width="340" alt="Screen Shot 2022-03-10 at 9 09 34 PM" src="https://user-images.githubusercontent.com/15346823/157806186-543451c6-bf1d-4009-8bc7-0a245efe0b4d.png">
- Select Mumbai Network, MATIC Token, and paste in your wallet address
<img width="543" alt="Screen Shot 2022-03-10 at 9 09 21 PM" src="https://user-images.githubusercontent.com/15346823/157806220-3c512ea1-d39f-4786-ab87-458aeb91341f.png">
- Wait 1-2 mins for the MATIC to show up in your wallet
<img width="452" alt="Screen Shot 2022-03-10 at 9 09 50 PM" src="https://user-images.githubusercontent.com/15346823/157806251-926fe94e-4cd8-4bab-87aa-a277e7fb5b29.png">

### Deploy Your Own ERC721 Contract - The Fun Part!

Now that we're all set up, we're ready to start coding our smart contract. 

1. In your terminal, navigate to your Desktop (or whatever directory you want to use to house this project's code.)
2. Create a new folder and then move into that new directory by running the following commands, one after the other:
```mkdir project-name-here```
```cd project-name-here```
3. Initalize your project: ```npm init```
4. Enter the information requested, but it doesn't really matter how you answer these. Approve the package.json at the end and then you're good to go!
5. Download [Hardhat](https://hardhat.org/getting-started/#overview), a devleopment envirtonment to compile and deploy your smart contracts by running this command in the terminal in your project directory: ```npm install --save-dev hardhat```
6. Create a Hardhat project by running the following command in your project directory: ```npx hardhat```
7. Select ```Create an empty hardhat.config.js```
<img width="494" alt="Screen Shot 2022-03-10 at 9 43 24 PM" src="https://user-images.githubusercontent.com/15346823/157809401-8dcf02ac-5027-4e8b-8407-9e7def52bd64.png">
8. Create a contracts and scripts folder in your project. This is where we will create the smart contract file as well as the scripts to test and deploy the smart contract. You can do this by running the following commands in your project directory, one after the other:
```mkdir contracts
mkdir scripts
```
9. Open this project in an IDE like VSCode and navigate to the contracts folder. Crate a new file, we'll call it videoNFT.sol

10. Our smart contract code will be based on the OpenZepplin ERC721 implementation. To get access to the classe from OpenZepplin contracts, run the following command in the terminal in your project directory: ```npm install @openzeppelin/contracts@3.1.0-solc-0.7```

11. >>NEED TO FIGURE OUT HOW TO GET RID OF THIS ERROR, WHICH PACKAGE DO I NEED TO INSTALL TO GET ACCESS TO THESE CLASSES<

<img width="598" alt="Screen Shot 2022-03-10 at 9 48 17 PM" src="https://user-images.githubusercontent.com/15346823/157809911-829b8344-b20c-4b50-8c6a-3846e8a3ba72.png">

``` solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";

contract VideoNFT is ERC721URIStorage {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;

    constructor() ERC721("Video NFT", "VIDEO") {}

    event Mint(
        address indexed sender,
        address indexed owner,
        string tokenURI,
        uint256 tokenId
    );

    function mint(address owner, string memory tokenURI)
        public
        returns (uint256)
    {
        require(
            owner == msg.sender,
            "Can only mint NFT for yourself on default contract"
        );
        _tokenIds.increment();

        uint256 newItemId = _tokenIds.current();
        _mint(owner, newItemId);
        _setTokenURI(newItemId, tokenURI);

        emit Mint(msg.sender, owner, tokenURI, newItemId);
        return newItemId;
    }

    function uri(uint256 tokenId) public view returns (string memory) {
        return tokenURI(tokenId);
    }
}
```

### Deploying Your Contract

Our contract is all done, now we have to deploy it. In order to perform any transaction on the blockchain, you need to sign the signature using your private key. Because we want to sign a tranaction from our smart contract, we need to give our program access to this key. You don't want to expose this private key to anyone when you push your project up to Github. We can give our app access to this key while protecting it by using something like [Dotenv](https://www.npmjs.com/package/dotenv) which loads a module that loads environemnt variables from a .env file. 

1. Install the dotenv package into your project by running the following command in the terminal: ```npm install dotenv --save```
2. Copy your app's HTTP URL API from Alchemy.
<img width="744" alt="Screen Shot 2022-03-10 at 8 36 06 PM" src="https://user-images.githubusercontent.com/15346823/157810981-512037b5-dc24-4975-a487-8e3197b60ede.png">

3. Export your private key from Metamask. Instructions for how to do that can be found [here](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key).
4. Go to your newly created .env file in your project in your code editor and update the file to look like this:
```
API_URL = "https://polygon-mumbai.g.alchemy.com/v2/your-http-here"
PRIVATE_KEY = "your-metamask-private-key-here"
```
5. Instal [Ether.js](https://docs.ethers.io/v5/), a library for interacting with the blockchain: ```npm install --save-dev @nomiclabs/hardhat-ethers "ethers@^5.0.0"```
6. Update your hardhat.config.js file to tell your hardhat environment how to find your private keys, as well as update our dependecies. 
``` json
/**
* @type import('hardhat/config').HardhatUserConfig
*/
require('dotenv').config();
require("@nomiclabs/hardhat-ethers");
const { API_URL, PRIVATE_KEY } = process.env;
module.exports = {
   solidity: "0.7.3",
   defaultNetwork: "matic",
   networks: {
      hardhat: {},
      matic: {
         url: API_URL,
         accounts: [`0x${PRIVATE_KEY}`]
      }
   },
}
```
7. Now that everything is configured, compile your contract by running this command in the terminal: ```npx hardhat compile```
8. Write the script to deploy your smart contract to the network. You can do this by creating afile inside the scripts folder called ```deploy.js```
``` js
async function main() {
   // Grab the contract factory 
   const MyNFT = await ethers.getContractFactory("MyNFT");

   // Start deployment, returning a promise that resolves to a contract object
   const myNFT = await MyNFT.deploy(); // Instance of the contract 
   console.log("Contract deployed to address:", myNFT.address);
}

main()
  .then(() => process.exit(0))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
  ```
9. You're finally ready to execute this script and deploy your contract. You can do that by running the following command in your terminal: 
```npx hardhat run scripts/deploy.js --network matic```
10. Once your contract is deployed, you'll get the address of where your contract lives. Copy this address and paste it in the Livepeer UI we walked through earlier. 
