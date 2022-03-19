# Livepeer NFT Workshop

## Purpose

This guide will walk users through uploading and minting a video as an NFT using [Livepeer](https://livepeer.com/), a decentralized video transcoding platform that has a [streaming](https://livepeer.com/docs/api-reference/stream/overview) and [video on demand API](https://livepeer.com/docs/api-reference/vod/upload). 

### Prerequisites: 
- Have node installed on your machine.
- Have a .mp4 video on your machine to upload.
- Have a metamask wallet and Metamask broswer extension.
- Have Polygon/Mumbai Testnet AND MATIC in your Metamask.
>To learn how to add Mumbai Testnet to your wallet, follow [these instructions.](https://github.com/camiinthisthang/mumbai-testnet/blob/main/README.md)
- Have MATIC or test MATIC in your Metamask wallet.

### Upload Your Video
1. Get an API key for Livepeer [here](https://livepeer.com/dashboard/developers/api-keys). 
2. Head to the directory where your video is stored - I'd suggest Desktop for east. 
3. Run the following command in your terminal to install the Livepeer CLI: ```npm install -g @livepeer/video-nft``` [(source code)](https://github.com/livepeer/video-nft)
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
Prerequisite: Be sure to have your Metamask network configured to the Plolygon mainnet.

1. After navigating to the URL provided, you can leave the contract field blank and default to Livepeer's contract. Hit "Mint NFT"
2. You'll have to confirm the transaction and pay for the gas in Metamask. 
3. Done 🎉
<img width="494" alt="Screen Shot 2022-03-13 at 5 20 16 PM" src="https://user-images.githubusercontent.com/15346823/158077728-a5cd3e08-990d-423f-a4f3-23ccabf8a90b.png">
4. Hit the link provided to view your video NFT on Opensea. You can view the one in this demo [here.](https://opensea.io/assets/matic/0x69C53E7b8c41bF436EF5a2D81DB759Dc8bD83b5F/41).
5. To view the metadata for this NFT, copy the IPFS hash provided to you in the terminal after uploadeding your video. You can take this hash and navigate to
https://ipfs.io/ipfs/your-ipfs-hash-here

<img width="578" alt="Screen Shot 2022-03-13 at 5 43 42 PM" src="https://user-images.githubusercontent.com/15346823/158078517-9968932a-7d7f-4107-b25f-341b65479060.png">


### Deploy Your Own ERC721 Contract
If you don't want to use Livepeer's contract to mint your NFT, you can create and deploy your own. For this workshop, we'll be using the [Remix IDE](https://remix.ethereum.org/)

1. Inside the remix broswer IDE, create a new file in the ```contracts``` folder. I'll name this ```myNFT.sol```.
<img width="392" alt="Screen Shot 2022-03-13 at 5 48 12 PM" src="https://user-images.githubusercontent.com/15346823/158078831-9029e187-8aa4-4f6a-a1a7-c96d5f84cd9f.png">
2. In this folder, paste in this code.

```solidity
// contracts/4_ERC721.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

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
If you want a step-by-step on what's happening in this code, read below 👇 If not, skip to step 4. |
3.
- [Openzepplin](https://openzeppelin.com/contracts/) provides solidity libraries which you can use to develop smart contracts easily and securely. We import the contract libraries for our project: ERC721 which is the parent. ```ERC721URIStorage``` gives us the methods necessary to associate an NFT with a URL, in this case an IPFS hash where we'll store the metadata.  ```Counters``` gives us some utility functions to safely assign a unique tokenID to each NFT we mint. 
```
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/utils/Counters.sol";
```

- Define the constructor which takes in the name and symbol for your collection:
```solidity
constructor() ERC721("Video NFT", "VIDEO") {}
```

- Define a Mint event to be emitted upon minting your NFT. We need to emit this event so your web3 client can get access to the tokenID after it has been minted.
```solidity
event Mint(
        address indexed sender,
        address indexed owner,
        string tokenURI,
        uint256 tokenId
    );
```
- Define a function ```mint``` that returns a tokenID. In this function, we make it so only the message signer can be the owner of the NFT. You can edit this for your contract if you want. In this function, we use the Counters utlity to increment the tokenID for each mint and set the tokenID for this NFT to be the current count. Then we call ```_mint```, a function inhereted from the ERC721 contract library we imported which actually associates the NFT with an owner via its ID. We call ```_setTokenURI``` from the ERC721 library which sets the tokenURI. THen, we actually emit the ```Mint``` event we created eaelier. Lastly, we return the TokenID for this newly minted tokebn
```solidity
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
 ```
 
 - We also define a function  ```uri``` which takes in a tokenID and returns the tokenURI.
```solidity
function uri(uint256 tokenId) public view returns (string memory) {
        return tokenURI(tokenId);
    }
```

4. Then head over to the tab with the Ethereum icon. Here you'll compile and deploy your contract.
<img width="1440" alt="Screen Shot 2022-03-13 at 6 26 11 PM" src="https://user-images.githubusercontent.com/15346823/158079884-1440c154-2003-4c8e-90ca-3e745f740a5e.png">
5. Leave the default settings and hit Compile Contract
<img width="318" alt="Screen Shot 2022-03-13 at 6 27 16 PM" src="https://user-images.githubusercontent.com/15346823/158079903-9d20eb2c-7231-4b3b-b24e-48054d5aaf7c.png">
6. On the left hand meny for Remix, hit the last icon, "Deploy and run transactions."
7. Under "Environment," select "Injected Web3." Sign the transaction in Metamask when prompted.
<img width="282" alt="Screen Shot 2022-03-13 at 6 41 12 PM" src="https://user-images.githubusercontent.com/15346823/158080367-e474ee9d-5b34-436d-a341-b8869c850daf.png">
>Be sure that you're on Mumbai Network
<img width="352" alt="Screen Shot 2022-03-13 at 6 32 30 PM" src="https://user-images.githubusercontent.com/15346823/158080133-69e01b2e-e036-415b-99b3-d6471c4e29ac.png">
8. In the "Contract" drop down, select the contract you want to deploy. Only contracts that have already been compiled will show up here, so be sure to complete step 5. Hit Deploy.
<img width="281" alt="Screen Shot 2022-03-13 at 6 43 23 PM" src="https://user-images.githubusercontent.com/15346823/158080428-440583ba-46ec-478b-b1cc-69ed52b93360.png">
9. Sign the transaction in MetaMask
<img width="352" alt="Screen Shot 2022-03-13 at 6 57 46 PM" src="https://user-images.githubusercontent.com/15346823/158080970-6d88fbf3-8fc0-48e8-a98c-4bd56ea10d1f.png">
10. Done! You can get the contract address on the bottom left hand side of the screen. Copy this address and go back to the Mint UI in Livepeer. You can paste this address in the box to mint your NFT using this contract.
<img width="315" alt="Screen Shot 2022-03-13 at 6 58 41 PM" src="https://user-images.githubusercontent.com/15346823/158081004-92499e5a-a13a-4dbc-9329-fa342e364cbb.png">


