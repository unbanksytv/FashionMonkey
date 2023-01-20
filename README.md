# Hyper Apes

Hyper Apes are NFTs generated by the 
[DALL-E](https://openai.com/blog/dall-e-now-available-in-beta/) Artificial Intelligence (AI). Hyper Apes can swing between blockchains. Images and metadata are stored on [IPFS/Filecoin](https://ipfs.tech/).

Hyper Ape holders earn one 🍌BANANA every second, streamed in real-time using the [Superfluid](https://www.superfluid.finance/) protocol. BANANAs have utility as a _mint pass_: 1,000,000 BANANA can be used to mint special edition Hyper Apes.

![Hyper Ape](https://hyperapes.club/images/ape.png)

# How it was Made

Artwork for Hyper Apes is generated at mint time by AI (DALL-E by OpenAI). Choose a color and an accessory and an Ape will be generated with a background color representing the "birth chain" of the Ape. Once the image has been generated, it is stored on IPFS (via the nft.storage service) and NFT metadata JSON is also saved to IPFS.

The Hyper Apes smart contracts adhere to the ERC721 standard. Hyper Apes enables minting NFTs from the collection ony any of the supported chains and the ability to send NFTs from one chain to another. Each chain has a defined range of IDs that can be minted on that chain -- but after minting, the NFTs can be sent to any of the other supported chains.

Hyper Apes features:

- *Mint with Metadata URI*. To add support for decentralized storage of metadata on IPFS, the `mint()` function for Hyper Apes assigns the IPFS metadata hash to the newly minted token. Open Zeppelin's `ERC721URIStorage` extension was used towards this end.

- *MintAndSend() Function*. Hyper Apes includes a second mint function that enables minting a new NFT and immediately sending it to another chain, in the same transaction. Suppose you want one of the cool Moonbeam Hyper Apes with the red background, but you want it in your Etheruem wallet. The `mintAndSend()` function enables this.

- *Hyperlane Send and Receive Functions*. A key challenge was ensuring the contract on the destination chain received the `tokenURI` (IPFS metadata hash), such that NFT marketplaces and explorers can access the Hyper Ape image and other metadata. I modified the `TokenRouter.sol` example to create `ERC721Router.sol`, a Router specifically for ERC721 tokens, with some flexibility to customize the message payload to be sent to remote chains.

- *Contracts Deployed are to the same address on each chain*.  

- *Streaming Mint Pass Tokens*. The `Streamer.sol` deploys an ERC20-compatible [Native Super Token](https://docs.superfluid.finance/superfluid/developers/super-tokens/super-tokens/types-of-super-tokens/native-asset-super-tokens) that can be streamed using the Superfluid prootcol. Hyper Apes mints 420 Trillion BANANA tokens to the Streamer contract. Unlike the Hyper Apes NFT contracts, _the Streamer contract only lives on Polygon Mumbai_. When you mint a Hyper Ape on _any_ of the supported chains, BANANA tokens will start streaming to you in real-time on Polygon Mumbai, at a rate of 1 BANANA per second. If you sell or transfer your Hyper Ape, the stream of BANANAS stops and the new owner starts to receive BANANAS. Message are dispatched across the Hyperlane network to the Streamer on Mumbai, so it doesn't matter where you mint, send, or transfer your Hyper Apes, the streams of BANANAS on Mumbai get updated accordingly.

- *Using Mint Passes*. On the streamer chain only, you can use your mint pass (BANANA) tokens with the `mintWithPass()` function to mint special edition Hyper Apes. You need spend 1,000,000 BANANAS to mint with this function in the current Hyper Apes deployment. This _Streaming Mint Pass_ concept could be used for accruding mit passes for secondary or even partner NFT collections. When used to mint *limited* edition NFTs, this encourages the minting/collecting of multiple NFTs in the collection, in order to accelerate your *flowRate* of tokens per second, and might also lead to token liquidity, if there is demand to acquire the tokens.

- *Cross Chain Registry*. As a bonus, the Streamer contract provides `ownerOf()` and `balanceOf()` functions that work like their ERC721 equivalents, but the responses _include the NFTs from all supported chains_. It does not matter which chain `tokenId` 420 is currently on, the Streamer contract will tell you the owner.

# Mint a Hyper Ape Today
![Minted!](https://hyperapes.club/images/hyper-apes-minted.png)
Hyper Apes is currently deployed to four testnets. You can mint on any chain and _jump_ your Ape to another, if desired:

- [Ethereum Goerli](https://goerli.etherscan.io/address/0x4674312E6c202662F78E72dF039e16f3AAB2ec3a) (minting #1-1000) [OpenSea](https://testnets.opensea.io/collection/hyper-apes-goerli)
- [Polygon Mumbai](https://mumbai.polygonscan.com/address/0x4674312E6c202662F78E72dF039e16f3AAB2ec3a) (minting #1001-2000) [OpenSea](https://testnets.opensea.io/collection/hyper-apes-mumbai)
- [Arbitrum Goerli](https://goerli.arbiscan.io/address/0x4674312E6c202662F78E72dF039e16f3AAB2ec3a) (minting #2001-3000) [OpenSea](https://testnets.opensea.io/collection/hyper-apes-arbitrum-goerli)
- [Moonbase Alpha](https://moonbase.moonscan.io/address/0x4674312E6c202662F78E72dF039e16f3AAB2ec3a) (minting #3001-4000)

The contract is deployed to the same address on each of the above chains: `0x4674312E6c202662F78E72dF039e16f3AAB2ec3a`.

The dapp can be found at https://hyperapes.club or ipfs://QmdqUZqeLZNyBi7Lwu2wJZx3VDspTXHEqim3Exc5NTDxUV

# Next Steps
- find a co-founder to do Community Management
- contract refinements
- remote minting function? (send mint txn on Chain A, to mint on chain B, and send NFT back to chain A)
- pricing of mainnet mint?
- choose chains for mainnet launch
- deploy to mainnets