# EigenLayer contest details
- Total Prize Pool: $90,500 USDC 
  - HM awards: $56,250 USDC 
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

Automated findings output for the contest can be found [here](add link to report) within 24 hours of contest opening.

*Note for C4 wardens: Anything included in the automated findings output is considered a publicly known issue and is ineligible for awards.*

EigenLayer has completed one security audit with Consensys Diligence and is currently concluding a second independent audit with Sigma Prime. We note that the scope for the Sigma Prime audit is expanded relative to the scope of this contest, and that the report provided here is in draft form, so it does not yet capture any mitigations taken by the team. All findings of the following audits are considered out-of-scope:

- [Consensys Diligence audit](https://consensys.net/diligence/audits/2023/03/eigenlabs-eigenlayer/)
- [Sigma Prime audit](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/audits/Sigma_Prime_Layr_Labs_Eigen_Layer_2_Security_Assessment_DRAFT.pdf)


# Overview

# EigenLayer

EigenLayer (formerly 'EigenLayr') is a set of smart contracts deployed on Ethereum that enable restaking of assets to secure new services.
At present, this repository contains *both* the contracts for EigenLayer *and* a set of general "middleware" contracts, designed to be reuseable across different applications built on top of EigenLayer.

Note that the interactions between middleware and EigenLayer are not yet "set in stone", and may change somewhat prior to the platform being fully live on mainnet; in particular, payment architecture is likely to evolve. As such, the "middleware" contracts should not be treated as definitive, but merely as a helpful reference, at least until the architecture is more settled.

The EigenLayer whitepaper is available [on our website](https://docs.eigenlayer.xyz/overview/whitepaper), as well as [introductory information](https://docs.eigenlayer.xyz/overview/readme) and links to other documentation.

This repo contains our developer-oriented documentation; you can click the links in the Table of Contents below to access more specific documentation, or simply browse the [/docs/ folder](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/). We recommend starting with the [EigenLayer Technical Specification](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/EigenLayer-tech-spec.md) to get a better overview before diving into any of the other docs.

**Code4rena-specific note:** The scope for this Code4rena contest is somewhat limited. We recommend reading through the [contest scope section](#scope) below before diving too deep into the specifics.

## Table of Contents  

* [Introduction](#eigenlayer)
* [Installation and Running Tests / Analyzers](#tests--installation)
* [EigenLayer Technical Specification](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/EigenLayer-tech-spec.md)

Design Docs
* [Withdrawals Design Doc](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/Guaranteed-stake-updates.md)
* [EigenPods Design Doc](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/EigenPods.md)

Flow Docs
* [EigenLayer Withdrawal Flow](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/EigenLayer-withdrawal-flow.md)
* [EigenLayer Deposit Flow](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/EigenLayer-deposit-flow.md)
* [EigenLayer Delegation Flow](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/EigenLayer-delegation-flow.md)
* [Middleware Registration Flow for Operators](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/docs/Middleware-registration-operator-flow.md)

# Scope

| Contract | SLOC | Purpose | Libraries used |  
| ----------- | ----------- | ----------- | ----------- |
| [src/contracts/core/StrategyManager.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/core/StrategyManager.sol) | 414 | The primary entry- and exit-point for funds into and out of EigenLayer. | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts), [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/core/StrategyManagerStorage.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/core/StrategyManagerStorage.sol) | 34 | Storage variables for the `StrategyManager` contract. | N/A |
| [src/contracts/strategies/StrategyBase.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/strategies/StrategyBase.sol) | 102 | Base implementation of `IStrategy` interface; holds a single token | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts), [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/pods/EigenPodManager.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/pods/EigenPodManager.sol) | 114 | The contract used for creating and managing EigenPods | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts), [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/pods/EigenPod.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/pods/EigenPod.sol) | 205 | The implementation contract used for restaking beacon chain ETH on EigenLayer | [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/pods/EigenPodPausingConstants.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/pods/EigenPodPausingConstants.sol) | 8 | Constants shared between 'EigenPod' and 'EigenPodManager' contracts | N/A |
| [src/contracts/pods/DelayedWithdrawalRouter.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/pods/DelayedWithdrawalRouter.sol) | 99 | Used for controlling withdrawals of ETH from EigenPods | [`@openzeppelin-upgrades/*`](https://github.com/OpenZeppelin/openzeppelin-contracts-upgradeable) |
| [src/contracts/permissions/Pausable.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/permissions/Pausable.sol) | 57 | Adds pausability to a contract, implemented using bit switches | N/A |
| [src/contracts/permissions/PauserRegistry.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/permissions/PauserRegistry.sol) | 32 | Defines pauser & unpauser roles + modifiers to be used elsewhere | N/A |
| [src/contracts/libraries/BeaconChainProofs.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/libraries/BeaconChainProofs.sol) | 150 | Utility library for parsing and PHASE0 beacon chain block headers | N/A |
| [src/contracts/libraries/Merkle.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/libraries/Merkle.sol) | 66 | Computes Merkle roots and checks proofs of inclusion | adapted from [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/libraries/Endian.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/libraries/Endian.sol) | 15 | Flips Endianness of uint64's | N/A |
| [src/contracts/interfaces/ISlasher.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/ISlasher.sol) | 11 | Interface for Slasher contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IPausable.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IPausable.sol) | 4 | Interface for Pausable contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IBeaconChainOracle.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IBeaconChainOracle.sol) | 3 | Interface for BeaconChainOracle contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IStrategy.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IStrategy.sol) | 4 | Generalized interface for Strategy contracts | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IStrategyManager.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IStrategyManager.sol) | 18 | Interface for StrategyManager contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IETHPOSDeposit.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IETHPOSDeposit.sol) | 4 | Interface for the [ETH2 Deposit Contract](https://etherscan.io/address/0x00000000219ab540356cbb839cbe05303d7705fa#code) | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IDelayedWithdrawalRouter.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IDelayedWithdrawalRouter.sol) | 11 | Interface for the DelayedWithdrawalRouter contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IEigenPod.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IEigenPod.sol) | 23 | Interface for EigenPods | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IEigenPodManager.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IEigenPodManager.sol) | 7 | Interface for EigenPodManager contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IPauserRegistry.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IPauserRegistry.sol) | 3 | Interface for PauserRegistry contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IDelegationManager.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IDelegationManager.sol) | 4 | Interface for DelegationManager contract | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |
| [src/contracts/interfaces/IServiceManager.sol](https://github.com/code-423n4/2023-04-eigenlayer/tree/main/src/contracts/interfaces/IServiceManager.sol) | 5 | Generalized interface for ServiceManager contracts | [`@openzeppelin/*`](https://github.com/OpenZeppelin/openzeppelin-contracts) |

## Out of scope

All files not listed above. Semi-complete list: 
- src/contracts/interfaces/IDelegationTerms.sol 
- src/contracts/interfaces/IVoteWeigher.sol 
- src/contracts/interfaces/IPaymentManager.sol 
- src/contracts/interfaces/IRegistry.sol 
- src/contracts/interfaces/IQuorumRegistry.sol 
- src/contracts/interfaces/IBLSPublicKeyCompendium.sol 
- src/contracts/interfaces/IWhitelister.sol 
- src/contracts/interfaces/IBLSRegistry.sol 
- src/contracts/interfaces/IDelayedService.sol 
- src/contracts/pods/BeaconChainOracle.sol 
- src/contracts/libraries/BytesLib.sol 
- src/contracts/libraries/MiddlewareUtils.sol 
- src/contracts/libraries/StructuredLinkedList.sol 
- src/contracts/libraries/BN254.sol 
- src/contracts/strategies/StrategyWrapper.sol 
- src/contracts/operators/MerkleDelegationTerms.sol 
- src/contracts/core/Slasher.sol 
- src/contracts/core/DelegationManager.sol 
- src/contracts/core/DelegationManagerStorage.sol 
- src/contracts/middleware/* 
- src/test/* 
- script/* 
- certora/* 

# Additional Context

## Scoping Details 
```
- If you have a public code repo, please share it here:  https://github.com/Layr-Labs/eigenlayer-contracts/
- How many contracts are in scope?:  24 
- Total SLoC for these contracts?:  1393
- How many external imports are there?: 10 
- How many separate interfaces and struct definitions are there for the contracts within scope?:  ~11 interfaces, ~10 structs
- Does most of your code generally use composition or inheritance?:   Inheritance
- How many external calls?:   6
- What is the overall line coverage percentage provided by your tests?:  95
- Is there a need to understand a separate part of the codebase / get context in order to audit this part of the protocol?:   true
- Please describe required context:   We will be excluding some parts of the protocol from scope, but understanding their interfaces and/or broad purposes may still be necessary. We are also doing proofs against Beacon Chain state, so understanding the details of the Beacon Chain & Execution Layer will be very helpful.
- Does it use an oracle?:  Others; Part of it is designed to interface with an oracle, but the exact details of the oracle are still TBD, and the oracle itself is considered out-of-scope. It is a custom oracle for bringing Beacon Chain roots to the Execution Layer (for proving against Beacon Chain state). The IBeaconChainOracle interface is included in the scope since the EigenPodManager will interact with this oracle for fetching state roots.
- Does the token conform to the ERC20 standard?:  N/A
- Are there any novel or unique curve logic or mathematical models?: N/A
- Does it use a timelock function?:  no
- Is it an NFT?: no
- Does it have an AMM?: no
- Is it a fork of a popular project?:   false
- Does it use rollups?:   no
- Is it multi-chain?:  no
- Does it use a side-chain?: false
- Describe any specific areas you would like addressed. E.g. Please try to break XYZ.": We are most concerned with a loss of user funds.
We’re aiming to launch with a very conservative design, in which all withdrawals from the system have a minimal enforced delay; we can then respond to observations of any anomalous withdrawal behavior by pausing functionality and subsequently upgrading the contracts. As such, any method to defeat these safeguards (i.e. to avoid the enforced minimum withdrawal delay) would also be of significant concern.
We’re also quite concerned with privilege escalation or the compromise of trusted roles; our docs will provide more details on trusted roles and the design philosophy we’ve taken here.
Another more specific concern we have is ensuring the correctness of the native restaking flow, i.e. “EigenPods” and their related functionality.  This is a rather complicated system with a lot of moving parts, and ensuring that our code accurately reflects the specification of the Consensus Layer is important.
```

# Tests & Installation

### Installation


This repository uses Foundry as a smart contract development toolchain.

See the [Foundry Docs](https://book.getfoundry.sh/) for more info on installation and usage.

```bash
foundryup
forge install
```

### Run Tests

Prior to running tests, you should set up your environment. At present this repository contains fork tests against ETH mainnet; your environment will need an `RPC_MAINNET` key to run these tests. See the `.env.example` file for an example -- two simple options are to copy the LlamaNodes RPC url to your `env` or use your own infura API key in the provided format.

The main command to run tests is:

`forge test -vv`

### Run Static Analysis

`solhint 'src/contracts/**/*.sol'`

`slither .`
