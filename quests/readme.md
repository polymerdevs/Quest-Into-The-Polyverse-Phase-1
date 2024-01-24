# Phase 1 Quest List: 


### 1. New dApps

<details>
<summary>Challenge 1 : **Build an Omnichain token standard for Polymer**</summary>
    
    
    Difficulty (0-5): 3
    
    Expected User Persona: Dev1, Dev4 
    
    Why: Allow Polymer partner to easily deploy tokens that exist on multiple chains in one go and transferrable across them without asset wrapping. 
    
    How: Build an interface/module to deploy token on multiple chains and implement a burn and mint token application/bridge between the token contracts. Then generalise the implementation for any xERC20 token.  
    
    Mint n Burn bridge = Escrow user tokens, send a mint message via Polymer hub, upon minting which is verified with a successful ack, burn the tokens in escrow. 
    
    Challenge 1A: Deploy it with channel specific minting/burning rights i.e only the protocol that owns the channel can initiate. 
    
    Challenge 1B: Deploy with universal channel with middleware contract acting as the trusted contract/party. 
    
    References: [LZ OFT](https://docs.layerzero.network/contracts/oft), [xERC20](https://github.com/rhlsthrm/awesome-xerc20?tab=readme-ov-file)
</details>
 <details>
<summary>Challenge 2: **Build a CCTP adaptor for Polymer**</summary>
    
    
    Difficulty (0-5): 4
    
    Expected User Persona: Devs
    
    Why: Polymer is a messaging bridge, many use cases need the movement of liquidity. Given 75% of the bridging volume is in USDC, we need a way to migrate liquidity and embed a destination action like LP, stake or swap in one transaction. 
    
    How: Deploy a contract which received the USDC from user, sends it to CCTP and parallelly sends the destination action (as a message) via Polymer Hub. Simple actions like deposit in a vault, swap or transfer to a new address work for now. Bonus points doing it in a non-custodial way. 
  </details>   
  <details>   
<summary>Challenge 3: **Build method to send small amounts of Native Gas** </summary>
    
    
    Difficulty (0-5): 2
    
    Expected User Persona: Dev1, Dev2 (primary) and Relayers (secondary)  
    
    Why: A cross-chain call might have to use different Gas tokens on the source and destination. It is better to accept the single gas token from the user on source and have it bridge into destination gas token for end execution. Its a primitive can be used by protocols and relayers. 
    
    How: Build a Get_GAS Method for Polymer that takes the source chain token and swaps a portion of it into destination gas token - with conditions to check. Given both OP and Base use ETH as the gas token, for testing the method you can take small amount of ETH on the receiving side, swap it into USDC on the destination. Hint: Catalyst can be used for cross-chain swap and pricing oracle. 
  </details>  
  <details>   
<summary>Challenge 4: Channel-isolation use cases</summary>
  </details>   

### 2. **ICS implementations**

Focus: Reword existing ICS implementations from Cosmos environment to Solidity. 

<details>
<summary>Challenge 1 : **ICS-20 (as wrapped asset bridge - lock n mint/burn)** </summary>       
    Difficulty (0-5): 4    
    Expected User Persona: Dev2, Dev5    
    Note: Dev3 persona can be targeted for auditing these standards in solidity and potential ways to optimise them.  
    Why: Allow existing tokens to exist on multiple chains in one go and transferrable across them.   
    How: Implement a lock n mint bridge with specs defined in **[ics-020-fungible-token-transfer](https://github.com/cosmos/ibc/tree/main/spec/app/ics-020-fungible-token-transfer).** For test case, deploy a token on Optimism and bridge its wrapped version over to Base. 
</details>

<details>
<summary>Challenge 2: **Interchain Accounts** </summary>
    Difficulty (0-5): 4
    Expected User Persona: Dev1,  Dev5 
    Note: Dev3 persona can be targeted for auditing these standards in solidity and potential ways to optimise them.     
    Why: Multiple cross-chain account management    
    How: Build a ICA implementation for Base and Optimism along with an interface to define owner(s), and simplify initiation of account from single source. For test case, use a multisig to deploy ICA on both source and destination chains.     
    References: [ics-027-interchain-accounts](https://github.com/cosmos/ibc/tree/main/spec/app/ics-027-interchain-accounts), [create2](https://docs.alchemy.com/docs/create2-an-alternative-to-deriving-contract-addresses),  
 </details>   
 
 <details>
<summary>Challenge 3: **Interchain queries**</summary>
    
    
    Difficulty (0-5): 4    
    Expected User Persona: Devs    
    Note: Dev3 persona can be targeted for auditing these standards in solidity and potential ways to optimise them.     
    Why: For dApps to on read data from other chains, e.g., a particular application on a chain may need to know the current price of the token of a second chain.    
    How: Implement [ics-031-crosschain-queries](https://github.com/cosmos/ibc/tree/main/spec/app/ics-031-crosschain-queries) in solidity and build an interface to query data. For test case fetch the on-chain price of a meme coin deployed on Base from Optimism. 
</details>

<details> <summary> Challenge 4:</summary>
</details>

### 3. Developer tooling

Focus: Build essential developer tooling to monitor and build with Polymer network. 
<details>
<summary>Challenge 1: **Automation scripts for Local Testnet deployment** </summary>        
    Difficulty (0-5): 4    
    Expected User Persona: Dev3, Relayers (secondary)     
    Why: Local testnet environments are a fairly cheap way to allow developer understand the tech stack and build application, run initial tests before deploying on testnet or mainnet.     
    How: 
</details>

<details>   
<summary> Challenge 2: **Method for Gas estimation and documentation support** </summary>        
    Difficulty (0-5): 3    
    Expected User Persona: Relayers    
    Why: To estimate how much gas a message will cost to send and receive `ACK` to avoid failure cases.     
    How: Build a `quote` function that can return an estimated gas value to input for a successful delivery, execution and also send back an ACK. Furthermore, need to keep in mind that execution gas can be a different token - account for gas token volatility. 
 </details>
 
<details>
<summary>Challenge 3: **Improve block explorer with channel and connection visualisers** </summary>    
    Difficulty (0-5): 4    
    Expected User Persona: Dev3, Relayers     
    Why: Ethereum developer are used to working with chains and chainID, and are not used to the IBC tech stack. So in order to help a wider audience understand the explorer (channel & connections) we invite community to contribute and suggest UI/UX improvements.     
    How: Suggest wireframes or working webflow wherein Protocol is be able to view all the connected channels corresponding to their portID or wallet - and initiate transactions from it. Furthermore, even open a new channel if required.     
    References: https://mapofzones.com/
</details>

<details>
<summary>Challenge 4: **Connection Helper** </summary>       
    Difficulty (0-5): 2    
    Expected User Persona: Relayers, Dev3    
    Why: For protocols to verify a connection is successfully established between the channel, port end-points.     
    How: Build a `getDestinationPortConfig()` (get proper name from engineering) method, a view function that returns the unique Port, Channel ID tuple for a given chain. Build it so that it can be generalised in the future to not only check but also fetch the destination tuple with a chainID. 
 </details>
 
<details>
<summary> Challenge 5: **GMP interface for universal channel** (can combine with 2) </summary>    
    Difficulty (0-5): 3  
    Expected User Persona: Dev3, Dev5   
    Why: For a smart contract to connect to Polymer end-point contracts, send outbound messages and calculating message delivery fee.     
    How:
  </details>
  
  <details>
<summary> Challenge 6: **Build a tx batcher** (is this even relevant?) </summary>
  </details>details>
