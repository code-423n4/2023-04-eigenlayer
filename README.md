# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos: 
- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted (shared with you after the contest) 

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest report is published and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Repo setup

## ⭐️ Sponsor: Add code to this repo

- [ ] Create a PR to this repo with the below changes:
- [ ] Provide a self-contained repository with working commands that will build (at least) all in-scope contracts, and commands that will run tests producing gas reports for the relevant contracts.
- [ ] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 24 hours prior to contest start time.**
- [ ] Be prepared for a 🚨code freeze🚨 for the duration of the contest — important because it establishes a level playing field. We want to ensure everyone's looking at the same code, no matter when they look during the contest. (Note: this includes your own repo, since a PR can leak alpha to our wardens!)


---

## ⭐️ Sponsor: Edit this README

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2022-08-foundation#readme))
  - [ ] When linking, please provide all links as full absolute links versus relative links
  - [ ] All information should be provided in markdown format (HTML does not render on Code4rena.com)
- [ ] Under the "Scope" heading, provide the name of each contract and:
  - [ ] source lines of code (excluding blank lines and comments) in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Optional / nice to have: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [ ] See also: [this checklist in Notion](https://code4rena.notion.site/Key-info-for-Code4rena-sponsors-f60764c4c4574bbf8e7a6dbd72cc49b4#0cafa01e6201462e9f78677a39e09746)
- [ ] Delete this checklist and all text above the line below when you're ready.

---

# EigenLayer contest details
- Total Prize Pool: $90,500 USDC 
  - HM awards: $63,750 USDC 
  - QA report awards: $7,500 USDC 
  - Gas report awards: $3,750 USDC 
  - Bot race awards: $7,500 USDC
  - Judge awards: $9,000 USDC 
  - Lookout awards: $6,000 USDC 
  - Scout awards: $500 USDC 
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code4rena.com/contests/2023-04-eigenlayer-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts April 27, 2023 20:00 UTC 
- Ends May 04, 2023 20:00 UTC

## Automated Findings / Publicly Known Issues

Automated findings output for the contest can be found [here](add link to report) within an hour of contest opening.

*Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards.*

[ ⭐️ SPONSORS ADD INFO HERE ]

# Overview

<a name="introduction"/></a>
# EigenLayer
EigenLayer (formerly 'EigenLayr') is a set of smart contracts deployed on Ethereum that enable restaking of assets to secure new services.
At present, this repository contains *both* the contracts for EigenLayer *and* a set of general "middleware" contracts, designed to be reuseable across different applications built on top of EigenLayer.

Note that the interactions between middleware and EigenLayer are not yet "set in stone", and may change somewhat prior to the platform being fully live on mainnet; in particular, payment architecture is likely to evolve. As such, the "middleware" contracts should not be treated as definitive, but merely as a helpful reference, at least until the architecture is more settled.

The EigenLayer whitepaper is available [on our website](https://docs.eigenlayer.xyz/overview/whitepaper), as well as [introductory information](https://docs.eigenlayer.xyz/overview/readme) and links to other documentation.

This repo contains our developer-oriented documentation; you can click the links in the Table of Contents below to access more specific documentation, or simply browse the [/docs/ folder](docs/). We recommend starting with the [EigenLayer Technical Specification](docs/EigenLayer-tech-spec.md) to get a better overview before diving into any of the other docs.

**Code4rena-specific note:** The scope for this Code4rena contest is somewhat limited. We recommend reading through the [contest scope section](#scope) below before diving too deep into the specifics.

## Table of Contents  
* [Introduction](#introduction)
* [Installation and Running Tests / Analyzers](#installation)
* [EigenLayer Technical Specification](docs/EigenLayer-tech-spec.md)

Design Docs
* [Withdrawals Design Doc](docs/Guaranteed-stake-updates.md)
* [EigenPods Design Doc](docs/EigenPods.md)

Flow Docs
* [EigenLayer Withdrawal Flow](docs/EigenLayer-withdrawal-flow.md)
* [EigenLayer Deposit Flow](docs/EigenLayer-deposit-flow.md)
* [EigenLayer Delegation Flow](docs/EigenLayer-delegation-flow.md)
* [Middleware Registration Flow for Operators](docs/Middleware-registration-operator-flow.md)

<a name="installation"/></a>
## Installation and Running Tests / Analyzers

### Installation

`foundryup`

This repository uses Foundry as a smart contract development toolchain.

See the [Foundry Docs](https://book.getfoundry.sh/) for more info on installation and usage.

### Natspec Documentation

You will notice that we also have hardhat installed in this repo. This is only used to generate natspec [docgen](https://github.com/OpenZeppelin/solidity-docgen). This is our workaround until foundry [finishes implementing](https://github.com/foundry-rs/foundry/issues/1675) the `forge doc` command.

To generate the docs, run `npx hardhat docgen` (you may need to run `npm install` first). The output is located in `docs/docgen`

### Run Tests

Prior to running tests, you should set up your environment. At present this repository contains fork tests against ETH mainnet; your environment will need an `RPC_MAINNET` key to run these tests. See the `.env.example` file for an example -- two simple options are to copy the LlamaNodes RPC url to your `env` or use your own infura API key in the provided format.

The main command to run tests is:

`forge test -vv`

### Run Static Analysis

`solhint 'src/contracts/**/*.sol'`

`slither .`

### Generate Inheritance and Control-Flow Graphs

first [install surya](https://github.com/ConsenSys/surya/)

then run

`surya inheritance ./src/contracts/**/*.sol | dot -Tpng > InheritanceGraph.png`

and/or

`surya graph ./src/contracts/middleware/*.sol | dot -Tpng > MiddlewareControlFlowGraph.png`

and/or

`surya mdreport surya_report.md ./src/contracts/**/*.sol`

<a name="scope"/></a>
# Scope

*List all files in scope in the table below (along with hyperlinks) -- and feel free to add notes here to emphasize areas of focus.*

*For line of code counts, we recommend using [cloc](https://github.com/AlDanial/cloc).* 

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [src/contracts/pods/EigenPod.sol](src/contracts/pods/EigenPod.sol) | 205 | The implementation contract used for restaking beacon chain ETH on EigenLayer | [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/pods/EigenPodPausingConstants.sol](src/contracts/pods/EigenPodPausingConstants.sol) | 8 | Constants shared between 'EigenPod' and 'EigenPodManager' contracts | N/A |
| [src/contracts/pods/DelayedWithdrawalRouter.sol](src/contracts/pods/DelayedWithdrawalRouter.sol) | 99 | Used for controlling withdrawals of ETH from EigenPods | [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/pods/EigenPodManager.sol](src/contracts/pods/EigenPodManager.sol) | 114 | The contract used for creating and managing EigenPods | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts), [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/permissions/Pausable.sol](src/contracts/permissions/Pausable.sol) | 57 | Adds pausability to a contract, implemented using bit switches | N/A |
| [src/contracts/permissions/PauserRegistry.sol](src/contracts/permissions/PauserRegistry.sol) | 32 | Defines pauser & unpauser roles + modifiers to be used elsewhere | N/A |
| [src/contracts/libraries/Merkle.sol](src/contracts/libraries/Merkle.sol) | 66 | Computes Merkle roots and checks proofs of inclusion | adapted from [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/libraries/Endian.sol](src/contracts/libraries/Endian.sol) | 15 | Flips Endianness of uint64's | N/A |
| [src/contracts/libraries/BeaconChainProofs.sol](src/contracts/libraries/BeaconChainProofs.sol) | 150 | Utility library for parsing and PHASE0 beacon chain block headers | N/A |
| [src/contracts/strategies/StrategyBase.sol](src/contracts/strategies/StrategyBase.sol) | 102 | Base implementation of `IStrategy` interface; holds a single token | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts), [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/core/StrategyManager.sol](src/contracts/core/StrategyManager.sol) | 414 | The primary entry- and exit-point for funds into and out of EigenLayer. | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts), [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/core/StrategyManagerStorage.sol](src/contracts/core/StrategyManagerStorage.sol) | 34 | Storage variables for the `StrategyManager` contract. | N/A |
| [src/contracts/interfaces/ISlasher.sol](src/contracts/interfaces/ISlasher.sol) | 11 | Interface for Slasher contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IPausable.sol](src/contracts/interfaces/IPausable.sol) | 4 | Interface for Pausable contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IBeaconChainOracle.sol](src/contracts/interfaces/IBeaconChainOracle.sol) | 3 | Interface for BeaconChainOracle contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IStrategy.sol](src/contracts/interfaces/IStrategy.sol) | 4 | Generalized interface for Strategy contracts | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IStrategyManager.sol](src/contracts/interfaces/IStrategyManager.sol) | 18 | Interface for StrategyManager contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IETHPOSDeposit.sol](src/contracts/interfaces/IETHPOSDeposit.sol) | 4 | Interface for the [ETH2 Deposit Contract](https://etherscan.io/address/0x00000000219ab540356cbb839cbe05303d7705fa#code) | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IDelayedWithdrawalRouter.sol](src/contracts/interfaces/IDelayedWithdrawalRouter.sol) | 11 | Interface for the DelayedWithdrawalRouter contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IEigenPod.sol](src/contracts/interfaces/IEigenPod.sol) | 23 | Interface for EigenPods | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IEigenPodManager.sol](src/contracts/interfaces/IEigenPodManager.sol) | 7 | Interface for EigenPodManager contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IPauserRegistry.sol](src/contracts/interfaces/IPauserRegistry.sol) | 3 | Interface for PauserRegistry contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IDelegationManager.sol](src/contracts/interfaces/IDelegationManager.sol) | 4 | Interface for DelegationManager contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IServiceManager.sol](src/contracts/interfaces/IServiceManager.sol) | 5 | Generalized interface for ServiceManager contracts | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |

## Out of scope

src/contracts/interfaces/IDelegationTerms.sol
src/contracts/interfaces/IVoteWeigher.sol
src/contracts/interfaces/IPaymentManager.sol
src/contracts/interfaces/IRegistry.sol
src/contracts/interfaces/IQuorumRegistry.sol
src/contracts/interfaces/IBLSPublicKeyCompendium.sol
src/contracts/interfaces/IWhitelister.sol
src/contracts/interfaces/IBLSRegistry.sol
src/contracts/interfaces/IDelayedService.sol
src/contracts/pods/BeaconChainOracle.sol
src/contracts/libraries/BytesLib.sol
src/contracts/libraries/MiddlewareUtils.sol
src/contracts/libraries/StructuredLinkedList.sol
src/contracts/libraries/BN254.sol
src/contracts/strategies/StrategyWrapper.sol
src/contracts/operators/MerkleDelegationTerms.sol
src/contracts/core/Slasher.sol
src/contracts/core/DelegationManager.sol
src/contracts/core/DelegationManagerStorage.sol
src/contracts/middleware/VoteWeigherBase.sol
src/contracts/middleware/BLSSignatureChecker.sol
src/contracts/middleware/RegistryBase.sol
src/contracts/middleware/BLSRegistry.sol
src/contracts/middleware/example/HashThreshold.sol
src/contracts/middleware/VoteWeigherBaseStorage.sol
src/contracts/middleware/PaymentManager.sol
src/contracts/middleware/example/ECDSARegistry.sol
src/contracts/middleware/BLSPublicKeyCompendium.sol

# Additional Context

*Describe any novel or unique curve logic or mathematical models implemented in the contracts*

*Sponsor, please confirm/edit the information below.*

## Scoping Details 
```
- If you have a public code repo, please share it here:  
- How many contracts are in scope?:  25 
- Total SLoC for these contracts?:  1560
- How many external imports are there?: 10 
- How many separate interfaces and struct definitions are there for the contracts within scope?:  ~11 interfaces, ~10 structs
- Does most of your code generally use composition or inheritance?:   Inheritance
- How many external calls?:   6
- What is the overall line coverage percentage provided by your tests?:  95
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:   true
- Please describe required context:   We will be excluding some parts of the protocol from scope, but understanding their interfaces / broad purposes may still be necessary. We are also doing proofs against Beacon Chain state, so understanding the details of the Beacon Chain / Execution Layer will be very helpful.
- Does it use an oracle?:  Others; Part of it is designed to interface with an oracle, but the exact details of the oracle are still TBD, and the oracle itself is considered out-of-scope. It is a custom oracle for bringing Beacon Chain roots to the Execution Layer (for proving against Beacon Chain state).
- Does the token conform to the ERC20 standard?:  
- Are there any novel or unique curve logic or mathematical models?: N/A
- Does it use a timelock function?:  
- Is it an NFT?: 
- Does it have an AMM?:   
- Is it a fork of a popular project?:   false
- Does it use rollups?:   
- Is it multi-chain?:  
- Does it use a side-chain?: false
- Describe any specific areas you would like addressed. E.g. Please try to break XYZ.": We are most concerned with a loss of user funds.
We’re aiming to launch with a very conservative design, in which all withdrawals from the system have a minimal enforced delay; we can respond to observations of any anomalous withdrawal behavior by pausing functionality and subsequently upgrading the contracts. The preliminary plan is to manually review user withdrawals until an automated solution is in place.
As such, any method to defeat these safeguards (i.e. to avoid the enforced minimum withdrawal delay) would also be of significant concern.
We’re also quite concerned with privilege escalation or the compromise of trusted roles; our docs will provide more details on trusted roles and the design philosophy we’ve taken here.
Another more specific concern we have is ensuring the correctness of the native restaking flow, i.e. “EigenPods” and their related functionality.  This is a rather complicated system with a lot of moving parts, and ensuring that our code accurately reflects the design of the consensus layer is important.
```

# Tests

*Provide every step required to build the project from a fresh git clone, as well as steps to run the tests with a gas report.* 

*Note: Many wardens run Slither as a first pass for testing.  Please document any known errors with no workaround.* 
