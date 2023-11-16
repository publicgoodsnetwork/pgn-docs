# Safe (multisig)

[Safe](https://safe.global/) is a robust and flexible smart contract platform, commonly utilized for asset management on Ethereum. It's a go-to choice for users requiring enhanced security and modular control over their digital assets. Traditionally, Safe contracts are employed for managing funds in a decentralized and secure manner. Users benefit from features like multi-signature (multisig) transactions, which require multiple confirmations before execution, ensuring greater oversight and reduced risk of unauthorized access.

While PGN network integrates with Safe contracts, it's important to note that we're currently in the process of developing a dedicated user interface. Please help us upvote this proposal to help build out a Safe UI:
https://forum.safe.global/t/deploy-safe-core-contracts-to-the-pgn-network/4352

Until then, interactions with Safe contracts will require a more hands-on approach, typically involving direct contract interactions. Our docs aims to guide you through this process, ensuring a smooth and secure experience with Safe on the PGN network.

## Deploying a Safe 

Until we have a working Safe UI, you can interact with the Safe contracts themselves using a couple of different strategies:

* Interact with the contracts themselves using the explorer
* Use the Safe SDK

### Interact with the smart contracts

Let's explore the steps you'd need to take to interact with the smart contracts using the PGN explorer:

1. Copy any [Safe contract address](#safe-contracts)
2. Open the [Explorer](https://explorer.publicgoods.network), search, and navigate to the selected contract addresses
3. Open the "Contract" tab, and navigate to "Write contract"
4. Connect your wallet
5. Expand the contract dropdown under the "Contract information" section
6. Enter your transaction and value into the input boxes and submit with the "Write" button

### Use the Safe SDK

We'll provide some guidance here, but it is important or you to read through the [Safe Core SDK](https://docs.safe.global/safe-core-aa-sdk/safe-core-sdk) docs to understand this method. 

Here are a few steps that you will need to follow:
1. [Create an `ethAdapter`](https://docs.safe.global/safe-core-aa-sdk/api-kit)
2. [Create a Safe Factory and deploy the Safe](https://docs.safe.global/safe-core-aa-sdk/protocol-kit/reference)

**Note:** PGN is a layer 2 so no need to include the `isL1SafeMasterCopy` flag
**Note:** Safe contracts are deployed to the PGN network in version v1.3.0

3. Use the different [components](https://docs.safe.global/safe-core-aa-sdk/protocol-kit/reference#safe-reference) available via the Protocol Kit to engage with the deployed Safe. 

## Safe contracts

| Contract Name            | Mainnet and testnet address                |
| ------------------------ | ------------------------------------------ |
| multiSend                | 0xA238CBeb142c10Ef7Ad8442C6D1f9E89e07e7761 |
| safeMasterCopy           | 0xd9Db270c1B5E3Bd161E8c8503c55cEABeE709552 |
| safeProxyFactory         | 0xa6B71E26C5e0845f74c812102Ca7114b6a896AB2 |
| multiSendCallOnly        | 0x40A2aCCbd92BCA938b02010E17A5b8929b49130D |
| fallbackHandler          | 0x1AC114C2099aFAf5261731655Dc6c306bFcd4Dbd |
| createCall               | 0x7cbB62EaA69F79e6873cD1ecB2392971036cFAa4 |
| signMessageLib           | 0xA65387F16B013cf2Af4605Ad8aA5ec25a2cbA3a2 |
