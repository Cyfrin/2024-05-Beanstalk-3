# Beanstalk Part 3 Codehawks

<p align="center">
<img src="https://res.cloudinary.com/droqoz7lg/image/upload/q_90/dpr_2.0/c_fill,g_auto,h_320,w_320/f_auto/v1/company/fsv4gpiuvkthl27oygeh?_a=BATAUVAA0" width="500" alt="Beanstalk">
</p>

# Contest Details

### Prize Pool

- Total Pool - $21,000
- H/M - $17,900
- Low - 1,000
- Community Judging - $2,100
- Starts: Monday, May 6, 2024 Noon UTC
- Ends: Monday, May 20, 2024 Noon UTC

### Stats

- nSLOC: 1,104
- Complexity Score: 818
- $/nSLOC: $19.02
- $/Complexity: $25.67

## About

Beanstalk is a permissionless fiat stablecoin protocol built on Ethereum. Its primary objective is to incentivize independent market participants to regularly cross the price of 1 Bean over its dollar peg in a sustainable fashion.

Beanstalk does not have any collateral requirements. Beanstalk uses credit instead of collateral to create Bean price stability relative to its value peg of $1. The practicality of using DeFi is currently limited by the lack of decentralized low-volatility assets with competitive carrying costs. Borrowing rates on USD stablecoins have historically been higher than borrowing rates on USD, even when supply increases rapidly. Non-competitive carrying costs are due to collateral requirements.

In particular, this audit is centered around the changes included in the Misc. Improvements BIP described in the PR comment [here](https://github.com/BeanstalkFarms/Beanstalk/pull/802). The diff from this PR may be helpful to review in order to understand exactly which code in this audit is new and unaudited.

You can read an overview of how Beanstalk works [here](https://docs.bean.money/almanac/introduction/how-beanstalk-works).

- [Docs](https://docs.bean.money/)
- [Whitepaper](https://bean.money/beanstalk.pdf)
- [Website](https://bean.money/)
- [Beanstalk Farms Twitter](https://twitter.com/BeanstalkFarms)
- [Beanstalk Public GitHub Repo](https://github.com/BeanstalkFarms/Beanstalk)

## Actors

- Stalkholder / Silo Member
  - Anyone who Deposits assets on the Deposit Whitelist into the Silo, earning the illiquid Stalk token in doing so. Stalkholders participate in governance and earn Bean seigniorage.
- `gm` caller
  - Anyone who calls the `gm` function to start the next Season.
- Unripe holder
  - Anyone who holds Unripe Beans or Unripe LP. These assets were distributed to holders of BDV (Bean Denominated Value) at the time of the April 2022 governance exploit. Most Unripe holders have their Unripe assets Deposited in the Silo, and thus are also Stalkholders.
- Fertilizer holder
  - Anyone who holds Fertilizer, the debt asset earned by participating in Beanstalk's recapitalization.
- Pod holder
  - Anyone who holds Pods, the Beanstalk-native debt asset. Pods are minting when lending Beans to Beanstalk (Sowing Beans).

## Scope

The following contracts are in scope.

```js
protocol/
└── contracts/
    ├── beanstalk/
    │   ├── barn/
    │   │   └── UnripeFacet.sol
    │   ├── silo/
    │   │   └── ConvertFacet.sol
    │   └── sun/
    │       └── SeasonFacet/
    │           └── Sun.sol
    └── libraries/
        ├── Convert/
        │   ├── LibChopConvert.sol
        │   ├── LibConvert.sol
        │   ├── LibConvertData.sol
        │   └── LibLambdaConvert.sol
        ├── LibChop.sol
        ├── LibFertilizer.sol
        ├── LibStrings.sol
        ├── LibUnripe.sol
        └── Minting/
            └── LibWellMinting.sol
```

## Compatibilities

Beanstalk implements the [ERC-2535 Diamond standard](https://docs.bean.money/developers/overview/eip-2535-diamond). It supports various whitelists for [Deposits](https://docs.bean.money/almanac/farm/silo#deposit-whitelist), [Minting](https://docs.bean.money/almanac/farm/sun#minting-whitelist), [Converts](https://docs.bean.money/almanac/peg-maintenance/convert#convert-whitelist), etc., particularly for LP tokens from [Basin](https://basin.exchange/).

Blockchains:

- Ethereum

Tokens:

- ERC-20 (all are accepted in Farm balances, a whitelist is accepted on the Deposit Whitelist, etc.)
- ERC-1155 (Fertilizer and Deposits are ERC-1155 tokens)

## Setup

Clone repo:

```bash
git clone https://github.com/Cyfrin/2024-05-Beanstalk-3
```

Install dependencies:

```bash
cd 2024-05-Beanstalk-3/protocol
yarn
```

Add RPC:

```bash
export FORKING_RPC=https://eth-mainnet.g.alchemy.com/v2/{RPC_KEY}
```

generate:

```bash
yarn generate
```

Test:

```bash
yarn test
```

## Known Issues

All findings in the following resources are considered known issues:

- All Beanstalk audit reports listed in this [repository](https://github.com/BeanstalkFarms/Beanstalk-Audits);
- All bug reports from the Immunefi program listed [here](https://community.bean.money/bug-reports);
- All reports and known issues in past Beanstalk Codehawks audits:
  - [Beanstalk Codehawks Part 1](https://www.codehawks.com/report/clsxlpte900074r5et7x6kh96); and
  - [Beanstalk Codehawks Part 2](https://www.codehawks.com/contests/clu7665bs0001fmt5yahc8tyh).
