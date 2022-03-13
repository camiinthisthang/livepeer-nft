# livepeer-nft

## Purpose

This guide will walk users through uploading and minting a video as an NFT using [Livepeer](https://livepeer.com/), a decentralized video transcoding platform that has a [streaming](https://livepeer.com/docs/api-reference/stream/overview) and [video on demand API](https://livepeer.com/docs/api-reference/vod/upload). 

### Prerequisites: 
- Have node installed on your machine.
- Have a .mp4 video on your machine to upload.
- Have a metamask wallet and Metamask broswer extension.
- Be connected to the Polygon network.
- Have MATIC or test MATIC in your Metamask wallet.

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

1. After navigating to the URL provided, you can leave the contract field blank and default to Livepeer's contract. Hit "Mint NFT"
2. You'll have to confirm the transaction and pay for the gas in Metamask. 
3. Done 🎉
<img width="494" alt="Screen Shot 2022-03-13 at 5 20 16 PM" src="https://user-images.githubusercontent.com/15346823/158077728-a5cd3e08-990d-423f-a4f3-23ccabf8a90b.png">
4. Hit the link provided to view your video NFT on Opensea. You can view the one in this demo [here.](https://opensea.io/assets/matic/0x69C53E7b8c41bF436EF5a2D81DB759Dc8bD83b5F/41).

