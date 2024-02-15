# Create a cross-chain dApp using using IBC 
One of the great things of IBC is its ability to allow applications to have their own independent communication channels that no other application can use. This enables vastly reduced risk between the various instances of an application on different chains. 

NOTE: Solutions that are effective for broader community engagement will be scored highly.

## Building Suggestions

### 1.  Build a multi-chain token contract factory/deployer

**User story:** 
- As a protocol I want to launch tokens on multiple chains so that my users can bridge them between chains easily.
  
**Objectives:**
- Build an interface to deploy a token standard on multiple chains with the ability to define a token ticker and token supply.
  
**Implementation tips:**
-   Users can use the quick start guide for universal channel to deploy contract on different chains in one source transaction.
-   Additionally, an added advantage of IBC is that it allows protocols to have sole ownership of their channel, so a protocol can choose to perform a channel handshake process to deploy a custom channel, and then deploy token contract that only their protocol can call.
-   You can choose to build a script to perform the channel handshake process for token deploy in-order to make it a seamless process to deploy tokens.
-   Feel free to generalize and extend the use-case to multichain deployment of *any* contract.
  
**References:** [xERC20](https://github.com/rhlsthrm/awesome-xerc20?tab=readme-ov-file), [OFT](https://docs.layerzero.network/contracts/oft), [IBC Channels](https://tutorials.cosmos.network/academy/3-ibc/3-channels.html), [IBC Channels (bis)](https://docs.polymerlabs.org/docs/learn/concepts/ibc/#channels-ics-4)

---
### 2.  Build a bridging application

**User story:** 
- As a user I want to bridge tokens between chains.
  
**Objectives:**
- Build a bridging application that lets users bridge tokens between chains.
  
**Implementation tips:**
-   You can choose to deploy a lock and mint bridge that mints wrapped token representations under your project name. 
-   You can choose to deploy a burn and mint for tokens you first deploy on different chains and whitelist for your bridge. 
-   You can further your bridge development by building an embedded bridge and swap action in order to perform cross-chain swaps. 
-   Creative use of Acknowledgements
    - In IBC when a packet is sent, it goes through three distinct phases of the packet lifecycle: (1) the initiating *sendPacket* transaction from the source chain, (2) the corresponding *recvPacket* transaction on the destination chain that executes a specific action (referred to as the 'onRecvPacket' callback action), and (3) finally a  acknowledgment or timeout transaction. The acknowledgment serves as a notification to the originating chain about the receipt of the packet on the destination. 
    **NOTE**: timeouts are currently not supported on testnet
    - If the action involves the minting of a specific token on the destination chain, there are ways to improve UX: the user tokens on source are unlocked or not burned in case of a failed onRecvPacket callback action of minting new tokens.
  
**References:** [Lock/burn and mint bridge](https://ethereum.org/developers/docs/bridges) , [xERC20](https://github.com/rhlsthrm/awesome-xerc20?tab=readme-ov-file), [OFT](https://docs.layerzero.network/contracts/oft)

---
### 3.  Building L2 to L2 native token bridge with mapping 

**User story:** 
- As a rollup I want to help users bridge existing tokens from other chains to improve liquidity and prevent unnecessary wrapped tokens on my chain.
  
**Objectives:**
- Primary objective: Build a lock and mint bridge with a feature to whitelist L2 and give them minting rights of the token contracts.
- Secondary objective: Build an L2 to L2 token mapping between an existing token on a L2 and deploy its corresponding new token contract on a new L2. 
  
**Implementation tips:**
-   Usually token mapping for a rollup is done between L1 and L2, but given the landscape of rising rollups and liquidity fragmentation it has become important to allow free flow of assets between L2s.
-   Polymer is best fit for this task as it uses the native L1<>L2 bridge for settlement when bridging between rollups, thus has the same security as that of the native bridge.
-   You need to build a system that deploys a standardized vault for locking tokens on the source, and either mints tokens with the same token ticker on the destination, or mints from an existing contract you provide the rights.
-   Note that currently our testnet only supports 2 chains, making this a relatively trivial task. In future a rollup can choose to allow token mapping from another rollup. In that case, you must make sure they mint from the same token contract and not create another representation. 
  
**References:** [Mapping tokens from L1 to L2](https://docs.arbitrum.io/for-devs/concepts/token-bridge/token-bridge-erc20)

---
### 4.  Building a CCTP adaptor for Polymer

**User story:** 
- As a user I want to bridge my USDC and swap it on the destination chains or deposit it into a vault or send to a different address.
- As an application I want to enable my users to deposit into my USDC vaults in a single-click from any other rollup without having to bridge separately. 
  
**Objectives:**
- Build an interface that application developers can use to enable one-click USDC cross-chain actions. Wherein USDC is bridged via CCTP and an action is performed via Polymer once funds reach the destination. 
  
**Implementation tips:**
-   CCTP is the native bridge for transferring USDC between chains, many times users have to bridge their tokens and then perform an action on another chain.
-   You can reduce it UX overhead by designing an interface that developers can to embed in their application and allow for easy cross-chain deposits via CCTP.
-   You can choose to deploy a contract on Base that the user wishes to deposit funds in, and bridge their test USDC from Optimism.
-   Kudos if you can build a deposit UI wherein users can select which chain they want to bridge USDC from and then deposit in the contract/vault directly.
-   Fun fact: On mainnet can use something like Socket or LiFi to enable wider cross-chain swaps. 
  
**References:** [CCTP](https://developers.circle.com/stablecoins/docs/cctp-getting-started) and [testnet contracts](https://developers.circle.com/stablecoins/docs/evm-smart-contracts#testnet-contract-addresses)

---
Experimental ideas: Having fun with Acknowledgements by building complex onRecvPacket and onAcknowledgment behaviors.
   
1. Implement ERC404 and build a cross-chain NFT minting game
   1. 10,000 ERC-20 tokens & same number of associated “Replicant” NFTs.
   2. If someone buys one full token on a DEX on Optimism testnet, one replicant NFT will be minted on Base.
   3. If the token is sold, the connected NFT must be burned.
