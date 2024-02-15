# Contribute to implementing the IBC standards in Solidity
IBC is a well documented and battle-tested protocol with many developers contributing code/standards to improve its developer experience and help other applications build off of their work. The PIT empowers further contributions, there are a few IBC standards Polymer thinks can be used by a wide variety of applications. 

IBC is a complete interop protocol with a well-defined specification, called the the [_**I**nter**C**hain **S**tandards_](https://github.com/cosmos/ibc) or ICS for short. There's a clear separation between the state and transport layers on the one hand (this is what Polymer has contributed to for L2-L2 commmunication) and the application layer.

In Phase 1 of the PIT we focus on the application layer. In Cosmos, where IBC originated, many standards have implementations as Go modules. The challenge you're presented with is to implement some of those for the EVM and Solidity.

Similar implementations already exist in EVM and we leave it to your best judgement to establish a fair analysis between them, propose and build a new standard that will help further the cosmosethereum.

Dive into the technicals between various standards which are available in IBC and their counter parts in Ethereum:

- Perform a technical overview of the designs.
- Compare the best features from each of the standards.
- Prepare a functioning solidity implementation and with an intuitive CLI for easier use.

Note: Research paper/proposal also counts as documentation in the rubric.

| Name  | Standard  | References from EVM |
| --- | --- | --- |
| Fungible token transfer | [ICS20](https://github.com/cosmos/ibc/tree/main/spec/app/ics-020-fungible-token-transfer) | [xERC20](https://github.com/rhlsthrm/awesome-xerc20?tab=readme-ov-file), [OFT](https://docs.layerzero.network/contracts/oft) |
| Interchain accounts  | [ICS27](https://github.com/cosmos/ibc/tree/main/spec/app/ics-027-interchain-accounts) | [Create2](https://docs.alchemy.com/docs/create2-an-alternative-to-deriving-contract-addresses)  |
| Interchain Queries  | [ICS31](https://github.com/cosmos/ibc/tree/main/spec/app/ics-031-crosschain-queries) |  |

