---
protocol: "Uniswap-V3"
website: "https://blog.uniswap.org/uniswap-v3"
x: "https://x.com/uniswap"
github: "https://github.com/Uniswap/v3-periphery/"
defillama_slug: "uniswap-v3"
chain: "Ethereum"
stage: 2
risks: ["L", "L", "L", "L", "L"]
author: ["mmilien"]
submission_date: "2024-11-12"
publish_date: "2024-12-16"
acknowledge_date: "1970-01-01"
update_date: "1970-01-01"
---

# Summary

Uniswap v3 is an AMM that builds upon Uniswap v2 by introducing a concentrated liquidity model, providing liquidity providers with granular control over capital allocation. Unlike v2, where liquidity is distributed uniformly across all price ranges, v3 allows LPs to specify custom price ranges in which their liquidity is active. This approach significantly improves capital efficiency, as LPs can concentrate their assets in high-demand price ranges, earning fees only within those specified ranges.
Uniswap v3 also introduces multiple fee tiers (0.01%, 0.05%, 0.3%, and 1%) to support different asset volatility profiles, allowing LPs to adjust their fee preferences based on expected risk and return. Additionally, it incorporates "range orders," which effectively turn liquidity positions into limit orders, further enhancing LP strategy flexibility.
The protocol is deployed across multiple chains enabling a wide range of use cases across decentralized finance (DeFi) applications.

# Overview

## Chain

Uniswap is deployed on Ethereum mainnet, Arbitrum, Optimism, Polygon, Base, BNB,
Avalanche C-Chain, CELO, Blast, ZKsync, Zora, and WorldChain. This analysis is
based on the contracts deployed on Ethereum mainnet.

> Chain score: L

## Upgradeability

The Uniswap DAO can change parameters such as fees through the GorvernorBravoDelegator contract.
Apart from the fees set by the governance, the protocol's contracts are immutable. No party is able to pause, revert trade execution, or otherwise change the behavior of the protocol.

No User funds nor unclaimed yield are affected by the upgradability.

Note that a TransparentProxy with the DAO as admin is used for the NonFungibleTokenPositionDescriptor, which is used for token descriptions.
This should not impact the safety of the protocol.

> Upgradeabillity score: L

## Autonomy

There are no particular dependencies for the Uniswap protocol.

> Autonomy score: L

## Exit Window

As the contracts are immutable the users can always withdraw their funds, but parameters such as protocol fees can be changed by the DAO. Propositions can be voted on over the duration of a week, and if a proposal is approved it can take effect between 2 and 30 days later.

> Exit score: L

## Accessibility

Although there is only a unique official uniswap frontend, Uniswap Labs offers an [open source interface](https://github.com/Uniswap/interface) for all Uniswap protocols. The web app is also hosted on IPFS. [Here](https://github.com/Uniswap/interface/releases) are the latest deployments on IPFS. Further details on the maintenance and access of the interface hosted on IPFS can be found [here](https://blog.uniswap.org/uniswap-interface-ipfs).

> Accessibility score: L

# Technical Analysis

## Contracts

| Contract Name                      | Address                                    |
| ---------------------------------- | ------------------------------------------ |
| UniswapV3Factory                   | 0x1F98431c8aD98523631AE4a59f267346ea31F984 |
| Multicall                          | 0x1F98415757620B543A52E61c46B32eB19261F984 |
| Multicall2                         | 0x5BA1e12693Dc8F9c48aAD8770482f4739bEeD696 |
| ProxyAdmin                         | 0xB753548F6E010e7e680BA186F9Ca1BdAB2E90cf2 |
| TickLens                           | 0xbfd8137f7d1516D3ea5cA83523914859ec47F573 |
| Quoter                             | 0xb27308f9F90D607463bb33eA1BeBb41C27CE5AB6 |
| SwapRouter                         | 0xE592427A0AEce92De3Edee1F18E0157C05861564 |
| NFTDescriptor                      | 0x42B24A95702b9986e82d421cC3568932790A48Ec |
| NonfungibleTokenPositionDescriptor | 0x91ae842A5Ffd8d12023116943e72A606179294f3 |
| TransparentUpgradeableProxy        | 0xEe6A57eC80ea46401049E92587E52f5Ec1c24785 |
| NonfungiblePositionManager         | 0xC36442b4a4522E871399CD717aBDD847Ab11FE88 |
| V3Migrator                         | 0xA5644E29708357803b5A882D272c41cC0dF92B34 |
| QuoterV2                           | 0x61fFE014bA17989E743c5F6cB21bF9697530B21e |
| SwapRouter02                       | 0x68b3465833fb72A70ecDF485E0e4C7bD8665Fc45 |
| Permit2                            | 0x000000000022D473030F116dDEE9F6B43aC78BA3 |
| UniversalRouter                    | 0x3fC91A3afd70395Cd496C647d5a6CC9D4B2b7FAD |
| v3StakerAddress                    | 0xe34139463bA50bD61336E0c446Bd8C0867c6fE65 |
| Timelock                           | 0x1a9C8182C09F50C8318d769245beA52c32BE35BC |
| GovernorBravo                      | 0x408ED6354d4973f66138C91495F2f2FCbd8724C3 |

## Permission owners

| Name          | Account                                                                            | Type     |
| ------------- | ---------------------------------------------------------------------------------- | -------- |
| ProxyAdmin    | [address](https://etherscan.io/address/0xB753548F6E010e7e680BA186F9Ca1BdAB2E90cf2) | Contract |
| GovernorBravo | [address](https://etherscan.io/address/0x408ED6354d4973f66138C91495F2f2FCbd8724C3) | Contract |
| TimeLock      | [address](https://etherscan.io/address/0x1a9C8182C09F50C8318d769245beA52c32BE35BC) | Contract |

## Permissions

| Contract                         | Function            | Impact                                                                                                                                                                                                                                    | Owner                                   |
| -------------------------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------- |
| UniswapV3Factory                 | setOwner            | changes the owner to a new address.                                                                                                                                                                                                       | TimeLocked DAO contract (GovernorBravo) |
| UniswapV3Factory                 | enableFeeAmount     | enables a fee amount for a given tick spacing.                                                                                                                                                                                            | TimeLocked DAO contract (GovernorBravo) |
| UniswapV3Factory (UniswapV3Pool) | setFeeProtocol      | set the protocol's % share of the fees.                                                                                                                                                                                                   | TimeLocked DAO contract (GovernorBravo) |
| UniswapV3Factory (UniswapV3Pool) | collectProtocol     | withdraw accumulated protocol fees to a custom address.                                                                                                                                                                                   | TimeLocked DAO contract (GovernorBravo) |
| ProxyAdmin                       | renounceOwnership   | abandons ownership of the contract, making the implementation immutable.                                                                                                                                                                  | TimeLocked DAO contract (GovernorBravo) |
| ProxyAdmin                       | changeProxyAdmin    | updates the admin of the proxy: the account with the rights to upgrade the proxy's implementation.                                                                                                                                        | TimeLocked DAO contract (GovernorBravo) |
| ProxyAdmin                       | transferOwnership   | changes the owner of the ProxyAdmin contract: the account with the rights to change the admin of the proxy and update the contract.                                                                                                       | TimeLocked DAO contract (GovernorBravo) |
| ProxyAdmin                       | upgrade             | triggers the upgrade of the NonFungibleTokenPositionDescriptor contract which allows to change the token descriptions.                                                                                                                    | TimeLocked DAO contract (GovernorBravo) |
| ProxyAdmin                       | upgradeAndCall      | triggers the upgrade of the NonFungibleTokenPositionDescriptor contract which allows to change the token descriptions and then call a function in the new contract.                                                                       | TimeLocked DAO contract (GovernerBravo) |
| TransparentUpgradeableProxy      | changeAdmin         | updates the proxy's admin: the account with the rights to upgrade the proxy's implementation.                                                                                                                                             | ProxyAdmin                              |
| TransparentUpgradeableProxy      | upgradeTo           | upgrades the NonFungibleTokenPositionDescriptor contract which allows to change the token descriptions.                                                                                                                                   | ProxyAdmin                              |
| TransparentUpgradeableProxy      | upgradeToAndCall    | upgrades the NonFungibleTokenPositionDescriptor contract which allows to change the token descriptions and then call a function in the new contract.                                                                                      | ProxyAdmin                              |
| TimeLock                         | queueTransaction    | queues a transaction that can be executed once a delay (between 2 and 30 days) has passed. This can impact the own TimeLock's settings (change admin, set delays) or interaction with any other contract the TimeLock has permissions on. | GovernorBravoDelegator                  |
| TimeLock                         | cancelTransaction   | cancels a pending transaction.                                                                                                                                                                                                            | GovernorBravo                           |
| TimeLock                         | executeTransaction  | executes a transaction that was previously queued, if the corresponding delay has passed.                                                                                                                                                 | GovernorBravoDelegator                  |
| GovernorBravoDelegator           | \_setImplementation | updates the implementation of the GovernorBravo (DAO) contract. Can only be triggered by the DAO itself.                                                                                                                                  | TimeLocked DAO contract (GovernerBravo) |

## Dependencies

No external dependency has been found.

## Exit Window

As the contracts are immutable the users can always withdraw their funds, but parameters such as protocol
fees can be changed by the DAO. Timelocks protect the contracts and update are governed by the Governor Bravo contract.
The lock period is at least two days and up to 30 days for governance actions.
When a proposal is created (at least 2.5M Uni), the community can cast their votes during a 3 day voting period. If a majority, and at least 4M votes are cast for the proposal, it is queued in the Timelock, and may be executed in a minimum of 2 days.

## Security Council

No security council needed because on-chain governance is in place.
