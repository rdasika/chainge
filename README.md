# Chainge: the chain of change

<img src="logo.jpeg" width=200 align="right"/>

Chainge is a web3 dApp that empowers users to create change around themselves and to encourage their communities to do the same. Users mint an NFT containing an "act of kindness" which they must do and then pass it on by transferring the token to their friends. If you mint or receive a token, it's now your responsibility to not break the chain!

This project was conceptualized and built during the [EthShanghai 2022](https://hackathon.ethshanghai.org/) hackathon. Follow me on [Twitter](https://twitter.com/rohandasika) or [LinkedIn](https://www.linkedin.com/in/rohandasika/)!

> App front-end deployed on Arweave [here](https://vfkzxr5zcu3cgjb7dfbeukbkuohdh56esgledqjzkau4kwi4sm.arweave.net/qVWbx7kVNiMkPxlCSigqo44z98SRl_kHBOVApxVkck0/)

> Contract deployed on Polygon Mumbai testnet [here](https://mumbai.polygonscan.com/address/0x9f15E1cF4af56117f8d83977C8529F3C5fb8266F)

> [Video Demo](https://www.loom.com/share/aaa03e31fbe944a28eaf6c3a2160e8ef)

# Contents

- [Technologies used](#tech-used)
- [How it works](#how-it-works)
- [Bounties](#bounties)
- [What's next?](#whats-next)
- [Setup instructions](#setup)

## Technologies used

- Network: [Polygon Mumbai testnet](https://polygon.technology/)
- Smart contract dev environment: [Truffle](https://trufflesuite.com/)
- RPC endpoints: [Infura](https://infura.io/), [Matic Vigil](https://maticvigil.com/)
- Front-end hosting & NFT image storage: [Arweave](https://www.arweave.org/)
- User profiles & Dynamic NFT metadata: [Ceramic](https://ceramic.network/)
- Social connections: [CyberConnect](https://cyberconnect.me/)
- Front-end framework: [Create React App](https://create-react-app.dev/)
- UI library: [Material UI React](https://mui.com/material-ui/getting-started/installation/)

## How it works

### NFT metadata

Apart from the image, each NFT contains an act of kindness and a counter of how many times the act has been done (by all owners of the NFT). The image is stored on Arweave and the dynamic metadata on Ceramic.

### User auth

User login and auth is handled behind-the-scenes by Ceramic. Once a user logs in, if they don't have a [3ID DID](https://developers.ceramic.network/docs/advanced/standards/accounts/3id-did/), it will be generated by the Ceramic Self.ID SDK. Once they have a 3ID DID, a [CAIP-10 Link](https://developers.ceramic.network/docs/advanced/standards/stream-programs/caip10-link/) is manually established to enable bi-directional linking between a user's ETH account and DID. This enables us to search for any Ceramic streams that are linked to the user's DID, just given their ETH address. The user's profile information (name, bio, etc.) is created and stored on Ceramic on [Self.ID](https://clay.self.id/) on the Clay testnet.

### NFT minting

Once a user logs in, in the _Home_ tab, they see a list of all the NFTs they own and a button to mint a new one. The `mint` button will generate a new NFT with a random act of kindness and a counter of 0.

### NFT verification

In the _Home_ tab, users also see a form to submit an NFT/action for verification and a list of all the NFTs that have already been verified. A Ceramic datamodel is used to keep track of which NFTs have been verified by the current user. Users can submit the NFT tokenID and a date in the `yyyy-mm-dd` format for verification. Once the NFT has been verified, the counter in the NFT metadat will automatically increment. It will show up immediately in the application, but may require a manual metadata refresh on OpenSea.

Since data stored on Ceramic is inherently public (and we have a CAIP-10 Link associating an ETH account to a DID) we can view (on-demand) which NFTs have been verified by _any_ user, given either a DID or ETH account.

### NFT transfer

Once an action has been verified, a button by the NFT entry appears which links to the NFT's OpenSea page, enabling the user to transfer the NFT to their friends.

### 'Friends' tab

Using the CyberConnect SDK, users can search for friends by their ETH addresses or ENS name. Once a user finds a friend, they can _follow_ them enabling them to see their accounts directly in the app. This enables users to see their friends' NFTs and actions, and to transfer them to their friends.

### 'Search' tab

Any user address can be entered into the search bar and the app will show the NFTs owned by that user and the NFTs verified by that user. This is useful for users who are not friends but want to see what they've done. For example, if a user is not following a friend, they can search for their NFTs and see how many times they've verified them.

Viewing the NFTs a user currently owns is possible by just using the smart contract. Viewing the NFTs a user has verified is possible by using the CAIP-10 Link to find the user's 3ID DID and then looking up the verified NFTs on the user's account.

## Bounties

### Bounty 1: [BUIDL for Social Good with Infura & Truffle using a Layer 2 solution](https://gitcoin.co/issue/28876)

The project's entire development was done through a combination of using Truffle as a dev environment with Infura endpoints, amongst others. This app is an example of using an L2 solution (Polygon) for social good, integrating a social network and verifiabiilty amongst your friends for performing acts of kindness in your communities.

I started by using a combination of the [Truffle React Box](https://trufflesuite.com/boxes/react/) and [Truffle Polygon Box](https://trufflesuite.com/boxes/polygon/). However, I ran into a few configuration issues with _actually_ deploying using a combination of these tools, as detailed in this [Discord thread](https://discord.com/channels/969606926868570162/973945098746335252/980448997892296704).

Ultimately, I was able to get the following combinations working...

- Truffle + Infura Rinkeby `wss://` endpoint ([Deployed contract](https://rinkeby.etherscan.io/address/0xdea5a3593d277c6fc317B742280927bb76f3413e))
- Truffle + Matic Vigil `http://` and `wss://` endpoints ([Deployed contract](https://mumbai.polygonscan.com/address/0x17FAd95EB36EEC17c1E6DDF8e1CCD3C444B99071))
- Hardhat + Infura Mumbai `http://` endpoint ([Deployed contract](https://mumbai.polygonscan.com/address/0x8d64155F24674067ea09cbAB74c4ff99E220e3D3))

In the end, the app was deployed using Truffle and the Matic Vigil `wss://` endpoint ([Deployed contract](https://mumbai.polygonscan.com/address/0x9f15E1cF4af56117f8d83977C8529F3C5fb8266F)). There lots of online [discussion](https://github.com/trufflesuite/truffle/issues/3356) in the Truffle forums about using the Infura `wss://` endpoints for best results, but unfortunately, Infura _only_ has a `http://` endpoint for the Polygon Mumbai testnet.

### Bounty 2: [Build the best social dApps with CyberConnect](https://gitcoin.co/issue/28881)

Building a social dApp like this without CyberConnect isn't possible. The purpose is to find friends to share tokens with and view each other's progress, and I used CyberConnect's SDK to incorporate the "Follow" Button and queries for identity lists. With this, users can follow others, clearly see who follows them, who they follow, and use those addresses to search for what acts of kindness they have completed.

### Bounty 3: [Build a dApp using Arweave](https://gitcoin.co/issue/28889)

The [image](https://ugnie2vqerzywroilo3dk4lerfv2xxidwfxxy2w5koozrtkwhq.arweave.net/oZqCarAkc4tFyFu2NXFki_Wur3QOxb3xq3VOdmM1WPE) for each NFT is stored on Arweave, using [ArDrive](https://ardrive.io/). Using `arkb` and `bundlr`, the frontend of the [app is deployed to the permaweb](https://vfkzxr5zcu3cgjb7dfbeukbkuohdh56esgledqjzkau4kwi4sm.arweave.net/qVWbx7kVNiMkPxlCSigqo44z98SRl_kHBOVApxVkck0/) 🐘 as well. I would have used Arweave to host _all_ the NFT metadata, but I chose to go with Ceramic instead to support dynamic NFT metadata since each time an 'act of kindness' is completed, a counter in the metadata must be incremented.

### Bounty 4: [Polygon Track 3 (Open Track)](https://gitcoin.co/issue/28870)

Chainge is built and tested fully on the Polygon Mumbai test network. Since this is an application in which we want the NFTs to be frequently transferred between people, it must be deployed on a fast network with low fees.

## What's next?

### Stability and UX

- Currently the CAIP-10 Link is manually triggered _after_ the user logs in, with them having to sign another message to establish the link. This is a temporary solution until the app is updated to allow automatic linking.
- Ceramic just transitioned to the Ceramic v2, which uses the new [Sign-In With Ethereum](https://login.xyz/) standard to create a DID:PKH, instead of a 3ID DID. This eases UX by limiting the number of signatures needed to create a DID. I continued using Ceramic v1 that came with the SDK, but in the future, should transition to v2.
- When the app first opens, it takes a few seconds for the connections to be made and for the Ceramic streams to be loaded. So typically, a user has to wait around 5s before they can click `connect` and log in.

### Features

- Time tracking on the how long it has taken for the user to complete the act of kindness. This way, if the user hasn't completed the act of kindness for a while, others can easily see it.
- Along with time tracking, if the user hasn't completed an action in a specified period (say, 2 weeks) the NFT can be burned, and the user can be issued a [Soulbound Token](https://vitalik.ca/general/2022/01/26/soulbound.html) as an NFT that is publicly visible.
- Today, the "verification" is just blindly updating the user's Ceramic datamodel. But in the future, we can add a "verification" process that requires the user to verify their NFTs with proof of activity (picture, video, etc).
- The update to the user's Ceramic datamodel can also follow the [W3C Verifiable Credential](https://www.w3.org/TR/vc-data-model/) standard.

## Setup instructions

> This is mainly a reminder for myself on how to build an app with all these components ;)

Clone the repo:

```
git clone <repo link>
```

Download all dependencies:

```
cd chainge
// From the root directory (for Truffle and smart contracts)
npm install

// From the client directory (for React app)
cd client
npm install
```

From the client directory, run `npm start` if running locally. If trying to deploy for production, run `npm run build`.

### Truffle

The smart contracts have already been compiled and deployed, so you won't need to recompile or add them to the project. If you'd like to edit the smart contracts, you can do so in the `contracts` directory. Please see [Truffle Guides](https://trufflesuite.com/guides/) on further instructions.

```
// Compile and deploy the contracts.
truffle migrate --network=mumbai
```

This will output a 'contract address' if successful. Upate `const nftContractAddress` in `client/src/utils/constants.js` with the new address.

### Ceramic

The Ceramic streams for this project have already been created and deployed on the Clay test network. If you'd like to edit these, please see the [Ceramic documentation](https://developers.ceramic.network/tools/glaze/development/) on further instructions.

```
// After editing schemas, deploy to Clay testnet
node createModels.mjs
```

This will create a new `dataModels.json` file. Take those definition/schema IDs and make sure to update the aliases in `client/src/utils/constants.js`.

### Arweave

After making any changes, if you'd like to deploy the front-end onto Arweave, please see the [arkb user guide](https://docs.arweave.org/developers/tools/textury-arkb) or these [permanotes](https://permanotes.app/#/notes/HTmJq7ib_dEmLW9hI9q2GUwoChhprlG88XMfVn_Zqaw). If you've never used arkb before, the user guide is a helpful resource as well as the [Arweave Discord](https://discord.gg/HGC7jxV4FH). Ensure to use `Hashrouter` for react-router and add `"homepage":"."` to the `package.json` file.

```
// From the /client directory
npm run buld
arkb deploy ./build --use-bundler https://node2.bundlr.network
```
