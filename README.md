<h1 align="center">
   <b>
        <a href="https://eddalytics.vercel.app/"><img src="https://github.com/SabeloMkhwanzi/evmos-defi-dashlytics/blob/main/public/eddalytics-logo4.png" /></a><br>
    </b>
</h1>


<p align="center">
    <a href="https://eddalytics.vercel.app/"><b>Website</b></a> •
    <a href="https://github.com/SabeloMkhwanzi/evmos-defi-dashlytics/blob/main/README.md"><b>Documentation</b></a>
</p> 

# EddAlytics - Evmos Dex Dashboard Analytics

Providing a visual analytics front-end with rich information about the health and performance of Dex on Evmos ecosystem - Powered by Evmos x Covalent. 

### Intro
- Web3 has changed the way we can interact with money by introducing Decentralized finance (DEFI). Decentralized finance provides users with a range of financial services similar to traditional financial industries such as banking, borrowing, insurance, and trading without the need to rely on centralized entities. (DEX) decentralized exchange is one of the popular financial services emerging on the web3 ecosystem, a DEX is an exchange where users trade directly from their own wallets, where you can pay money from one currency to buy money for another.  

- DEX uses Automated Market Makers (AMMs), that allows users to swap tokens on DEX, by paying money from one currency to buy money for another eg. using US dollars stable coin (USDC) to buy Ethereum tokens (ETH). AMMs are protocols for DEXs that help give you the price between these two assets when you swap tokens. AMMs usually rely on a mathematical formula to price assets. Currently, there are different formulas that AMMs use. However, the most fundamental and most well-known formula is the XY=K equation. XY=K is an equation you use for determining how much of one token, called X, is needed to swap for another token, called Y, in any DEX.

### Project mission 
- For any (DEX) decentralized exchange protocol runing on XY=K engine to be successful, it needs a visual analytics front-end dashboard to provide users with rich information about the health and performance of that ecosystem. Emvos is a fast growing ecosystem with (DEX) decentralized exchange built on the ecosystem, thus we approach to solve the problem by providing  [EddAlytics](https://eddalytics.vercel.app/) - Evmos Dex Dashboard Analytics.

### Project Solution 
- Introducing [Covalent](https://www.covalenthq.com/), they built XY=K: the suite of Class B, generalized UniswapV2-like endpoints in their unified API for exchange on various blockchains. They introduced these endpoints to provide you with the richest and most robust on-chain DEX data and allow you to retrieve all the required data for any UniswapV2-like protocol (such as SpiritSwap and SushiSwap) on a variety of blockchains, simply by providing the name of the exchange and the correct chainID for the blockchain. As a result, these endpoints enable you to access any DEX-related protocol in a matter of minutes - Awesome right! 😊

### Project Stack
- Building Framework: [Nextjs](https://nextjs.org/) -Next.js is an open-source web development framework created by Vercel enabling React-based web applications with server-side rendering and generating static websites
- Frontend: 
  - [1. Mantine](https://mantine.dev/) - Build fully functional accessible web applications faster than ever
  - [2. Chakra-ui](https://chakra-ui.com/) - Create accessible React apps with speed
- [React-Query](https://react-query-v3.tanstack.com/) - Fetch, cache and update data in your React and React Native applications all without touching any "global state".
- [Recharts](https://recharts.org/en-US/) - A composable charting library built on React components

- Backend [Covalent](https://www.covalenthq.com/) - One unified API, One billion possibilities, Covalent provides a unified API bringing visibility to billions of Web3 data points.
- Hosting Platform: [Vercel Platform](https://vercel.com/new?utm_medium=default-template&filter=next.js&utm_source=create-next-app&utm_campaign=create-next-app-readme) from the creators of Next.js.

## Table of Contents
- [Installing](#installing)
- [How to use](#how-to-use)
  - [Get information about chain statuses](#get-information-about-chain-statuses)
  - [Get 30d volume and liquidity as a timeseries chart](#Get-30d-volume-and-liquidity-as-a-timeseries-chart)
  - [Calculating amount of tokens to send](#calculating-amount-of-tokens-to-send)
  - [Getting the amount of gas fee](#getting-the-amount-of-gas-fee)
  - [Getting the average transfer time](#getting-the-average-transfer-time)
- [Semver](#semver)

## Installing

1. clone the repo with the following git command:

```bash
git clone https://github.com/SabeloMkhwanzi/Gigiblock-social-hour-web3
```

2. open a terminal in the root directory of the project and run:

```bash
npm install
```

to install all the package dependencies for the project

Create a .env.local file in the root folder and populate it with the following variables, Get Api a key from [Covalent](https://www.covalenthq.com/)

```bash
NEXT_PUBLIC_COVALENTKEY=
```


Finally, run the development server:

```bash
npm run dev
```

## How to use 

### Get information about chain statuse

API method `GET/v1/chains/status/` can be used to get information about chain statuse and blocked Signed at.
 - note: `const chainStatus = data?.data?.items[25].synced_block_height` at `[25]` we the get the chain id of Evmos in array of objects 😊

```js
const APIKey = process.env.NEXT_PUBLIC_COVALENTKEY;

export default function ChainStatus() {
  // used React-Query to fetch Covalent API
  const { data, error, isFetching } = useQuery(["chainStatus"], async () => {
    const res = await fetch(
      `https://api.covalenthq.com/v1/chains/status/?key=${APIKey}`
    );
    return res.json();
  });
  const chainStatus = data?.data?.items[25].synced_block_height;
  const blockedSignedAt = data?.data?.items[25].synced_blocked_signed_at;
  
     if (isFetching)
    return "Loading..."

  if (error) "error + messaage"
    
}


```

### Get 30d volume and liquidity as a timeseries chart

API method `/v1/9001/xy=k/cronus/ecosystem/?&key=${APIKey}`can be used to get information on volume 7-30d & liquidity 7-30d
- notes: we simply by providing the name of the exchange `/cronus/` and the correct chainID `/9001/` for the blockchain, lastly your Api key from [Covalent](https://www.covalenthq.com/) you provided on .env.local as a variables `/?&key=${APIKey}`  As a result, these endpoints enable you to access any DEX-related protocol in a matter of minutes, cool hey 😊

```js
const APIKey = process.env.NEXT_PUBLIC_COVALENTKEY;

export default function CronusOverview() {
  // used React-Query to fetch Covalent API
  const { data, error, isFetching } = useQuery(["cronusEco"], async () => {
    const res = await fetch(
      `https://api.covalenthq.com/v1/9001/xy=k/cronus/ecosystem/?&key=${APIKey}`
    );
    return res.json();
  });

  // Chart data for Evmos liquidity_chart_30d
  const CronusLiquidity = data?.data?.items[0].liquidity_chart_30d.map(
    (item) => ({
      X: moment(item.dt).format("MMM Do"),
      Y: item.liquidity_quote,
    })
  );

// Chart data for Evmos volume_chart_30d
  const CronusVolume = data?.data?.items[0].volume_chart_30d.map((item) => ({
    X: moment(item.dt).format("MMM Do"),
    Y: item.volume_quote,
  }));
  
   if (isFetching)
    return "Loading..."

  if (error) "error + messaage"
     
  }
```

### Calculating amount of tokens to send

SDK method `getAmountToSend` can be used to calculate the amount of tokens to send based on the required amount of
tokens the receiving party should get.

```js
const amountToSend = await sdk.getAmountToSend(
  amountToBeReceived,
  sourceTokenInfo,
  destinationTokenInfo
);
```

### Getting the amount of gas fee

SDK method `getTxCost` can be used to fetch information about the amount of gas fee required to complete the transfer on
the destination chain. Gas fee is paid during the [send](#32-send-tokens) operation in the source chain currency.

```js
const weiValue = await sdk.getTxCost(
  usdtOnEthTokenInfo, // from ETH
  usdtOnTrxTokenInfo, // to TRX
  Messenger.ALLBRIDGE
);
```

### Getting the average transfer time

SDK method `getAverageTransferTime` can be used to get the average time in ms it takes to complete a transfer for a
given combination of tokens and messenger.

```js
const transferTimeMs = sdk.getAverageTransferTime(
  sourceTokenInfo,
  destinationTokenInfo,
  Messenger.ALLBRIDGE
);
```

## Semver

Until bridge-core-sdk reaches a `1.0.0` release, breaking changes will be released with a new minor version. For
example `0.3.1`, and `0.3.4` will have the same API, but `0.4.0` will have breaking changes.



# Evmos Dex Dashborad Analytics
