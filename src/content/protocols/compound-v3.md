---
protocol: "Compound-V3"
website: "https://compound.finance/"
x: "https://x.com/compoundfinance"
github: ["https://github.com/compound-finance/compound-protocol"]
defillama_slug: ["compound-v3"]
chain: "Ethereum"
stage: 0
reasons: ["Incorrect Docs"]
risks: ["L", "H", "L", "H", "M"]
author: ["mmilien_"]
submission_date: "2025-01-26"
publish_date: "1970-01-01"
acknowledge_date: "1970-01-01"
update_date: "1970-01-01"
---

# Summary

Compound III is an EVM compatible protocol that enables supplying of crypto assets as collateral in order to borrow the base asset. Multiple base assets are supported such as USDC, WETH, USDT, wstETH, and USDS. Accounts can also earn interest by supplying the base asset to the protocol. The market logic of each base asset is implemented in respective `Comet`contracts. Those `Comet`s are deployed by the `Comet Factory` using the `Configurator`. The `Configurator` holds the parameters of each market. A new `Comet` contract needs to be deployed every time the parameters of the market change.

# Overview

## Chain

The initial deployment of Compound III is on Ethereum mainnet.

> Risk: Low

## Upgradeability

The `Comet`s and `Configuration` contracts can be changed at any time with a delay of 2 days once a proposal is accepted. The comet contracts manages all deposits and borrows. A PauseGuardian (security council) can pause the contracts at any time.
If a malicious upgrade passes the governance vote a ProposalGuardian (currently same as PauseGuardian) can cancel proposals
before execution.

Any `Comet`parameter change requires a new deployment.The process is as follows:

1.  new parameters are set using setters in the `Configurator` contract.
2.  the `ProxyAdmin` contract uses `deployAndUpgradeTo` to deploy a new comet contract using the `Configurator` and update the corresponding `Comet Proxy` to point to this newly deployed contract.

A malicious update could simply perform the update to the `Comet Proxy`to introduce a malicious `Comet` contract, effectively stealing funds.
This is because the `upgrade` function can still be called by the DAO in addition to `deployAndUpgradeTo`, allowing the DAO to deploy `Comet` contracts not created by the `Configurator`.

> Risk: High

## Autonomy

The protocol uses Chainlink's oracle to get the price of the base token. If the oracle fails it may
be replaced only with a contract upgrade triggered from the DAO (5+ days delay).

> Risk: Low

## Exit Window

Once an upgrade is approved by the governance there is a delay of 2 days allowing users to react. Anyone with more than 25'000 Comp can create a proposal, each proposal has a minimum voting time of 3 days and requires at least 400'000 votes to be valid. A malicious upgrade could hijack user funds if it is not blocked by the ProposalGuardian.
In addition to that, the tranfers/deposits/withdrawals can be paused by the PauseGuardian (Security Council) with no delay, freezing all assets.

> Risk: High

## Accessibility

The frontend of Compound V3 is open source. Instructions to deploy it locally or deploy it
on IPFS are available [here](https://github.com/compound-finance/palisade). There is no registry
of alternative deployments.

> Risk: Medium

# Technical Analysis

## Contracts

| Contract Name                      | Address                                                                                                               |
| ---------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| cUSDCv3 (Comet Proxy)              | [0xc3d688B66703497DAA19211EEdff47f25384cdc3](https://etherscan.io/address/0xc3d688B66703497DAA19211EEdff47f25384cdc3) |
| cUSDCv3 (Comet Implementation)     | [0xaeC1954467B6d823A9042E9e9D6E4F40111069a9](https://etherscan.io/address/0xaeC1954467B6d823A9042E9e9D6E4F40111069a9) |
| cUSDCv3 Ext                        | [0x285617313887d43256F852cAE0Ee4de4b68D45B0](https://etherscan.io/address/0x285617313887d43256F852cAE0Ee4de4b68D45B0) |
| cWETHv3 (Comet Proxy)              | [0xA17581A9E3356d9A858b789D68B4d866e593aE94](https://etherscan.io/address/0xA17581A9E3356d9A858b789D68B4d866e593aE94) |
| cWETHv3 (Comet Implementation)     | [0x318b8615643bdae03B7ca63E69b3f06ff1af0bC7](https://etherscan.io/address/0x318b8615643bdae03B7ca63E69b3f06ff1af0bC7) |
| cWETHv3 Ext                        | [0xe2C1F54aFF6b38fD9DF7a69F22cB5fd3ba09F030](https://etherscan.io/address/0xe2C1F54aFF6b38fD9DF7a69F22cB5fd3ba09F030) |
| cUSDTv3 (Comet Proxy)              | [0x3Afdc9BCA9213A35503b077a6072F3D0d5AB0840](https://etherscan.io/address/0x3Afdc9BCA9213A35503b077a6072F3D0d5AB0840) |
| cUSDTv3 (Comet Implementation)     | [0xf930618E2202e6A2a20606AE89ef7406974622e7](https://etherscan.io/address/0xf930618E2202e6A2a20606AE89ef7406974622e7) |
| cUSDTv3 Ext                        | [0x5C58d4479A1E9b2d19EE052143FA73F0ee79A36e](https://etherscan.io/address/0x5C58d4479A1E9b2d19EE052143FA73F0ee79A36e) |
| cwstETHv3 (Comet Proxy)            | [0x3D0bb1ccaB520A66e607822fC55BC921738fAFE3](https://etherscan.io/address/0x3D0bb1ccaB520A66e607822fC55BC921738fAFE3) |
| cwstETHv3 (Comet Implementation)   | [0x25DabAB0c230131aaE9B312Ce1591934f43e024A](https://etherscan.io/address/0x25DabAB0c230131aaE9B312Ce1591934f43e024A) |
| cwstETHv3 Ext                      | [0x995E394b8B2437aC8Ce61Ee0bC610D617962B214](https://etherscan.io/address/0x995E394b8B2437aC8Ce61Ee0bC610D617962B214) |
| cUSDSv3 (Comet Proxy)              | [0x5D409e56D886231aDAf00c8775665AD0f9897b56](https://etherscan.io/address/0x5D409e56D886231aDAf00c8775665AD0f9897b56) |
| cUSDSv3 (Comet Implementation)     | [0xBEBbC5Fc967D8425CF96e97838249eBc9495F9A3](https://etherscan.io/address/0xBEBbC5Fc967D8425CF96e97838249eBc9495F9A3) |
| cUSDSv3 Ext                        | [0x95DeDD64b551F05E9f59a101a519B024b6b116E7](https://etherscan.io/address/0x95DeDD64b551F05E9f59a101a519B024b6b116E7) |
| Bulker                             | [0x74a81F84268744a40FEBc48f8b812a1f188D80C3](https://etherscan.io/address/0x74a81F84268744a40FEBc48f8b812a1f188D80C3) |
| Configurator                       | [0x316f9708bB98af7dA9c68C1C3b5e79039cD336E3](https://etherscan.io/address/0x316f9708bB98af7dA9c68C1C3b5e79039cD336E3) |
| Configurator Implementation        | [0xcFC1fA6b7ca982176529899D99af6473aD80DF4F](https://etherscan.io/address/0xcFC1fA6b7ca982176529899D99af6473aD80DF4F) |
| Proxy Admin                        | [0x1EC63B5883C3481134FD50D5DAebc83Ecd2E8779](https://etherscan.io/address/0x1EC63B5883C3481134FD50D5DAebc83Ecd2E8779) |
| Comet Factory                      | [0xa7F7De6cCad4D83d81676717053883337aC2c1b4](https://etherscan.io/address/0xa7F7De6cCad4D83d81676717053883337aC2c1b4) |
| Rewards                            | [0x1B0e765F6224C21223AeA2af16c1C46E38885a40](https://etherscan.io/address/0x1B0e765F6224C21223AeA2af16c1C46E38885a40) |
| Compound Governor (Proxy)          | [0x309a862bbC1A00e45506cB8A802D1ff10004c8C0](https://etherscan.io/address/0x309a862bbC1A00e45506cB8A802D1ff10004c8C0) |
| Compound Governor (Implementation) | [0x501Eb63A2120418C581B3bD31CF190b0a0616752](https://etherscan.io/address/0x501Eb63A2120418C581B3bD31CF190b0a0616752) |
| TimeLock                           | [0x6d903f6003cca6255D85CcA4D3B5E5146dC33925](https://etherscan.io/address/0x6d903f6003cca6255D85CcA4D3B5E5146dC33925) |
| Comp                               | [0xc00e94Cb662C3520282E6f5717214004A7f26888](https://etherscan.io/address/0xc00e94Cb662C3520282E6f5717214004A7f26888) |

## Permission owners

| Name              | Account                                                                                                               | Type         |
| ----------------- | --------------------------------------------------------------------------------------------------------------------- | ------------ |
| Pause Guardian    | [0xbbf3f1421D886E9b2c5D716B5192aC998af2012c](https://etherscan.io/address/0xbbf3f1421D886E9b2c5D716B5192aC998af2012c) | Multisig 4/8 |
| Proposal Guardian | [0xbbf3f1421D886E9b2c5D716B5192aC998af2012c](https://etherscan.io/address/0xbbf3f1421D886E9b2c5D716B5192aC998af2012c) | Multisig 4/8 |
| TimeLock (DAO)    | [0x6d903f6003cca6255D85CcA4D3B5E5146dC33925](https://etherscan.io/address/0x6d903f6003cca6255D85CcA4D3B5E5146dC33925) | Contract     |
| Governor (DAO)    | [0xc0Da02939E1441F497fd74F78cE7Decb17B66529](https://etherscan.io/address/0xc0Da02939E1441F497fd74F78cE7Decb17B66529) | Contract     |

## Permissions

| Contract                      | Function                              | Impact                                                                                                                                                                                                                                                  | Owner                          |
| ----------------------------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| Comet Proxy                   | changeAdmin                           | Updates the admin of this proxy contract. The admin can update the implementation contract. The new admin would replace the `ProxyAdmin` contract, it could be used if the `ProxyAdmin` contract is upgraded/replaced.                                  | ProxyAdmin                     |
| Comet Proxy                   | upgradeTo                             | Triggers the update of the `Comet Implementation` contract with a new contract.                                                                                                                                                                         | ProxyAdmin                     |
| Comet Proxy                   | upgradeToAndCall                      | Triggers the update of the `Comet Implementation` contract with a new contract and then calls a function in the new contract.                                                                                                                           | ProxyAdmin                     |
| Comet Implementation          | pause                                 | This function pauses the specified protocol functionality in the event of an unforeseen vulnerability. Deposits, transfers, and withdrawals may be paused. This may be called by either the DAO or the Pause Guardian.                                  | Timelock (DAO) & PauseGuardian |
| Comet Implementation          | withdrawReserves                      | Allows governance to withdraw base token reserves from the protocol and send them to a specified address.                                                                                                                                               | Timelock (DAO)                 |
| Comet Implementation          | approveThis                           | Sets the Comet contract’s ERC-20 allowance of an asset for a manager address. The approved address can freely transfer ERC-20 tokens out of the Comet contract.                                                                                         | Timelock (DAO)                 |
| Configurator (Proxy)          | changeAdmin                           | Update the admin of this proxy contract. The admin can update the implementation contract. The new admin would replace the `ProxyAdmin` contract, it could be used if the `ProxyAdmin` contract is upgraded/replaced.                                   | Timelock (DAO)                 |
| Configurator (Proxy)          | upgradeTo                             | Triggers the update of the `Congiruator Implementation` contract with a new contract.                                                                                                                                                                   | Timelock (DAO)                 |
| Configurator (Proxy)          | upgradeToAndCall                      | Triggers the update of the `Congiruator Implementation` contract with a new contract and then calls a function in the new contract.                                                                                                                     | Timelock (DAO)                 |
| Configurator (Implementation) | setFactory                            | Sets the official contract address of the `Comet Factory`.                                                                                                                                                                                              | Timelock (DAO)                 |
| Configurator (Implementation) | setConfiguration                      | Sets the entire Configuration for a `Comet Proxy` contract.                                                                                                                                                                                             | Timelock (DAO)                 |
| Configurator (Implementation) | setGovernor                           | Sets the official contract address of the Compound III protocol Governor for subsequent proposals.                                                                                                                                                      | Timelock (DAO)                 |
| Configurator (Implementation) | setPauseGuardian                      | Sets the official contract address of the Compound III protocol pause guardian. This address has the power to pause supply, transfer, withdraw, absorb, and buy collateral operations within Compound III.                                              | Timelock (DAO)                 |
| Configurator (Implementation) | setBaseTokenPriceFeed                 | Sets the official contract address of the price feed of the protocol base asset.                                                                                                                                                                        | Timelock (DAO)                 |
| Configurator (Implementation) | setExtensionDelegate                  | Sets the official contract address of the protocol’s `Comet` extension delegate. The methods in `CometExt.sol` are able to be called via the same proxy as `Comet`.sol.                                                                                 | Timelock (DAO)                 |
| Configurator (Implementation) | setSupplyKink                         | Sets the supply interest rate utilization curve kink for the Compound III base asset.                                                                                                                                                                   | Timelock (DAO)                 |
| Configurator (Implementation) | setSupplyPerYearInterestRateSlopeLow  | Sets the supply interest rate slope low bound in the approximate amount of seconds in one year.                                                                                                                                                         | Timelock (DAO)                 |
| Configurator (Implementation) | setSupplyPerYearInterestRateSlopeHigh | Sets the supply interest rate slope high bound in the approximate amount of seconds in one year.                                                                                                                                                        | Timelock (DAO)                 |
| Configurator (Implementation) | setSupplyPerYearInterestRateBase      | Sets the supply interest rate slope base in the approximate amount of seconds in one year.                                                                                                                                                              | Timelock (DAO)                 |
| Configurator (Implementation) | setBorrowKink                         | Sets the borrow interest rate utilization curve kink for the Compound III base asset.                                                                                                                                                                   | Timelock (DAO)                 |
| Configurator (Implementation) | setBorrowPerYearInterestRateSlopeLow  | Sets the borrow interest rate slope low bound in the approximate amount of seconds in one year.                                                                                                                                                         | Timelock (DAO)                 |
| Configurator (Implementation) | setBorrowPerYearInterestRateSlopeHigh | Sets the borrow interest rate slope high bound in the approximate amount of seconds in one year.                                                                                                                                                        | Timelock (DAO)                 |
| Configurator (Implementation) | setBorrowPerYearInterestRateBase      | Sets the borrow interest rate slope base in the approximate amount of seconds in one year.                                                                                                                                                              | Timelock (DAO)                 |
| Configurator (Implementation) | setStoreFrontPriceFactor              | Sets the fraction of the liquidation penalty that goes to buyers of collateral instead of the protocol. This factor is used to calculate the discount rate of collateral for sale as part of the account absorption process.                            | Timelock (DAO)                 |
| Configurator (Implementation) | setBaseTrackingSupplySpeed            | Sets the rate at which base asset supplier accounts accrue rewards.                                                                                                                                                                                     | Timelock (DAO)                 |
| Configurator (Implementation) | setBaseTrackingBorrowSpeed            | Sets the rate at which base asset borrower accounts accrue rewards.                                                                                                                                                                                     | Timelock (DAO)                 |
| Configurator (Implementation) | setBaseMinForRewards                  | Sets the minimum amount of base asset supplied to the protocol in order for accounts to accrue rewards.                                                                                                                                                 | Timelock (DAO)                 |
| Configurator (Implementation) | setBaseBorrowMin                      | Sets the minimum amount of base token that is allowed to be borrowed.                                                                                                                                                                                   | Timelock (DAO)                 |
| Configurator (Implementation) | setTargetReserves                     | Sets the target reserves amount. Once the protocol reaches this amount of reserves of base asset, liquidators cannot buy collateral from the protocol.                                                                                                  | Timelock (DAO)                 |
| Configurator (Implementation) | addAsset                              | Adds an asset to the protocol through governance.                                                                                                                                                                                                       | Timelock (DAO)                 |
| Configurator (Implementation) | updateAsset                           | Modifies an existing asset’s configuration parameters.                                                                                                                                                                                                  | Timelock (DAO)                 |
| Configurator (Implementation) | updateAssetPriceFeed                  | Updates the price feed contract address for a specific asset.                                                                                                                                                                                           | Timelock (DAO)                 |
| Configurator (Implementation) | updateAssetBorrowCollateralFactor     | Updates the borrow collateral factor for an asset in the protocol.                                                                                                                                                                                      | Timelock (DAO)                 |
| Configurator (Implementation) | updateAssetLiquidateCollateralFactor  | Updates the liquidation collateral factor for an asset in the protocol.                                                                                                                                                                                 | Timelock (DAO)                 |
| Configurator (Implementation) | updateAssetLiquidationFactor          | Updates the liquidation factor for an asset in the protocol.                                                                                                                                                                                            | Timelock (DAO)                 |
| Configurator (Implementation) | updateAssetSupplyCap                  | Sets the maximum amount of an asset that can be supplied to the protocol. Supply transactions will revert if the total supply would be greater than this number as a result.                                                                            | Timelock (DAO)                 |
| Configurator (Implementation) | transferGovernor                      | Changes the address of the Configurator’s Governor.                                                                                                                                                                                                     | Timelock (DAO)                 |
| Proxy Admin                   | changeProxyAdmin                      | Updates the admin of one of the proxy contracts: the account with the rights to upgrade the implementations of `Comet` and `Configurator`.                                                                                                              | Timelock (DAO)                 |
| Proxy Admin                   | upgrade                               | Triggers the update of either a `Comet (Implementation)` contract or the `Configurator (Implementation)` contract. A malicious proposal could use this function to replace a Comet contract and steal funds.                                            | Timelock (DAO)                 |
| Proxy Admin                   | upgradeAndCall                        | Triggers the update of either the `Comet (Implementation)` contract or the `Configurator (Implementation)` contract and calls a function in the new contract. A malicious proposal could use this function to replace a Comet contract and steal funds. | Timelock (DAO)                 |
| Proxy Admin                   | renounceOwnership                     | Abandons ownership of the contract. The DAO would renounce the access to the administrative functions of the contracts, which includes upgrading the `Comet Implementation` and `Configurator Implementation` contracts.                                | Timelock (DAO)                 |
| Proxy Admin                   | transferOwnership                     | Updates the owner of the `ProxyAdmin` contract: the account with the rights to change the admin of the proxy and upgrade the implementation contracts.                                                                                                  | Timelock (DAO)                 |
| Proxy Admin                   | deployAndUpgradeTo                    | Deploy a new `Comet` using the `Configurator` and upgrade the implementation of the `Comet Proxy`.                                                                                                                                                      | Timelock (DAO)                 |
| Proxy Admin                   | deployUpgradeToAndCall                | Deploy a new `Comet` using the `Configurator` and upgrade the implementation of the `Comet Proxy`, then call a function in the new contract.                                                                                                            | Timelock (DAO)                 |
| Rewards                       | setRewardConfig                       | Set the reward token for a `Comet` instance.                                                                                                                                                                                                            | Timelock (DAO)                 |
| Rewards                       | withdrawToken                         | Withdraw reward tokens from the contract. This could drain all the tokens held on the contract to any output address if the DAO approves the proposal.                                                                                                  | Timelock (DAO)                 |
| Rewards                       | transferGovernor                      | Transfers the governor rights to a new address.                                                                                                                                                                                                         | Timelock (DAO)                 |
| CompoundGovernor (Proxy)      | upgradeToAndCall                      | Updates the implementation of the `Compound Governor` contract. Can only be triggered by the DAO itself.                                                                                                                                                | Timelock (DAO)                 |
| TimeLock                      | setDelay                              | Updates the delay to wait between when a proposal is accepted and when it is executed.                                                                                                                                                                  | Timelock (DAO)                 |
| TimeLock                      | setPendingAdmin                       | Updates the TimeLock's admin. This can be used if the Governor contract is updated.                                                                                                                                                                     | Timelock (DAO)                 |
| TimeLock                      | queueTransaction                      | Queues a transaction that can be executed once a delay. Current dela is 2 days. This can impact the own TimeLock 's settings (change admin, set delays) or interaction with any other contract the DAO has permissions on.                              | Governor (DAO)                 |
| TimeLock                      | cancelTransaction                     | Cancels a pending transaction and removes it from the queue. This allows the DAO to cancel one of its own decision before it is executed.                                                                                                               | Governor (DAO)                 |
| TimeLock                      | executeTransaction                    | Executes a transaction that was previously queued, if the corresponding delay has passed.                                                                                                                                                               | Governor (DAO)                 |

The permissions for all Comet contracts (USDC, WETH, wsETH, USDT, USDS) are similar and therefore only
represented once as `Comet Proxy` and `Comet Implementation`in the table above.

# Security Council

A security council called `Pause Guardian` has the power to pause all deposits, withdrawals, and transfers
in the protocol. The guardian is currently a 4/8 multisig made of Compound DAO community members. The signers are announced [here](https://www.comp.xyz/t/community-multisig-4-of-6-deployment/134/18). The same multisig is also `Proposal Guardian` and has the power to cancel Governance Proposal before their executions.

| Requirement                                             | Pause Guardian |
| ------------------------------------------------------- | -------------- |
| At least 7 signers                                      | ✅             |
| At least 51% threshold                                  | ❌             |
| At least 50% non-team signers                           | ✅             |
| Signers are publicly announced (with name or pseudonym) | ✅             |
