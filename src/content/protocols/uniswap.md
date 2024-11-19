---
protocol: "Uniswap-V3"
website: "https://blog.uniswap.org/uniswap-v3"
x: "https://x.com/uniswap"
github: "https://github.com/Uniswap/v3-periphery/"
defillama_slug: "the slug used by https://defillama.com"
chain: "Ethereum Mainnet"
stage: 0
risks: ["L", "H", "L", "M", "M"]
author: ["Emilien"]
submission_date: "2024-11-12"
publish_date: "1970-01-01"
acknowledge_date: "1970-01-01"
update_date: "1970-01-01"
---

# Summary

Uniswap is an Automated Market Maker (AMM), a type of exchange where anyone can pool assets into shared market making strategies.
Uniswap serves critical infrastructure for decentralized finance, empowering developers, traders, and liquidity providers to participate in a secure and robust financial marketplace.

Uniswap v3 introduces:

- Concentrated liquidity, giving individual LPs granular control over what price ranges their capital is allocated to. Individual positions are aggregated together into a single pool, forming one combined curve for users to trade against

- Multiple fee tiers, allowing LPs to be appropriately compensated for taking on varying degrees of risk

# Overview

## Chain

See http://localhost:3000/learn-more#chain for more guidance.

## Upgradeability

See http://localhost:3000/learn-more#upgradability for more guidance.

## Autonomy

See http://localhost:3000/learn-more#autonomy for more guidance.

## Exit Window

See http://localhost:3000/learn-more#exit-window for more guidance.

## Accessibility

See http://localhost:3000/learn-more#accessibility for more guidance.

# Technical Analysis

## Contracts

| Contract Name                      | Address                                    |
| ---------------------------------- | ------------------------------------------ |
| UniswapV3Factory                   | 0x1F98431c8aD98523631AE4a59f267346ea31F984 |
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

## Permission owners

| Name                        | Account                                                                            | Type                |
| --------------------------- | ---------------------------------------------------------------------------------- | ------------------- |
| TimeLock                    | [address](https://etherscan.io/address/0x1a9C8182C09F50C8318d769245beA52c32BE35BC) | TimeLock Contract   |
| TimeLock                    | [address](https://etherscan.io/address/0x1a9C8182C09F50C8318d769245beA52c32BE35BC) | TimeLock Contract   |
| ProxyAdmin                  | [address](https://etherscan.io/address/0x1a9C8182C09F50C8318d769245beA52c32BE35BC) | TimeLock Contract   |
| TransparentUpgradeableProxy | [address](https://etherscan.io/address/0xB753548F6E010e7e680BA186F9Ca1BdAB2E90cf2) | ProxyAdmin Contract |
| GovernerBravo               | [address](https://etherscan.io/address/0x408ED6354d4973f66138C91495F2f2FCbd8724C3) | Contract            |

## Permissions

| Contract                    | Function          | Impact                             | Owner                                |
| --------------------------- | ----------------- | ---------------------------------- | ------------------------------------ |
| UniswapV3Factory            | setOwner          | changes the owner to a new address | TimeLock contract (Governor Bravo)   |
| UniswapV3Factory            | enableFeeAmount   | changes the owner to a new address | TimeLock contract (Governor Bravo)   |
| UniswapV3Pool               | setFeeProtocol    | changes the fees                   | TimeLock contract (Governor Bravo)   |
| UniswapV3Pool               | collectProtocol   | withdraw accumulated fees          | TimeLock contract (Governor Bravo)   |
| ProxyAdmin                  | renounceOwnership | abandons ownership to the contract | TimeLock contract (Governor Bravo)   |
| ProxyAdmin                  | changeProxyAdmin  | changes ownership to the contract  | TimeLock contract (Governor Bravo)   |
| ProxyAdmin                  | upgrade           | upgrades the contract              | TimeLock contract (Governor Bravo)   |
| TransparentUpgradeableProxy | admin             | changes the fees                   | ProxyAdmin Contract (Governor Bravo) |
| TransparentUpgradeableProxy | implementation    | changes the fees                   | ProxyAdmin Contract (Governor Bravo) |
| TransparentUpgradeableProxy | changeAdmin       | update the admin                   | ProxyAdmin Contract (Governor Bravo) |
| TransparentUpgradeableProxy | upgradeTo         | upgrade the contract               | ProxyAdmin Contract (Governor Bravo) |
| TransparentUpgradeableProxy | upgradeToAndCall  | upgrade the contract               | ProxyAdmin Contract (Governor Bravo) |
| Ownable                     | transferOwnership | changes the owner                  | 0x0                                  |
| Ownable                     | renounceOwnership | abandons ownership                 | 0x0                                  |
| ProxyAdmin                  | transferOwnership | changes the owner                  | 0x0                                  |
| ProxyAdmin                  | renounceOwnership | abandons ownership                 | 0x0                                  |

## Dependencies

No external dependency has been found.

## Exit Window

Timelocks protect the contracts and update are governed by the Governor Bravo contract. The lock period is at least two days and up to 30 days
for governance actions.

# Security Council

When a proposal is created (at least 2.5M Uni), the community can cast their votes during a 3 day voting period. If a majority, and at least 4M votes are cast for the proposal, it is queued in the Timelock, and may be executed in a minimum of 2 days.

change ✅ or ❌ accordingly

| ✅ /❌ | Requirement                                             |
| ------ | ------------------------------------------------------- |
| ❌     | At least 7 signers                                      |
| ✅     | At least 51% threshold                                  |
| ❌     | At least 50% non-team signers                           |
| ❌     | Signers are publicly announced (with name or pseudonym) |
