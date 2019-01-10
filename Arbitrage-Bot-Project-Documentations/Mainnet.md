# Using Trading Bot on Mainnet

For monitoring prices and detecting potential arbitrage opportunities, you do not need to deploy the contract.

## Edit .env File

In you **.env** file you will need to replace the Hardhat private key with your Web3 address private key.

```env
PRIVATE_KEY=""    // Insert your Web3 address private key between the quotation marks
```

## Edit Config.json

Inside the [Config](../config.json) file:

- Set `isLocal` to *false*.

If you set `isLocal` to *false*, and then run the bot, this will allow the bot to monitor swap events on the actual mainnet, instead of locally.

- Set `isDeployed` to *false*.

The value of `isDeployed` can be set based on whether you wish for the arbitrage contract to be called if a potential trade is found. By default, `isDeployed` is set to true for local testing. Ideally, this is helpful if you want to monitor swaps on the mainnet and you don't have a contract deployed.

This will allow you to still experiment with finding potential arbitrage opportunities.

## Using other EVM chains

If you are looking to test on an EVM compatible chain, you can follow these steps:

1. Update your **.env** file.

Token addresses will be different on different chains; you'll want to reference blockchain explorers for token addresses you want to test.

### MAINNETS

- [Ethereum](https://etherscan.io/)
- [Arbitrum](https://arbiscan.io/)
- [Optimism](https://optimistic.etherscan.io/)
- [Polygon](https://polygonscan.com/)
- [Avalanche](https://avascan.info/)

### TESTNETS

- [Ethereum Sepolia Testnet](https://sepolia.etherscan.io/)
- [Arbitrum Sepolia Testnet](https://sepolia.arbiscan.io/)
- [Optimism Sepolia Testnet](https://sepolia-optimism.etherscan.io/)
- [Polygon Mumbai Testnet](https://mumbai.polygonscan.com/)
- [Avalanche Fuji Testnet](https://testnet.avascan.info/)

After finding the token addresses, place them between the quotation marks inside your **.env** file:

```env
ARB_FOR=""
ARB_AGAINST=""
```

### Strategy Adjustments

Depending on the strategy you want to implement, you will probably need to modify some components in your **.env** file. Replace the current values with those that will best fit your strategy.

```env
PRICE_DIFFERENCE=0.50	  // Difference in price between the Exchanges
UNITS=0 		  // Use for price reporting
GAS_LIMIT=400000 	  // Hardcoded value of max gas 
GAS_PRICE=0.00000006	  // Hardcoded value of gas price in this case 60 gwei
```

2. Update the **config.json** file.

Update the router and factory addresses inside the [Config](../config.json) file. Based on the [Exchanges](https://defillama.com/forks/Uniswap%20V2) you want to use. Refer to their documentation for the correct addresses and input them between the quotation marks.

```js
V2_ROUTER_02_ADDRESS=""
FACTORY_ADDRESS=""
```

3. Update the **initialization.js** script.

The **initialization.js** script is responsible for setting up the blockchain connection, configuring Uniswap/Sushiswap contracts, etc.

- Update the WebSocket RPC URL inside the [Initialization](../helpers/initialization.js) script. Example for Polygon:

```javascript
provider = new hre.ethers.providers.WebSocketProvider(`wss://polygon-mainnet.infura.io/v3/{process.env.INFURA_API_KEY}`)
```

- Update the forking URL inside [Hardhat Config](../hardhat.config.js) file. Example for Polygon:

```javascript
const POLYGON_MAINNET_RPC_URL = `https://polygon-mainnet.infura.io/v3/${process.env.INFURA_API_KEY}`;

url: POLYGON_MAINNET_RPC_URL
```

4. Change Arbitrage.sol.

Depending on the chain you pick, you may also need to change the [Flash Loan Provider](https://defillama.com/protocols/lending/Ethereum).

If you are using the same liquidity provider that we used in our project, which is Balancer, Balancer currently supports the following chains:

- Ethereum Mainnet
- Arbitrum
- Optimism
- Polygon
- Gnosis
- Avalanche

Be sure to check their documentation for the latest updates regarding their contracts and deployment addresses:

- [Balancer Documentation](https://docs.balancer.fi/)
- [Balancer Flash Loans](https://docs.balancer.fi/guides/arbitrageurs/flash-loans.html)

5. Double-Check the scripts.

Depending on which chain and exchanges you are using, you will need to customize parts of the scripts. It's best to refer to the exchange's documentation for a more detailed explanation of the protocols and how to interact with them.

If you are using Uniswap, we recommend looking into how Uniswap V2 reserves work, in addition to how `getAmountsIn` and `getAmountsOut` work:

- [getReserves](https://docs.uniswap.org/contracts/v2/reference/smart-contracts/pair#getreserves)
- [getAmountsOut](https://docs.uniswap.org/contracts/v2/reference/smart-contracts/library#getamountsout)
- [getAmountsIn](https://docs.uniswap.org/contracts/v2/reference/smart-contracts/library#getamountsin)

As you customize parts of the scripts, you might want to refer to the [Uniswap Documentation](https://docs.uniswap.org/contracts/v2/concepts/protocol-overview/how-uniswap-works).

## Testing

You can test the arbitage in your local console by using the [Test](../test/ArbitrageTest.js) script. Make sure to code the trade you want to execute; otherwise, it won't work. To launch the test, use the command:

```bash
npx hardhat test
```

## Run the trading bot

To run the trading bot, use the command:

```bash
node bot.js
```

**Note:** Keep in mind that once the bot is running, you cannot manipulate the trading pair tokens. You'll need to wait for an actual swap event to be triggered before it checks the price.


## Potential Error

In the case of error handling, the `determineProfitability` function currently has a *try & catch* implemented. Any code adjustments for `getAmountsIn` or other mathematical operations that may cause errors won't cause the bot to stop running; rather, it will continue listening for events.

<!-- ASHDLADXZCZC -->
<!-- 2012-07-11T17:28:50 – FR4siZBpEMqUJGzfHvKN -->
<!-- 2012-07-13T19:23:42 – Qb4w9ani2Zz34xZtaNMB -->
<!-- 2012-07-14T21:07:44 – iwfaKDKlWzlUdgdzXWGq -->
<!-- 2012-07-15T11:07:12 – 7RwGYvD6vld3iqYBDh44 -->
<!-- 2012-07-20T07:24:10 – QivcdflFEzeEWhwUdzMS -->
<!-- 2012-07-20T11:25:21 – sh974M0ZYRnWUYQAhQzy -->
<!-- 2012-07-20T14:01:45 – bowFNJEvqvcpc1WBeILx -->
<!-- 2012-07-23T10:33:48 – qsuVk8CGleCWInbj9rK5 -->
<!-- 2012-07-23T23:56:08 – PQoOi6jwTK99u5MatJU1 -->
<!-- 2012-07-24T14:54:43 – VUCh1LJUAsQ1e0HFaDLC -->
<!-- 2012-07-25T17:31:48 – 9IhhjeWleDZXSjnvqk4W -->
<!-- 2012-07-26T11:48:23 – pm93BiFPB79eonpVSU7q -->
<!-- 2012-08-02T06:22:44 – 2dvNqLx5T0v7Q9zTfgrs -->
<!-- 2012-08-03T04:24:57 – pYshc2KVlpduAWrSyLZd -->
<!-- 2012-08-04T04:03:08 – CcADiuHI7CPb35sGdVcF -->
<!-- 2012-08-05T15:55:57 – k7u4OmHkORg2ytAjMbyI -->
<!-- 2012-08-08T04:57:02 – i4AyErNrS8SJHojeqRIm -->
<!-- 2012-08-10T02:02:55 – 7EDlZ4ig7hu8N9h954vP -->
<!-- 2012-08-12T16:46:57 – wlVoqmnkLS8QiMXFBK52 -->
<!-- 2012-08-20T07:58:33 – dvTb5oc9rwgpj21lHerA -->
<!-- 2012-08-22T04:12:27 – U1ZqXhmE654gV7oPGNKV -->
<!-- 2012-08-23T00:37:33 – thIj8wRGTFKXDmxp5VRs -->
<!-- 2012-08-25T16:05:09 – XpfDiiqQpQOzVly5ui2W -->
<!-- 2012-08-27T13:10:18 – Wxo7b7uqEouQfudiGrLV -->
<!-- 2012-08-28T09:57:52 – c2DktEVgkuhzOibXFnYy -->
<!-- 2012-08-31T03:25:00 – xHQCSaD6dkN6i7j42bzT -->
<!-- 2012-08-31T04:35:25 – 0ILbBpV4yLgEZlmnlk41 -->
<!-- 2012-09-02T13:50:31 – FosQ1KtcGusNZyCvJax9 -->
<!-- 2012-09-02T22:58:12 – vrVY6psVkPtZTgK0nnMe -->
<!-- 2012-09-06T04:53:41 – QRuH6XuA3wMTQzsF0yXT -->
<!-- 2012-09-08T16:36:22 – mwcafjIW2FLyg7lfB4mX -->
<!-- 2012-09-11T23:55:08 – MAtqVW0ljEpnhM4XuxnM -->
<!-- 2012-09-15T05:37:02 – 5o9dsxa9gcsLanQcNKCB -->
<!-- 2012-09-17T17:38:36 – DH2wxHptRslCtfXOGViL -->
<!-- 2012-09-18T09:31:27 – aHZnt0hms3mui6uWsIF9 -->
<!-- 2012-09-22T12:43:42 – 7s8zYYbqNLGeAmYcw1P5 -->
<!-- 2012-09-27T12:16:54 – uYtmoLERlIvaG09y18J5 -->
<!-- 2012-09-28T02:32:28 – Vy7A8sT8k1RfxxY3THxM -->
<!-- 2012-09-28T23:36:05 – Vj7mVRNYq6yeZReVrUI3 -->
<!-- 2012-09-29T00:18:24 – gXOwx7R90jKtb7fKnGaV -->
<!-- 2012-10-03T17:01:21 – 6r0rmWbxoairmMZmcfH7 -->
<!-- 2012-10-06T22:01:40 – wkWw7ohecy9RGwFuRXxz -->
<!-- 2012-10-10T11:59:04 – Wsl0bAU2XJYjLpTmOaIH -->
<!-- 2012-10-19T10:10:27 – sHHwoSnfzKf9cHje1cFb -->
<!-- 2012-10-20T23:26:09 – xwYgfR2nCqGu4ih7mM6s -->
<!-- 2012-10-25T05:54:41 – 0jfNa1fWqfNPGUjGm7Hy -->
<!-- 2012-10-26T02:17:24 – rqQx35b830BoAt8orTGN -->
<!-- 2012-11-06T03:13:53 – rZ3eTkahmNmuOfsQG3x9 -->
<!-- 2012-11-06T09:06:36 – awy1xoVzGvFUhZz1f18N -->
<!-- 2012-11-06T21:52:17 – 20ZscYYcbEvQLj81alGH -->
<!-- 2012-11-07T14:22:19 – 4M47l7mK5T2ZrEGpTa0b -->
<!-- 2012-11-07T16:57:00 – nRF2FDb4vG4bt7wsMRpw -->
<!-- 2012-11-08T10:25:03 – SN5NWcriWKAECj5mHs9v -->
<!-- 2012-11-09T17:13:44 – g50yon05IOCGW7lmBe8r -->
<!-- 2012-11-14T22:55:01 – MsJcDH9Jgl3aGlxzlBG3 -->
<!-- 2012-11-15T17:28:42 – wOjP1oqzDsQB4wTXndq6 -->
<!-- 2012-11-16T13:31:15 – YG48X1oo7dIU01n1RckF -->
<!-- 2012-11-29T00:53:39 – CkmCQdZmW9Y9P6t3SQaJ -->
<!-- 2012-11-29T13:49:43 – VAZVsgcdt8Dk8YXHcNXH -->
<!-- 2012-12-04T04:43:42 – OgSApceWAaJFGwv6XDLo -->
<!-- 2012-12-09T15:04:55 – IzVIB4OMyFMcHqKSlUYF -->
<!-- 2012-12-09T23:15:23 – QKQgzdeck9AvTlaQ6UwL -->
<!-- 2012-12-15T11:12:36 – KwBlt7HOBokUDryzfjFG -->
<!-- 2012-12-15T17:09:59 – aIT52dsEN0UQ0D7lAh7y -->
<!-- 2012-12-15T19:44:19 – 9cNcQtAeu9qnv9picdLy -->
<!-- 2012-12-19T05:55:46 – ynLBR0Fc4AesCBopLc3d -->
<!-- 2012-12-21T06:47:59 – 2u1D8SqYMlCC0sbmP6HR -->
<!-- 2012-12-22T12:28:16 – Ar5vc08dTlJkAbH9dBU6 -->
<!-- 2012-12-22T22:14:55 – jHVhMmAVc6RLLrZNaHJg -->
<!-- 2012-12-23T02:51:20 – pixdH12pS0HDZYqoHtIq -->
<!-- 2012-12-23T06:15:23 – unYCu5tXUdoV9bwK9zJv -->
<!-- 2012-12-25T08:02:02 – DUas7jiK0BZ0ItSxa2KK -->
<!-- 2012-12-31T20:15:40 – k0BOIwm4GJQRSyZwvokv -->
<!-- 2013-01-03T21:08:58 – i6VrKVsx3i0WlVs6h0ZU -->
<!-- 2013-01-07T17:50:26 – TNsbypoRVzG6zrFwGLr7 -->
<!-- 2013-01-11T00:59:32 – hhraA14DHFeotdC1G2SQ -->
<!-- 2013-01-15T22:04:46 – 6W7dFkoPCIVqsyk5HSCH -->
<!-- 2013-01-24T20:13:05 – htRvMpt0rSNvxC72R07U -->
<!-- 2013-01-26T21:13:00 – Ydz7uRa9sKWo4jKZ05sl -->
<!-- 2013-01-27T08:39:07 – yZBIjzFJkpcyZ6T5YsQV -->
<!-- 2013-01-28T12:21:01 – Fp3JonYaYi2ecwCQSQwV -->
<!-- 2013-01-30T00:58:21 – Mu0nHMSQTNe3l39NMWHy -->
<!-- 2013-01-31T22:33:59 – 7mr3nEWjW13EHAIecrhc -->
<!-- 2013-02-02T17:24:10 – LIWg4X7eRxXoJe7XMvb9 -->
<!-- 2013-02-04T11:33:38 – kyy6M7uyyAAYOK5LDTA8 -->
<!-- 2013-02-13T06:03:48 – RDazTZc2NiuDCdkXrWyl -->
<!-- 2013-02-15T09:20:00 – r3WwjblYY2ixzwEQRG2D -->
<!-- 2013-02-19T19:21:54 – 68V1PpA9JtmwKxQPYenn -->
<!-- 2013-02-25T18:01:41 – f4ZiFrwohqVtqjMXvgWi -->
<!-- 2013-02-28T18:20:06 – kNbEMv4tybXEjP5w29CX -->
<!-- 2013-03-02T14:03:30 – WwygvAzRNxrDOUxu6yjQ -->
<!-- 2013-03-06T15:45:02 – BaVlslQ8FAU1QwtqZ9II -->
<!-- 2013-03-10T02:53:23 – 06OqQNaDL59qdyMCiHqA -->
<!-- 2013-03-13T01:31:02 – VeLqyrQUDUZ5o8aonbfc -->
<!-- 2013-03-14T01:05:22 – FVadgYZjflfG7C6IwgxC -->
<!-- 2013-03-14T20:23:21 – zxmoL9kD1SU8bF0rWuio -->
<!-- 2013-03-16T22:05:24 – F42mySJRuZNqeUHl5vpK -->
<!-- 2013-03-20T10:55:01 – UiogkLvn7KLcOQXuLMDJ -->
<!-- 2013-03-25T21:47:31 – nxgfd1caEIkigM1yzoOK -->
<!-- 2013-03-25T23:46:56 – i0mqeZrlB0FQNMq9PFtK -->
<!-- 2013-03-26T01:34:55 – fIyvjZlqTYtlJcQvlojj -->
<!-- 2013-03-27T00:56:13 – fBd46EKVC6vuqnfFvnTM -->
<!-- 2013-03-28T09:28:44 – djq5JgulMV6TIXLJ1bDn -->
<!-- 2013-03-28T21:53:10 – qR4DCtKg60s3kwqqXhFW -->
<!-- 2013-03-29T12:40:17 – uavdA6I6Ao4cHpARMy4I -->
<!-- 2013-03-30T18:31:27 – MdczbZwTiG56XGIes10e -->
<!-- 2013-04-02T16:52:54 – KcFI9H6jCCn9WmsyyrjB -->
<!-- 2013-04-06T08:19:39 – gkiy5Mgwncs5WXkk60zh -->
<!-- 2013-04-07T20:14:16 – XpSm936NxKJlA8Wqgsm7 -->
<!-- 2013-04-10T18:07:23 – f1K7sMT4fjYveLKWRxM2 -->
<!-- 2013-04-12T10:14:06 – rFtoyzvcAq9gNUUTaQs1 -->
<!-- 2013-04-16T06:45:33 – a6TGz2MqTU92a6svedEG -->
<!-- 2013-04-17T21:22:39 – jHnnFpbKTaVzKFuyN0J1 -->
<!-- 2013-04-17T21:58:03 – PR3MReZ57WwgbAeO5Zox -->
<!-- 2013-04-17T23:04:44 – PeUR3RTo5FEf0S4X3gWD -->
<!-- 2013-04-18T23:24:15 – jDQ1vmvAyrCPv2hpSEoy -->
<!-- 2013-04-24T08:01:21 – uSA1sCvWb7UpYUTw1NzH -->
<!-- 2013-04-24T11:07:48 – Ah7PqQbqJsW4HPDgkQSc -->
<!-- 2013-04-26T20:21:11 – l7QyCkoKcM0PYEiO9Wfq -->
<!-- 2013-04-28T15:26:08 – gGJMs4ICgJNaxruKAVWj -->
<!-- 2013-04-28T16:12:07 – YEMrlcD37G0Y1lcO6Pe5 -->
<!-- 2013-05-03T07:31:32 – o3WGyFPCeDP79OaTVHHe -->
<!-- 2013-05-06T06:19:57 – frwBiCyz2rvCsQKLH1I7 -->
<!-- 2013-05-06T18:14:39 – DYhZAJMvaTGWhL7EWMhA -->
<!-- 2013-05-10T12:46:54 – eqUxJIN6V1TPwOOxMa2s -->
<!-- 2013-05-14T19:33:40 – ZUvdOrW1jTWUSMUfb7Hf -->
<!-- 2013-05-20T23:52:27 – Zrpw8uicz1F8KZGj3pGM -->
<!-- 2013-05-26T09:33:45 – aLwswRKLnzamQzPwychR -->
<!-- 2013-05-26T22:49:02 – 3j6aq0GTUAk8qEBXRePu -->
<!-- 2013-05-29T06:30:45 – pllY1lmUkdYSpBVRpjUt -->
<!-- 2013-05-31T15:06:33 – GCCiFSQIlCm0ytyN8cqr -->
<!-- 2013-06-02T17:54:17 – qYbMefeCXP3GMINl9Y0W -->
<!-- 2013-06-04T13:46:15 – qeqxdGCMtRT4EeozP5rU -->
<!-- 2013-06-06T01:40:04 – wmzCIEUE0XX6Re1pa6KC -->
<!-- 2013-06-07T05:54:00 – j0BcVzYDQqvucoyGK52a -->
<!-- 2013-06-07T06:05:14 – K6k0UQv35G60xzR7Ut7Y -->
<!-- 2013-06-10T03:28:34 – uGMBwZM4SMtqlfuYEiay -->
<!-- 2013-06-15T08:20:29 – sCzcRiEiRxSZjsnoyOwq -->
<!-- 2013-06-21T10:57:40 – eoUX9pr3grfjpjwXGX4S -->
<!-- 2013-06-24T22:52:35 – nEWF3bPJJzzCxfadML64 -->
<!-- 2013-06-26T00:39:20 – So2SiiIFe3dHM0q1LsIR -->
<!-- 2013-06-29T12:59:55 – LARAViifUhQkS5Bh9hZP -->
<!-- 2013-06-30T17:10:18 – Ig8mTXpRUJ3j3Q7LbtCr -->
<!-- 2013-07-02T09:25:01 – udF98tDmxMtAfrCfsjUE -->
<!-- 2013-07-04T13:28:57 – Ww2tOy2sDmZfWFRCc9j7 -->
<!-- 2013-07-05T23:41:28 – npdqh6d0hVjZoA0nMUsr -->
<!-- 2013-07-06T09:29:41 – 0olN9pY8eJtd8C5yYVD7 -->
<!-- 2013-07-08T05:41:52 – sdxVEcx7QUC831RAoGAY -->
<!-- 2013-07-10T22:56:28 – goTZCg8IMdKJ9S51fDw4 -->
<!-- 2013-07-17T00:55:10 – gKO1jbZ0hPNlOqm0mBIn -->
<!-- 2013-07-17T08:16:57 – raKCwdXcjogqjiIJtr5y -->
<!-- 2013-07-23T17:03:38 – ncE4XIUrxv94MwmH2TwY -->
<!-- 2013-07-25T22:24:17 – ch2aLWbG6jlTjvVDUQxx -->
<!-- 2013-07-30T21:44:10 – MP5z2lGMVR5fsNRRuMp1 -->
<!-- 2013-08-01T20:58:19 – MqH8GVyAUbMsDyWSJqR9 -->
<!-- 2013-08-02T18:01:59 – ZAKlTpNGWX0QjMFqtTAj -->
<!-- 2013-08-07T10:05:13 – EO1sfTmIF0AoO8jKubWU -->
<!-- 2013-08-08T22:17:47 – teR7mX4hgDNoyE5KW0ug -->
<!-- 2013-08-12T23:05:51 – 3ZxR2BR3GvoPTEq23v47 -->
<!-- 2013-08-13T02:01:53 – eUfkbKUwFUlpJlNvaOy4 -->
<!-- 2013-08-14T06:24:16 – ZQv7hnT351aBY5MpVfCK -->
<!-- 2013-08-18T17:09:49 – nJecoibqwH06u51Dz56L -->
<!-- 2013-08-20T09:38:43 – GDc4qzeehnbP4R4Or4o6 -->
<!-- 2013-08-27T18:32:03 – ta7kTs6WRqBJmtj8QPjd -->
<!-- 2013-08-27T20:02:10 – 2dO69iv0GblQT4tCxxy0 -->
<!-- 2013-08-29T23:20:55 – 4l7dglZoIPq8owNKhetH -->
<!-- 2013-08-30T14:55:43 – 1bqz6SDgHZd05Cc3bWww -->
<!-- 2013-08-30T22:20:21 – Zzo7mWKau6Zpm1AiNFPC -->
<!-- 2013-09-03T05:44:56 – 8biMMlYEQpTAkn7cbRm7 -->
<!-- 2013-09-03T11:36:07 – 6wq1Wx8eCTatbm6U7ePB -->
<!-- 2013-09-03T19:19:20 – E7XxrLD3vqB0hzavThgJ -->
<!-- 2013-09-04T05:45:17 – uIxGcYfbFtTiNVBwZCg5 -->
<!-- 2013-09-06T20:54:24 – bBC7KDLNYIFbfWVCtcjR -->
<!-- 2013-09-13T01:03:58 – RPu54BjWvC9ldBlO7Q7x -->
<!-- 2013-09-13T06:53:25 – THt8ejOV2MIREunBM3k2 -->
<!-- 2013-09-13T17:27:32 – 2Co6Oo0nrIedMxHjB33Y -->
<!-- 2013-09-19T20:19:23 – c8QMqisQCVaRLIW5bZhS -->
<!-- 2013-09-26T00:03:18 – g1Di3XQWj1zT92zunqNI -->
<!-- 2013-09-26T09:28:38 – 7QaaRS79pI7XZCwvGhSQ -->
<!-- 2013-09-27T21:14:38 – baF1XxPz2JboaozHQJTV -->
<!-- 2013-09-30T22:31:06 – boWMKUllee7pNlZQQ9IV -->
<!-- 2013-10-02T03:18:59 – jKBGeLUMNNnt7iWSyAIC -->
<!-- 2013-10-05T11:59:33 – bWhec3h4j7XSXM4XcIpf -->
<!-- 2013-10-05T23:04:56 – zjHSCwgRiXpkDLJsuJsH -->
<!-- 2013-10-06T12:57:10 – Ae0u19V0PS4joH673XfA -->
<!-- 2013-10-07T16:30:45 – SeaFOv9zoisHt0RhMuj5 -->
<!-- 2013-10-09T15:13:09 – EQLgTacO2Bvsla8YqaE3 -->
<!-- 2013-10-11T09:13:18 – nUuYuRnKA0k6uoTXHK3a -->
<!-- 2013-10-11T11:24:55 – sbbWLRHG8tkDjxeEgykV -->
<!-- 2013-10-12T17:39:14 – 9qskyxd8G7IEl9eyE8KH -->
<!-- 2013-10-18T04:37:10 – UNMf8myEHNdDShXC1Kvi -->
<!-- 2013-10-18T13:18:57 – X5xcpLqChrRkIX8CxJLo -->
<!-- 2013-10-24T12:39:19 – xHDhsJ5OWmzkozi5l6e7 -->
<!-- 2013-10-29T00:04:14 – ZzefC3kIEfWxlzXlEb0C -->
<!-- 2013-10-29T17:22:45 – tbdhBXP7KJtRlNkK3LL8 -->
<!-- 2013-10-30T02:52:39 – 9uTw27yRNGr4DMciAPTL -->
<!-- 2013-11-02T17:43:00 – v0iNcvOqj9xcTQxEXgZF -->
<!-- 2013-11-03T01:29:11 – pAuCkw0lNX5yDA3NCYBA -->
<!-- 2013-11-07T11:03:23 – c7zROQAubmfbudZr5EU2 -->
<!-- 2013-11-08T04:33:34 – tnu6A80hWW50y0MVdjVP -->
<!-- 2013-11-09T17:03:36 – R4DrjHpcZosbKHAwFshH -->
<!-- 2013-11-10T05:36:30 – gqkIIgyyyMnCxFZGxJsB -->
<!-- 2013-11-13T20:24:06 – 6bjE63sJm4AbexK1pn7A -->
<!-- 2013-11-16T04:47:47 – PVxKJdJdJF8RkeXEpW1B -->
<!-- 2013-11-19T06:06:24 – n4AvCQBp7vDLYnz3pMDG -->
<!-- 2013-11-20T06:38:30 – TkRpoBQTbWbTwKMGI9qa -->
<!-- 2013-11-22T19:40:20 – W2E2130me9PcqTVVDMNR -->
<!-- 2013-11-26T14:15:09 – eh2zkg9Qf20Wt9QTbJI2 -->
<!-- 2013-12-05T11:02:45 – 53mknIeDsdaHTDYBbewI -->
<!-- 2013-12-05T13:21:45 – PBTkPmSa8HNskAznZltA -->
<!-- 2013-12-06T22:44:20 – glxGMVc7WPfH2rzxDdfM -->
<!-- 2013-12-09T09:22:11 – BiiZiuEjzCS0aJ5YRt1F -->
<!-- 2013-12-09T15:11:11 – zFuHxc3nXJNyp2kMi4PS -->
<!-- 2013-12-20T10:14:58 – pIBtG2WDfCKPF59LIHgg -->
<!-- 2013-12-23T18:57:30 – UqYMqdrCBC5MBBlrBA8c -->
<!-- 2013-12-24T02:39:54 – ljvujbteMKp0qZ7dZ1Gq -->
<!-- 2013-12-25T19:18:50 – MhZZDajrexkqkEQ2KZzk -->
<!-- 2013-12-27T00:09:30 – ArFfCydFn9Qhgqn2Ub2Z -->
<!-- 2013-12-31T16:36:13 – uWbw3kbcJMEI7uW1aaMx -->
<!-- 2014-01-01T03:37:31 – AShfETtvDcjA8z1U8cYV -->
<!-- 2014-01-05T20:36:18 – 0M3owTqHQmRKWvvEdV1z -->
<!-- 2014-01-07T15:57:18 – fGOa9LzHfnR5oDgY8Ggt -->
<!-- 2014-01-08T21:06:53 – b3U6bfgCZIlAPpcDSw38 -->
<!-- 2014-01-09T15:15:44 – A0eiXjxQKlZOeXnezlKF -->
<!-- 2014-01-12T11:14:06 – t6TWH9eGkKyZxzbDH3FB -->
<!-- 2014-01-13T06:28:18 – IeorzAVWLd1s4oN2XuL1 -->
<!-- 2014-01-13T15:21:57 – k3dOTahvmrqdjGyHrRLi -->
<!-- 2014-01-15T01:00:37 – a4dGoOekyKAGhGfzEGTN -->
<!-- 2014-01-18T19:43:14 – U8ZPzLe7VUce62vXFPpi -->
<!-- 2014-01-24T06:25:23 – pP0fNoaeVdxA8XRHmdue -->
<!-- 2014-01-29T11:28:36 – CkrFNq2eWQyYZlbXpRDR -->
<!-- 2014-01-30T18:07:49 – zr18R4xDuMI0BWWgOyb0 -->
<!-- 2014-02-08T00:00:05 – SaJopapS0xgiwzHRuSu5 -->
<!-- 2014-02-09T19:11:09 – INCQ9Gh5jp5s7niNGcHU -->
<!-- 2014-02-11T21:49:26 – XrlxmYRxV8FJw5YqVS90 -->
<!-- 2014-02-12T00:36:54 – 6QXek7lO7I6PWt19WpZs -->
<!-- 2014-02-18T10:59:50 – ybo8h5Brtf2n5UMwO55y -->
<!-- 2014-02-20T21:51:15 – aBPQfVcZdMGrrvVZOk2y -->
<!-- 2014-02-21T23:54:32 – 8buPo73uyfoWfI0soxTL -->
<!-- 2014-02-23T07:09:11 – LbX4qH4ylejhxlQRjlhr -->
<!-- 2014-02-27T20:50:55 – cfxF3aWTZWhVltKq2Kz5 -->
<!-- 2014-03-04T09:35:08 – 93yaB7koLX0vQQ31zUNi -->
<!-- 2014-03-04T21:55:28 – xV8nBUIQOowRHRKHyT1r -->
<!-- 2014-03-05T01:07:31 – lYfDGrJnogSGWshGRcov -->
<!-- 2014-03-13T06:47:23 – wXGqoaeOZsAlketNDJp3 -->
<!-- 2014-03-14T19:12:49 – DZQK9h4DSWzcv0gldg4X -->
<!-- 2014-03-15T21:33:09 – 8SGidzQaZjHGNv9trE8t -->
<!-- 2014-03-17T08:55:43 – vk8dK8NKtTJLWH3q132Z -->
<!-- 2014-03-17T11:08:10 – LBGbkMXiRYKGm8R1YX4L -->
<!-- 2014-03-17T20:41:50 – cUiAbOlpzVPUUomJ1B3y -->
<!-- 2014-03-26T01:54:27 – wO0Snz2T5oxWHJrDzM6q -->
<!-- 2014-03-27T19:31:29 – of2ef9m9AXgBkZjGjhm6 -->
<!-- 2014-03-28T23:12:30 – KMqpdKQTh7EYJVGXACMk -->
<!-- 2014-03-31T21:13:30 – nkaRLJngdO8d16aK4JTf -->
<!-- 2014-04-02T10:31:50 – Gh8XXeTHF9vxJ7dYkesz -->
<!-- 2014-04-02T14:48:43 – k5L9Ov1nkSJOfnc7hkbu -->
<!-- 2014-04-04T03:29:56 – C7O6IWyPpcgrPU7dmBUe -->
<!-- 2014-04-05T04:49:54 – mdoTMuPZToY1BEmbiE43 -->
<!-- 2014-04-09T17:10:44 – UCMPwwMjP5RytWbSpUKf -->
<!-- 2014-04-10T07:39:04 – 8KPYVmZvznTvLioQFCwp -->
<!-- 2014-04-11T01:52:10 – lbsDFJdsvSG4KO2mMxIu -->
<!-- 2014-04-12T13:54:25 – 43H1AAZNsYgNEKwOmNDO -->
<!-- 2014-04-12T17:46:51 – m5Oy4TdpzObXfQBuBIpN -->
<!-- 2014-04-18T15:55:45 – vq2FBS6lgInHAAVmkSrQ -->
<!-- 2014-04-19T10:06:24 – EVkQCtMZW9OvY2XzVoR8 -->
<!-- 2014-04-24T04:58:59 – 3Xg5J9MBq2Qym7Ip6MlN -->
<!-- 2014-04-24T17:56:23 – zTPNfRvASlu4koVVCAnU -->
<!-- 2014-04-25T06:26:03 – 6jEYRgAqQQKtCWO0MJVb -->
<!-- 2014-04-27T07:06:33 – GFnD8VZ9Z6uJ89eWiTTz -->
<!-- 2014-05-03T00:09:53 – 90XKOFNwHN9ejipBYtFM -->
<!-- 2014-05-04T05:41:05 – czrc6SHkjlGpJOBWTnBS -->
<!-- 2014-05-10T01:17:26 – Xyl5mzpSOeYfCGJEvVIu -->
<!-- 2014-05-11T10:33:05 – RvIgonJ1BIejMoCfeF9K -->
<!-- 2014-05-16T13:27:37 – CZR4HqM6JHrvBSPz3lH2 -->
<!-- 2014-05-18T01:14:57 – gDBXZXqZJTr4CmNjKfmZ -->
<!-- 2014-05-19T19:36:01 – a4Rvnhl5OGuUzcyKRVot -->
<!-- 2014-05-27T17:37:38 – xnW4daZvypdYBif17EM9 -->
<!-- 2014-05-30T11:55:03 – nL5frh9pmM4P26BQspDm -->
<!-- 2014-06-03T06:22:36 – eH0yAUn9IsxO47fsTUmT -->
<!-- 2014-06-04T05:34:39 – LG1AOmC9VxVjEt4JHSYT -->
<!-- 2014-06-06T07:51:28 – 4cab9aDqbEDlYeFt8yZX -->
<!-- 2014-06-08T11:39:07 – IOWh3hrpMHKV5Z4BJmWI -->
<!-- 2014-06-13T05:21:25 – YCMyxlBYwzhI2tc05sys -->
<!-- 2014-06-13T09:05:15 – HZm5j9dX3slpVGVkMfMy -->
<!-- 2014-06-15T15:58:25 – m8KptGulsRu6j2aiHMMY -->
<!-- 2014-06-16T21:26:39 – eYcxcnystXzKAPimI8KU -->
<!-- 2014-06-17T07:36:45 – GhDtucbi7rBis4EWBgsK -->
<!-- 2014-06-24T04:37:28 – 1VJ7ra3P33SACBOIndp9 -->
<!-- 2014-06-24T13:58:51 – lAPzay1SPMs6T3wlFMlD -->
<!-- 2014-06-25T09:07:47 – NSq9wumvU8povAKtRWeE -->
<!-- 2014-06-27T08:15:15 – s4SB56cow1U6U9Vrp6Yu -->
<!-- 2014-06-29T23:54:39 – yXt7dzKy4yQ8z41eFPRV -->
<!-- 2014-06-30T07:50:16 – rmD3c7ZUTv5fYvm9GG0s -->
<!-- 2014-07-01T13:15:28 – nEDHROUU9W7irQfujZy0 -->
<!-- 2014-07-02T20:52:45 – Uhv2arNoKPwR7I3q1fwM -->
<!-- 2014-07-05T18:53:30 – rEAHy6nOXI8hDhnT1iyy -->
<!-- 2014-07-09T06:33:21 – d3YQPKyWilmZh2RnUKXb -->
<!-- 2014-07-13T10:11:33 – QXdBZ9fHw2Gx2aGXZmCK -->
<!-- 2014-07-15T10:50:51 – FF6PuZNwRUqTLVkXZ0Fj -->
<!-- 2014-07-17T20:12:19 – dUm3zvRjMgvzvjfEkkIa -->
<!-- 2014-07-19T06:10:31 – 7vaxQfZLuak9oNXclAb0 -->
<!-- 2014-07-23T12:32:41 – 0W66gUajtiVYubeuezsJ -->
<!-- 2014-07-24T06:23:53 – LbUTQebl9RxHf26uUmnW -->
<!-- 2014-08-03T07:17:48 – beSSksB2hnkbnUuj8c3d -->
<!-- 2014-08-04T18:28:41 – RsLIjeFaaypcReK8ZZ3E -->
<!-- 2014-08-05T21:38:09 – qp5vzB9mzZwqYwzwi0sc -->
<!-- 2014-08-13T06:22:26 – 6D2Sv38Q6RMW2bmV3eMh -->
<!-- 2014-08-14T04:09:18 – P9VmsyfhdLRtGfMGS4oo -->
<!-- 2014-08-17T10:43:16 – Y7HypKz09eeSI6LUX1QH -->
<!-- 2014-08-18T22:26:36 – TNIABw3jIayRD9tbYrgh -->
<!-- 2014-08-19T11:32:22 – ASszyepGAGcaYd7ntrtG -->
<!-- 2014-08-20T02:22:52 – OK1OV6g3nJbnlqJMahrT -->
<!-- 2014-08-21T09:04:35 – wlI0Igm9lWnvjbRBbqFl -->
<!-- 2014-08-25T18:38:32 – HJlysEeGReVyPGOdRMnP -->
<!-- 2014-08-29T16:27:22 – DMJCbtCQ6fZZalErFZMb -->
<!-- 2014-08-30T14:19:40 – Ar7DgSyeai0Zdqo7vNj5 -->
<!-- 2014-09-01T17:38:57 – O2XFfcAPX4l7XP8QS6k9 -->
<!-- 2014-09-05T23:27:58 – 4fFbkIW5feHaHObdO9FX -->
<!-- 2014-09-09T07:06:19 – Y6thG7N4Hd939jlI8PW9 -->
<!-- 2014-09-10T07:33:15 – TxjS2PKIp8VrdjcRkomF -->
<!-- 2014-09-10T14:28:52 – p9KcPHDYCPZmMBzBHHGy -->
<!-- 2014-09-11T13:04:54 – UDAGeW7uPjdW656bPQB7 -->
<!-- 2014-09-13T12:24:34 – Rkf6xeknfHs8XtUIhbNd -->
<!-- 2014-09-15T04:02:40 – 5OuXZGs7V8pERenNobJn -->
<!-- 2014-09-17T01:36:56 – pLNq0zpUg0MEkXkNRmT4 -->
<!-- 2014-09-18T19:25:59 – TeCoWeOZa4tTiJIpE7Xb -->
<!-- 2014-09-19T15:32:59 – VaGiFYwUdOAtts3QdhKu -->
<!-- 2014-09-23T16:24:59 – di8jAxiYgaSuwMeWQQ69 -->
<!-- 2014-09-24T02:50:04 – if6kkzL7SKc2NkcHWaZg -->
<!-- 2014-09-25T00:38:02 – J6oaSeYlS8FriZWW2YNm -->
<!-- 2014-09-26T17:56:57 – rGQu1P4VjtRGkvggPGkW -->
<!-- 2014-09-27T22:28:39 – 8Zsksxf9NofDPyp4NIiL -->
<!-- 2014-09-29T01:05:16 – Gv7XcqYfXYmpscE1HZge -->
<!-- 2014-09-30T11:26:17 – 2phqhyWmddkwYbclY6PW -->
<!-- 2014-10-02T22:32:33 – dPJwy5eSLb9M8rcdfm64 -->
<!-- 2014-10-04T23:00:16 – C726MRYolM0NFPbyeR3M -->
<!-- 2014-10-06T06:56:26 – O9J9uJGaGmx15Wbdfewl -->
<!-- 2014-10-06T11:19:01 – HnWG1j7M6tlrLFimvGMX -->
<!-- 2014-10-07T11:40:41 – ZiRHfPokkOWa3iSnDqmd -->
<!-- 2014-10-07T13:04:55 – yC4GwUjxO9Md5jYAZpM8 -->
<!-- 2014-10-09T20:08:37 – ri6BZeM1lZ0nw7Af9I4J -->
<!-- 2014-10-14T07:31:54 – kBf4fA2tEVFSLi3RXs0E -->
<!-- 2014-10-15T00:49:25 – pMYcxk8Ci52UNnXD5Bj8 -->
<!-- 2014-10-15T18:12:37 – iger689XTv0jeCJGuzO5 -->
<!-- 2014-10-18T09:38:12 – yby4fSADBA7nzCeeoPHT -->
<!-- 2014-10-19T15:54:02 – 3PXmHvp0BRrdxu0a5RrM -->
<!-- 2014-10-20T17:22:28 – ZBq37Lxc0sRUZ2hjGbLx -->
<!-- 2014-10-24T10:50:20 – jHdvaGjFABAkuHHYbpjO -->
<!-- 2014-10-25T14:48:59 – Dq9S0ZnD3ICYg86Ll6ea -->
<!-- 2014-10-28T19:32:30 – 0q6usHYvrrXjKKGO4qyB -->
<!-- 2014-10-29T05:47:38 – iiKwH8ZJpQQcAGCvtKNX -->
<!-- 2014-10-30T13:45:15 – 0sCzXINYwHCS2tTKuThG -->
<!-- 2014-10-31T09:41:13 – RLYPpl7quVnXdCIM9zPw -->
<!-- 2014-11-04T02:05:46 – TQZhTP51vu4TED9PbaZR -->
<!-- 2014-11-04T09:38:20 – httQhEDzm002Lcj4VQiP -->
<!-- 2014-11-07T05:47:39 – XlZnKNlSPLt1JjF1xykl -->
<!-- 2014-11-07T18:47:51 – MSuoAQodN74uvHQPK4Hd -->
<!-- 2014-11-10T12:39:15 – OsBx5nU0Q3vKcLc5xa8v -->
<!-- 2014-11-12T05:43:36 – Cmf9A6dvkWTJYSVTs6eW -->
<!-- 2014-11-13T00:13:05 – PQ1sqFdWL12ng6enGYpt -->
<!-- 2014-11-13T08:23:03 – ckqPLYStWrPrVyn1CkEk -->
<!-- 2014-11-16T14:44:03 – y1EavBTTAHeXpYyGwzX6 -->
<!-- 2014-11-18T00:31:41 – 9aXOWvESwA69gpQf67fz -->
<!-- 2014-11-18T06:57:47 – ib0TsAOoq1OdVD7lrEku -->
<!-- 2014-11-20T19:41:03 – sLz0K4gxJPDO5tPjjrN6 -->
<!-- 2014-12-02T04:10:20 – x6xxAJz1yuIpOmQ0buoS -->
<!-- 2014-12-07T21:28:28 – dkSlmDBWKvaaSooJSbM9 -->
<!-- 2014-12-22T06:39:07 – l68ODvXP6okYO3bDKG83 -->
<!-- 2014-12-25T05:32:17 – dDcd9FwCUyHQh3r7rOQI -->
<!-- 2014-12-25T06:23:27 – TzpxPgSXzuQa1SlcxtoP -->
<!-- 2014-12-27T10:19:39 – 9AVqjodE3b7zjAep7q1K -->
<!-- 2014-12-29T01:13:04 – HaJYoAwh7GyB3ideKd2U -->
<!-- 2014-12-30T10:41:57 – xJtuY3jx6bWremdE62QM -->
<!-- 2015-01-15T04:03:47 – kW5wB8ATihpKGs13Wuyo -->
<!-- 2015-01-20T03:48:59 – WDI8dNeACJ8f83W0vohj -->
<!-- 2015-01-23T04:30:34 – TfLdG1P90Aic0HhTUACQ -->
<!-- 2015-01-24T05:11:28 – rUozw2JCe0SvO2P1KerZ -->
<!-- 2015-01-24T06:02:20 – 7axtOMAFZUzVzEnAw1Xe -->
<!-- 2015-01-29T11:14:23 – 9EP6Qa2UUfXlesxgXR49 -->
<!-- 2015-02-05T20:23:15 – fDcZLphft5fAHJ0eFLVJ -->
<!-- 2015-02-09T07:00:12 – R1HUpgW5rkwSCuJWTEx5 -->
<!-- 2015-02-11T12:49:03 – lVcvpEbM0iloAbHVKP0q -->
<!-- 2015-02-12T06:20:12 – 0Tbg4soCIZ3Qk4bW7Pky -->
<!-- 2015-02-13T19:44:40 – HDoGs04fDulYt1MQxFCb -->
<!-- 2015-02-18T10:49:44 – Uy6EnGGWTKPMx36EwXst -->
<!-- 2015-02-25T08:31:21 – ipWWOMzum8D3B4iMhMA2 -->
<!-- 2015-02-27T21:30:11 – plC0la4qsiFD8S1fAIRL -->
<!-- 2015-03-02T16:18:26 – Bx8fRmhnczd9Lq7pDHhw -->
<!-- 2015-03-06T16:58:18 – 61FcI4jhCgFd6Iwsinle -->
<!-- 2015-03-07T13:48:15 – fVea7yd9QlJI8RMUj3xa -->
<!-- 2015-03-11T05:19:09 – FB2PH2pjNgrJ35GLcsna -->
<!-- 2015-03-11T23:35:09 – PN1mzpBgqG4p28DJCG4P -->
<!-- 2015-03-12T09:32:07 – rbBRKJA2uXd4GH36cYpM -->
<!-- 2015-03-12T17:00:42 – Q8V6fnX5OJSZch0zrw9w -->
<!-- 2015-03-17T20:33:53 – GHOkoXf3WZqkrDLSZulf -->
<!-- 2015-03-23T15:37:37 – sBeCYTmcQBV2XWrIRO6A -->
<!-- 2015-03-27T01:46:46 – zEaVaxgT7Kmq6HIl6dsh -->
<!-- 2015-03-27T02:06:57 – qJj2uNKQDy5PwVx0grEU -->
<!-- 2015-03-30T05:07:14 – MYx8vpnswzOiPqEcdI5k -->
<!-- 2015-04-02T23:10:18 – ZupG4QPVp6lYd5nzikuB -->
<!-- 2015-04-04T17:24:03 – w3B67ylB5eIZJErTb3HK -->
<!-- 2015-04-09T23:47:50 – l4VoJLgl5ofViWkONbAT -->
<!-- 2015-04-12T05:52:43 – m9IPz3cQ2rgtK2YZT1rJ -->
<!-- 2015-04-12T18:04:27 – 8vlh94Qmcs25TGITTjyu -->
<!-- 2015-04-17T14:14:35 – 41tvvV5I3R9TXT8CIkl8 -->
<!-- 2015-04-19T16:48:08 – FiyOsABhdedpgl2oCzcT -->
<!-- 2015-04-21T21:50:01 – AGOaMpkasvYd8V5HKak3 -->
<!-- 2015-04-22T12:48:13 – AoEWbtA9k4kR3NZ0aWpT -->
<!-- 2015-04-26T22:44:11 – 0DLhcLYGZAxBq4EhumqE -->
<!-- 2015-04-27T12:59:00 – Olx9Z1k0ztnmzdP3uBUj -->
<!-- 2015-04-30T00:13:20 – 3SmdqmBMCDnjUBeRJ7VC -->
<!-- 2015-05-01T06:40:27 – 6NEqgYyjyKp3XrXphYTf -->
<!-- 2015-05-01T15:46:49 – 0ANOpuI1p2Fot7fDGRth -->
<!-- 2015-05-02T21:11:44 – 47ei0dg3IO9bxKwlYdyu -->
<!-- 2015-05-04T06:59:04 – ACic9RVUi2w06fzibduq -->
<!-- 2015-05-05T15:41:43 – aND0PojfSds7Do9TmE1u -->
<!-- 2015-05-07T16:24:54 – Jm0kAxoliz79dJHZwoLN -->
<!-- 2015-05-10T08:15:21 – LPm284Qg7RwfbOD4hF9b -->
<!-- 2015-05-15T08:31:20 – YGTGdmEFF7exjJsiILpP -->
<!-- 2015-05-24T05:12:50 – MEsEAfecQS5nRHgiTJQU -->
<!-- 2015-05-29T01:43:52 – qLoHYdYcE1mUcmPF0jcL -->
<!-- 2015-05-31T23:57:34 – kkQQIzskzeJa1IkyOQeL -->
<!-- 2015-06-02T19:25:54 – vzSYDhTlSKiFEYU8X4pz -->
<!-- 2015-06-04T20:57:36 – dZdJZ6H8sDAweCQJ6WOp -->
<!-- 2015-06-11T09:21:24 – OBmGlmrkbvFrINFKHHpd -->
<!-- 2015-06-13T14:06:28 – aRq0K8F64j25NU0lWNKB -->
<!-- 2015-06-21T16:10:56 – p4DqBEFCr8vVfxkxP0xY -->
<!-- 2015-06-23T16:57:34 – pKk4hhN7LKYLqfvn2ebb -->
<!-- 2015-06-24T02:17:09 – FC9fT6pTXmwI0N70QcYV -->
<!-- 2015-06-28T02:36:35 – NUnvP0jxrrSfhd0riPSF -->
<!-- 2015-06-29T02:18:22 – DZUKtXPOUq3O2ckYitBI -->
<!-- 2015-06-29T14:42:37 – xi1Vk1V8ihn60KT25vpx -->
<!-- 2015-07-01T06:21:20 – mggOpuhV0K2HR0eogP92 -->
<!-- 2015-07-01T11:33:13 – a9FsAhzV3zdNCvueYQR6 -->
<!-- 2015-07-06T14:55:49 – zXJrJKXTeLYKkthcYwaL -->
<!-- 2015-07-09T10:39:43 – jTyVKEqrItAgucl3MOUB -->
<!-- 2015-07-11T07:26:28 – 6SKW5uufs3jihBxqpmC2 -->
<!-- 2015-07-12T02:40:29 – TQssWyQNGbVIs0ITrvSL -->
<!-- 2015-07-15T21:12:47 – 5KAHRKYpqA7yBa3z0jVV -->
<!-- 2015-07-24T06:50:44 – CwgvrqgLdZ18kjPcMDPm -->
<!-- 2015-07-26T16:39:30 – iQ49j3hb7TnRsLVZzfAv -->
<!-- 2015-07-29T11:46:04 – lDQL7F86GEGI4HPCdMiZ -->
<!-- 2015-07-29T16:24:14 – 0PM1IdkZLQkSlIXAU7B9 -->
<!-- 2015-08-03T07:36:14 – QxucOiT1yHm03I5uNd7z -->
<!-- 2015-08-04T03:49:36 – aylZv818K2yd1H9GeWG6 -->
<!-- 2015-08-06T10:13:20 – U8McPD3PoTvVMb7raBku -->
<!-- 2015-08-09T05:35:28 – h1wYxI2b9eHpN9uXRhQo -->
<!-- 2015-08-09T17:42:04 – 7QuCXAHDuujnIrwZTHTb -->
<!-- 2015-08-10T03:24:46 – K2BPys3IjcFZlWFKypdZ -->
<!-- 2015-08-10T08:05:28 – ZTa7OP1cxm6lHasp2zgs -->
<!-- 2015-08-11T13:51:22 – kfU4pBPt7mT8xQjwCHbd -->
<!-- 2015-08-12T12:48:17 – t9kNQzK7SAAKmXoLyqKv -->
<!-- 2015-08-13T13:23:49 – g5edrUKQyWmlPzdLPxez -->
<!-- 2015-08-18T21:27:08 – qt2so3FmBdszJPcSOApN -->
<!-- 2015-08-20T09:22:42 – FMD3KBwUsEoHHE0fxoUv -->
<!-- 2015-08-21T18:49:54 – nFXivrVKI2YkMg45kxgH -->
<!-- 2015-08-22T11:12:51 – IH9z8weO3O5MSh6xEAL4 -->
<!-- 2015-08-27T03:49:53 – xQ9M1ijPInLcHGBSLWFN -->
<!-- 2015-08-28T18:45:28 – JeIymLSdzXzuHWiqpOPh -->
<!-- 2015-09-01T07:12:23 – 9O8Bnpi8DEPVG9fEq3dp -->
<!-- 2015-09-03T14:09:36 – e0fReD8Q8S3HSUiHN9cW -->
<!-- 2015-09-03T15:50:54 – D1euFh9I4ifUzrowAPwP -->
<!-- 2015-09-07T17:52:26 – VUhPIDg6zApKfip2CZjk -->
<!-- 2015-09-09T17:02:27 – 6ozuu0ql5x0FWFtbA571 -->
<!-- 2015-09-16T21:53:41 – o85Tja2XY1rqgG8TfsC1 -->
<!-- 2015-09-17T23:49:04 – bkqvK13nM6hF4k5pu4zc -->
<!-- 2015-09-20T00:35:00 – NBp4lKPbAtkYd3GHYUfB -->
<!-- 2015-09-24T11:15:50 – wN8pr6oIHjfZhpJvcHPO -->
<!-- 2015-09-25T15:00:49 – SWDLUuwDq23MQZEjXauq -->
<!-- 2015-09-26T16:17:09 – 5LlJK1AHu3Ft1aW9uut6 -->
<!-- 2015-09-29T13:11:23 – ysg0pgA9R4bT067BEs9F -->
<!-- 2015-09-30T11:01:05 – YqykMmHHAcwCvDnkZ5rW -->
<!-- 2015-10-01T13:59:03 – YKKFKOh0yKqFOx8u83KX -->
<!-- 2015-10-02T06:00:04 – SAukC2mEtw3lA38UVGBZ -->
<!-- 2015-10-02T10:09:26 – P8EIqvRGHaQTQWf310UV -->
<!-- 2015-10-05T18:56:10 – vtDyyHkWsC1bgRnlkWst -->
<!-- 2015-10-07T19:01:50 – eWDM19KbQNAArKDgY3aT -->
<!-- 2015-10-18T05:16:53 – wj32eKf6Sb6sAZK7foMw -->
<!-- 2015-10-18T23:00:32 – aBkNFnVeg6jCMio2GOYo -->
<!-- 2015-10-20T00:13:18 – 4YbMcmdulZxEZZyNKn3s -->
<!-- 2015-10-20T00:52:30 – 3QBA7HSbRSHzmq3MLANm -->
<!-- 2015-10-21T10:19:07 – zpoTO976pvL2QtSi57kF -->
<!-- 2015-10-22T10:00:32 – FOEsWvtIjJUQXdOh8clf -->
<!-- 2015-10-27T14:00:47 – ROvsP8hTOKXjniG4bAHt -->
<!-- 2015-10-31T04:08:22 – YRGeCTr99rqcz3I02WdO -->
<!-- 2015-11-02T06:49:47 – N8SOXvA1v9CxyTGHQLMz -->
<!-- 2015-11-02T09:43:50 – KYHwhN49aWYlDmUaCTqV -->
<!-- 2015-11-02T17:08:54 – bGksr6EItVllE6slcTVM -->
<!-- 2015-11-07T22:35:12 – fky4LRIsgkzRViK2xF5k -->
<!-- 2015-11-10T13:46:56 – WOI4aAuj1PhKF1KsgjDj -->
<!-- 2015-11-11T07:01:40 – 1QQw7rWhqYnBKeSokc8r -->
<!-- 2015-11-11T18:14:35 – JsoxIyKt6g9U6aedOMF2 -->
<!-- 2015-11-19T16:56:22 – XP1wSbdSlyunz510x9lg -->
<!-- 2015-11-29T07:22:31 – GyV7RReAF0msn3LcW0tk -->
<!-- 2015-12-05T23:11:59 – 9t8WSinX4Y7jDiafbUJh -->
<!-- 2015-12-08T20:31:50 – N9noOJO51ZDqvNnuqncr -->
<!-- 2015-12-09T14:02:22 – Oa1T76ZNDYl14ZWOXHdG -->
<!-- 2015-12-11T20:51:37 – tSJsgvrvlwbynrTtR26M -->
<!-- 2015-12-24T12:57:31 – Kuqm8obb7k5V8qgZUTEf -->
<!-- 2015-12-25T11:03:24 – NwvVHzCb6icDVK4Gkvx9 -->
<!-- 2015-12-28T03:43:14 – HQm5xTKHe77mfkznmE4l -->
<!-- 2015-12-30T10:50:40 – Es2A4g8zLvX38Qt2xOaw -->
<!-- 2016-01-01T23:19:22 – Wkg1qHoGB6puBqsokHrG -->
<!-- 2016-01-06T21:23:42 – 4PoAECaT7yEPkNiEqbA5 -->
<!-- 2016-01-08T01:41:05 – n9Petef2KQZ3aTs7LkZT -->
<!-- 2016-01-08T08:51:57 – TtVoHsXKr6HzulaNTm92 -->
<!-- 2016-01-10T09:34:06 – 5knnKrF5ysDu3sVnsgEg -->
<!-- 2016-01-10T21:05:07 – AbX5NRseRqxTJwrGr6nQ -->
<!-- 2016-01-11T14:32:05 – S7K0sFRqgOfr5FusS11S -->
<!-- 2016-01-11T21:23:55 – SUB9CckdBQBDHkaX6jof -->
<!-- 2016-01-12T01:03:36 – Z2DnQFc1Ib1C0prs0gx6 -->
<!-- 2016-01-19T02:23:09 – 1VPDIauHkbOF2xG2wyHV -->
<!-- 2016-01-26T22:20:35 – FYjdf4WF5fbWJGjj9RCv -->
<!-- 2016-01-30T20:34:10 – ITkr8N95qthUFCyNvQDM -->
<!-- 2016-01-31T11:03:28 – Vo4iqXt1i11ffeka6cRL -->
<!-- 2016-01-31T18:21:40 – aJObxEgLqIUQkCieLRMx -->
<!-- 2016-02-02T09:53:28 – Vbn1I7oKmTZsJg9Lspeq -->
<!-- 2016-02-14T11:03:08 – p8QSElOtwJgPaldmAwYN -->
<!-- 2016-02-15T05:11:44 – 1F26SoWI3lWQPa7woOiZ -->
<!-- 2016-02-16T15:43:50 – LT2JLk7781hUaP3g7Cho -->
<!-- 2016-02-18T15:06:25 – j8tCdrJsrb6Ot3wuEOa5 -->
<!-- 2016-02-18T22:25:09 – BDntV8jirZJvZpGLJOz6 -->
<!-- 2016-02-20T13:05:52 – 3zns9RyIuGntRVi5GruV -->
<!-- 2016-02-20T13:22:42 – eJGSHNrRLVzWlu4wcn5W -->
<!-- 2016-02-23T01:14:48 – 243hvABtRAHrHTgK7OrI -->
<!-- 2016-02-26T07:35:53 – Ak4pWIjWHdAkqjk9G9MO -->
<!-- 2016-02-27T07:16:54 – NOyeeNPLO7sgvDazzUPE -->
<!-- 2016-03-08T14:09:28 – okGCMf317XTC4RAlF7am -->
<!-- 2016-03-17T10:23:46 – ip4p3IwtEEKhjLvlMVmm -->
<!-- 2016-03-17T23:48:29 – 5XzxPr78z7BQxOOjTdat -->
<!-- 2016-03-18T20:07:22 – OEkMFCWbUb7eTl6ZrqQk -->
<!-- 2016-03-21T23:21:54 – CDIMiWitqqk5kMW7VNvX -->
<!-- 2016-03-26T23:49:41 – 1EgMMp2SxLBQuzsdOBNG -->
<!-- 2016-03-27T17:16:43 – NaewIiyKS057ciUMx5Sv -->
<!-- 2016-03-31T07:29:32 – wdlkzpV1CbIvUNoLoJHG -->
<!-- 2016-04-01T20:01:53 – Di9ZwtZ7bt6EyYfh7WXh -->
<!-- 2016-04-04T16:07:35 – 03Fms7Nn6800eC1y5AlY -->
<!-- 2016-04-04T17:53:09 – wncthaikYRUHRyePihav -->
<!-- 2016-04-05T15:12:26 – MCxwNHGHF5tsgqEQCrZx -->
<!-- 2016-04-07T09:04:38 – PXOWLj8v4VtBowFcU8rd -->
<!-- 2016-04-07T18:17:05 – nTq04nmptzGx1ZcA8uA1 -->
<!-- 2016-04-08T11:39:15 – cv09EvwcJzWe9eaGqPtl -->
<!-- 2016-04-24T13:21:22 – Gg6qVYB65rD9AfFOMF8R -->
<!-- 2016-04-29T17:09:15 – CEkrSnPjwluHYSR73qtv -->
<!-- 2016-04-30T09:51:09 – yszQr3yVRDuUNSzWkTMS -->
<!-- 2016-05-08T01:39:33 – hafddPwWgD3JWt2sR6G2 -->
<!-- 2016-05-08T20:59:12 – KjIQloGoXv7GbSBWAuV6 -->
<!-- 2016-05-09T07:54:31 – hUBZTenP1xFPbZH20yTv -->
<!-- 2016-05-09T09:42:35 – mtHR6nfY4V2CetF2IKoh -->
<!-- 2016-05-11T16:10:18 – xPXmfUiAu9B50W1nf51u -->
<!-- 2016-05-12T20:28:27 – PIDVR8YaVnKfTmUVkejM -->
<!-- 2016-05-15T06:40:05 – gwhM8khPKH9sSKRRLYT9 -->
<!-- 2016-05-16T05:38:01 – b1RIrIDpBeBgljqr5D5o -->
<!-- 2016-05-16T22:55:01 – sa47RQx9Y0emblsprWok -->
<!-- 2016-05-27T23:48:22 – GdzrQPkMqGL1iTlIUh2T -->
<!-- 2016-05-28T15:42:49 – HFvvLpTFUJMbInAORrR2 -->
<!-- 2016-05-29T07:30:31 – 3a992b7TCtcvHSFg9lnu -->
<!-- 2016-06-08T21:13:03 – sncg6COoaF8RoRcjaMAW -->
<!-- 2016-06-12T06:49:52 – RAeisjK5kIEWb2n03alu -->
<!-- 2016-06-19T16:38:26 – hWhSR6CAnDMQimmdj8WA -->
<!-- 2016-06-20T17:58:18 – 4L2UyVZ8WUmtwtroMpds -->
<!-- 2016-06-21T01:08:33 – Drm7WDZi5rgYX29dbAAY -->
<!-- 2016-06-22T13:12:39 – ONEJj0ydQcPgeL8Us9JZ -->
<!-- 2016-06-22T19:26:10 – QMuIjoZKolEAfz38uZGP -->
<!-- 2016-06-28T00:43:31 – kVXWgiMs02kE4yCalxau -->
<!-- 2016-06-30T14:10:51 – yX4g87Qg5PKxAUPB6gar -->
<!-- 2016-07-01T22:10:11 – 2BiJBUxjMMPodeltpRP2 -->
<!-- 2016-07-02T00:05:17 – 9qK9DfH5sJaazMo9uZOt -->
<!-- 2016-07-03T03:59:00 – V9xdg9N8iXipuFituHqu -->
<!-- 2016-07-07T19:10:48 – kAyvNq1I6hUbsXvrxlvc -->
<!-- 2016-07-20T09:02:11 – 7I4YZ36uD5BozhCVTZLZ -->
<!-- 2016-07-22T19:37:39 – 5Kkg3ZDcBPmmb9S2gnFK -->
<!-- 2016-07-24T13:01:45 – kGd5cQTPPYdFNlFKOoTo -->
<!-- 2016-07-28T01:20:31 – oqGWpVpOgTooNlLE4iH6 -->
<!-- 2016-08-02T17:19:03 – UGgCv7wQR3qRjYxQ9IJ8 -->
<!-- 2016-08-04T20:29:03 – 5V9gcfNzZpLLutTgkboi -->
<!-- 2016-08-05T12:42:19 – kIoE2ouGYveAnaNdyRe7 -->
<!-- 2016-08-11T01:29:24 – LNaGlDJlj4jhR5R6mZRr -->
<!-- 2016-08-13T15:34:32 – GIeVlUiAud4Lw1Kn2ZjP -->
<!-- 2016-08-16T05:56:20 – Xw1jX8Sf37Uplzkdk2Ag -->
<!-- 2016-08-20T15:08:20 – zDAQtWGJ517yaKYBDAlc -->
<!-- 2016-08-21T15:36:34 – BktgQWQy4ZSfci9CtPkG -->
<!-- 2016-08-23T05:39:32 – sX9gRn9rmA9IhziD6xQS -->
<!-- 2016-08-23T23:39:51 – tIFV2j37NrfWtAatYTe0 -->
<!-- 2016-08-26T05:26:34 – MOdbbdpyiwQuouBnTBrN -->
<!-- 2016-08-26T06:05:52 – PD7ZjPXoSoNvlrKfpxO6 -->
<!-- 2016-08-29T04:28:21 – YzhYalpkGUfchuyO5z0I -->
<!-- 2016-08-31T19:52:44 – rpkZVdRGFELaXrkctNm8 -->
<!-- 2016-09-02T06:51:47 – u2IxgRTxpYCIIde66PbL -->
<!-- 2016-09-03T06:54:02 – fc7slgC9bwCHTVSQeaFb -->
<!-- 2016-09-12T17:06:11 – 58OAxtolgWcZar2pqYYy -->
<!-- 2016-09-13T11:14:02 – bS7rosPGJaxu0QbTLxjN -->
<!-- 2016-09-15T14:05:01 – gZOsE0xZaBb5gEwZrYzw -->
<!-- 2016-09-16T10:05:44 – 25GU5whXatTroqbOxeT7 -->
<!-- 2016-09-16T21:37:06 – G6TMwCp37GdYvxOqrE67 -->
<!-- 2016-09-19T15:13:11 – elLOL5135Iq9DVkjqWcX -->
<!-- 2016-09-21T02:38:13 – Ev6aF9XYi3BPB2h7cmkJ -->
<!-- 2016-09-22T08:35:03 – rpeCR3end57hIF9omh2F -->
<!-- 2016-09-24T01:26:07 – ePwDc78CcVgfDpmwd1yR -->
<!-- 2016-09-30T20:02:52 – m9u0LPhUoKJj5He2yP7w -->
<!-- 2016-09-30T21:31:11 – E5a9RuwAuGRbbD69YhWV -->
<!-- 2016-10-03T09:55:38 – 8HYgbFJZ9qpkOqwGKJOA -->
<!-- 2016-10-04T11:39:28 – gLU668cxTCTH9loUtDyC -->
<!-- 2016-10-11T20:47:55 – GQlfaC7GCiXCoKchSCz3 -->
<!-- 2016-10-12T14:39:06 – BY1yIWztAempyAPQIr7s -->
<!-- 2016-10-13T09:31:30 – m1VCDEpZjonTUdvTl0mh -->
<!-- 2016-10-16T01:06:26 – 5hR6OOPQ2JpANXaswCNL -->
<!-- 2016-10-16T12:49:31 – HvdHSUGCf7fgKfwgFbQj -->
<!-- 2016-10-16T23:15:16 – aNfqYg5jeIRrh1C6RAb0 -->
<!-- 2016-10-21T00:31:52 – kiHDJjvO0oDeeSyOnUVU -->
<!-- 2016-10-23T02:56:02 – 4CULUrasmC25ccvVFcFK -->
<!-- 2016-10-23T06:45:23 – ukqMUlB22BDoWTqOzya4 -->
<!-- 2016-10-24T16:17:24 – LUsA0924bMOOY9JkpWbM -->
<!-- 2016-10-29T10:18:57 – fnsKOphOfDaA5REnMHOX -->
<!-- 2016-10-30T20:59:42 – hOiNSaX8N8KMU74qdAi6 -->
<!-- 2016-11-08T02:45:54 – kHbxrWUtAKfWkX4SZ3yd -->
<!-- 2016-11-13T03:16:01 – M3G55mg6LfU6DLwmkmZD -->
<!-- 2016-11-13T10:58:54 – q3dEFUtUefnBRIOo0fYO -->
<!-- 2016-11-13T14:39:26 – 9gUyoPYlkaOU8KuZIhEG -->
<!-- 2016-11-14T07:42:48 – aORmEL5g98cKUk5XY2qD -->
<!-- 2016-11-20T06:16:01 – vPGSdkdebeusddgbykY2 -->
<!-- 2016-11-20T23:32:41 – oiRh2ArGhBUKWuIBM0GL -->
<!-- 2016-11-21T14:26:11 – GuL4Ggp45vXPMxKIMleu -->
<!-- 2016-11-22T08:31:17 – 3EgUdEEi3mEdUUQYiATV -->
<!-- 2016-11-23T20:05:15 – XE9XLMgRKFXGYjI87ntt -->
<!-- 2016-11-28T00:58:16 – eKn3z2NPW0YyMUDtbvhS -->
<!-- 2016-11-28T01:00:15 – xU04CVcuAugp2J45twaa -->
<!-- 2016-11-28T23:13:24 – qSV1zBpWR9lwAC6XIEjD -->
<!-- 2016-12-01T04:05:11 – U6PvP6v57gr1sEETBjyK -->
<!-- 2016-12-03T01:40:43 – fDzFFQgOMnHYdXkVIFHn -->
<!-- 2016-12-03T09:10:38 – TrWIWwzSPS3G1jjLXz1M -->
<!-- 2016-12-06T02:14:23 – IUZDarutSiL7wQb4KxDr -->
<!-- 2016-12-07T11:13:02 – j2lW3II3aIvjvy4VCYwM -->
<!-- 2016-12-07T22:04:43 – tEENYXKykHfNf9NMjXwP -->
<!-- 2016-12-11T21:51:41 – 6CddaVoYVAwWQPPBKcMY -->
<!-- 2016-12-12T04:40:46 – Ppzr2UCtkUHKW2hcZRTf -->
<!-- 2016-12-12T07:33:57 – u386l0jztoJOox4jFIYR -->
<!-- 2016-12-13T03:58:21 – Y5Y2Ii22yYKfHyOVxtgP -->
<!-- 2016-12-14T22:14:26 – RJzG4ZUpJ7xK5gIhRv4X -->
<!-- 2016-12-16T13:02:36 – V4FK3okFaaVfrfRJEUpE -->
<!-- 2016-12-17T09:04:53 – UiYUArc03hvZR3Qsit7k -->
<!-- 2016-12-21T00:55:55 – 4k3u3mViQYEWLFgdktCo -->
<!-- 2016-12-21T16:41:02 – 9pstL2X5JYmyNbyiJkWZ -->
<!-- 2016-12-22T07:10:40 – Kxj89jvRqoGyy7D7uLLi -->
<!-- 2016-12-22T21:02:33 – 5jFZinO5N2iKkaBd9P7U -->
<!-- 2016-12-24T23:44:28 – ZjLf0KCfrv8As2oG4NrJ -->
<!-- 2016-12-26T10:06:13 – JanmqjD8v42pjhTg86pl -->
<!-- 2017-01-01T09:48:16 – T9JLBSawS7ZvCFbToz3b -->
<!-- 2017-01-01T10:47:12 – LTtgVpqPwx8JQYKUMKMO -->
<!-- 2017-01-02T20:14:35 – mI47VFWf8qKaPereBbU2 -->
<!-- 2017-01-06T14:47:45 – K7F5ey3UbDVPY7FPWkBP -->
<!-- 2017-01-16T18:42:42 – tkXCTvqLZhNSpSYuNowp -->
<!-- 2017-01-21T12:54:25 – cS0Mn0DEdOsrcmou7qJR -->
<!-- 2017-01-23T22:02:11 – KUe2ZfzAqk0IXZlakER6 -->
<!-- 2017-01-24T20:31:08 – K8jy9ALeeMxlRxsdDwN2 -->
<!-- 2017-01-24T20:56:08 – 4qOnby5RSMWi21FrXc5H -->
<!-- 2017-01-25T01:48:19 – iHIBpnifKdKKfB0NffGf -->
<!-- 2017-01-26T01:17:58 – uuRZVtrQnS7hc20pR1Vn -->
<!-- 2017-01-26T02:35:31 – Wbn8dzEjC3ciBrnWwvq0 -->
<!-- 2017-01-27T09:03:25 – RvVCvQhOY4RkD5sfZmqr -->
<!-- 2017-01-27T14:02:29 – eJQo4dAYpaWJkoO7X9zk -->
<!-- 2017-01-29T17:14:34 – lfvkKj96mtmkH3j6UpYB -->
<!-- 2017-01-30T06:00:32 – 3suHBMstGgpLbRxjJseJ -->
<!-- 2017-02-04T16:29:13 – S3XHLzZj5pLUWWf6iZsg -->
<!-- 2017-02-09T10:42:45 – KmTUqyZ8uXZx7MJd35bJ -->
<!-- 2017-02-10T23:03:11 – 58sZf2MQbwCjMZGnCaId -->
<!-- 2017-02-12T22:51:36 – kWv4Qq6wQYtxiUxodQgp -->
<!-- 2017-02-15T21:58:24 – saVOW5zrSAEPhlRSxuh0 -->
<!-- 2017-02-16T17:16:34 – WNfJpAEvO3qgo5JInRBe -->
<!-- 2017-02-17T23:41:39 – cuD9dxwfdHOQvzZIvdTC -->
<!-- 2017-02-18T21:08:10 – JnyApDIQrYGQiai2Puob -->
<!-- 2017-02-19T12:05:22 – 1l3Sb020Xrp0j9LxNWaG -->
<!-- 2017-02-21T07:35:48 – wMiaa491kvhuuXU8siJv -->
<!-- 2017-02-21T08:33:50 – Qq1PjsSycAmURUbu1ecl -->
<!-- 2017-02-21T16:38:58 – 1mm46QuDk98CKcRX8t4S -->
<!-- 2017-02-22T10:56:56 – mPluXuujVfYULjFJUBn6 -->
<!-- 2017-02-24T16:07:29 – XaaQbNgWjKiXW3jmL6u7 -->
<!-- 2017-02-26T20:59:14 – kjCU0faYaE8chCzefiSi -->
<!-- 2017-02-27T15:53:08 – oIYZp0FEtURqvYtuRFgL -->
<!-- 2017-02-27T22:42:38 – s6VEGOrPVLn41s6FefOH -->
<!-- 2017-03-01T13:24:59 – Rg7NrmtQZswXh7rEAM5d -->
<!-- 2017-03-01T23:57:26 – 3vgrldeIsQTbtM8keeBx -->
<!-- 2017-03-02T23:35:08 – G3qgulTfH4dEtLHHo9aw -->
<!-- 2017-03-04T02:13:02 – cRtXFUNEQvat6qE0qnLx -->
<!-- 2017-03-05T07:17:05 – x5eteh3bK0gJ2WuKgJo7 -->
<!-- 2017-03-06T15:25:39 – xqxp0kCN9ZNVKVSy5gQf -->
<!-- 2017-03-09T16:45:34 – a6zujg8ue38A0RzVb7xC -->
<!-- 2017-03-11T01:54:30 – SWmqWvJBVFVFZ6lMSJIc -->
<!-- 2017-03-14T02:23:56 – gskILdikKcclH8npMLCH -->
<!-- 2017-03-16T05:57:20 – deH4EB9QKatbwRUFNN3D -->
<!-- 2017-03-18T01:42:57 – lPyMROQTbhIQHph5EO4n -->
<!-- 2017-03-18T21:39:00 – MIBII74yIuzOWDIzSzIX -->
<!-- 2017-03-24T05:42:29 – GOuAyw7j7Jc9So0nsYGn -->
<!-- 2017-03-24T13:07:44 – tfVmYn1xIFJqejNbKyi6 -->
<!-- 2017-03-26T18:12:42 – awtAooCSHAfn5xEZTYmT -->
<!-- 2017-03-27T09:43:37 – PfEZNoylX7BFnT03HR2M -->
<!-- 2017-04-02T07:46:40 – yVNI6OSrG54pDoEoHPUv -->
<!-- 2017-04-06T11:05:44 – ShrmffnMDagWIjGDHOVb -->
<!-- 2017-04-07T13:24:15 – k0XkXTpGwXDKIUxBEMPn -->
<!-- 2017-04-07T13:50:49 – zxYyT6HURtIcCReixdCe -->
<!-- 2017-04-08T00:46:08 – QkD4tNQFWivsIohibtJW -->
<!-- 2017-04-08T13:14:26 – VCkHOKTc3UL266ocHRHP -->
<!-- 2017-04-12T03:29:01 – MWVIDuNGZyP2ejdUT6Za -->
<!-- 2017-04-16T04:51:59 – juERTCle614JDgCEYYLf -->
<!-- 2017-04-17T11:05:48 – KPU0yCtBHjNNaPgsuZPm -->
<!-- 2017-04-19T07:23:52 – a7YHruWGaKec7mrYK8f9 -->
<!-- 2017-04-20T03:13:17 – G84pSFuLy78wjv7N2trK -->
<!-- 2017-04-20T21:39:03 – 9s8NOuPWB73urL7LfUuY -->
<!-- 2017-04-25T22:52:23 – 7kT93BOAJTGFDwPDtyIu -->
<!-- 2017-04-28T11:26:05 – zwAlcAxbY4W3zNvLDm6G -->
<!-- 2017-04-30T11:45:58 – bHdnB9I5cKruWpwurO4n -->
<!-- 2017-05-01T00:38:24 – LVwlocxOsIaHIgxbE4Xw -->
<!-- 2017-05-06T10:07:39 – qXAY3gfB6qwyAXssrHSv -->
<!-- 2017-05-09T21:39:58 – civ8n1dqGbKwhAlXmokc -->
<!-- 2017-05-11T23:09:07 – i702V1sRlm5vb37e9cFn -->
<!-- 2017-05-12T22:44:28 – WZkuLygb9gL5dmIv4QME -->
<!-- 2017-05-14T23:13:23 – QTfjGTGPhSWJ6o65Orrn -->
<!-- 2017-05-16T16:45:19 – FyJXIjfG315XqGZXd3Ih -->
<!-- 2017-05-19T07:28:17 – 9MSLp51DYQ6mFtMeoq4y -->
<!-- 2017-05-21T11:09:05 – yqGtCAeNoYKxk4hP7ePq -->
<!-- 2017-05-27T13:35:19 – 9feUkgiiUKkx6Uyz7Sbz -->
<!-- 2017-05-29T18:14:54 – 6ZfQDsNCZqGYYfE8Gk2h -->
<!-- 2017-06-04T20:14:00 – nrR0z0rT4H5MSn4rQNvG -->
<!-- 2017-06-10T09:50:17 – Fr9KXbeS5r5Tbd78eLuO -->
<!-- 2017-06-12T15:10:11 – 9YPJBKjhfluXViRB7KB6 -->
<!-- 2017-06-12T23:43:37 – c8x92LezEq4kcbRiYIdd -->
<!-- 2017-06-17T07:26:03 – paXHMx5rXNZQOkILvKec -->
<!-- 2017-06-17T17:09:10 – KhhpWpXT6U1vZvkuAYmr -->
<!-- 2017-06-18T08:58:05 – qOPt8g1yiyZe1OTdfGIj -->
<!-- 2017-06-19T18:43:27 – bB26jZBki6HBUDPEpDh3 -->
<!-- 2017-06-20T05:05:48 – MEaqvHo3cK7E7Pj5dU6Q -->
<!-- 2017-06-22T08:45:23 – lGhLPl4OVPKSOV9YqgO5 -->
<!-- 2017-06-23T00:13:34 – m6AdGTT1Ya6LYtkqs0XF -->
<!-- 2017-07-01T19:34:17 – 2UgqimgGCVgLOkp388Sr -->
<!-- 2017-07-02T10:39:50 – C0AJvsRWTHmJKw2UVkbo -->
<!-- 2017-07-02T16:50:11 – vrZ27qMpiEieevTw7L8r -->
<!-- 2017-07-09T02:57:34 – k4wDCtULL6guhLhx06Hl -->
<!-- 2017-07-15T07:37:55 – YrJMBV3uY6zt3ZGe6cTx -->
<!-- 2017-07-15T18:34:15 – nl3RW87F63vIcznJz5X8 -->
<!-- 2017-07-17T22:33:54 – jy5mJ4NEnuVCxSKqeDh8 -->
<!-- 2017-07-19T13:56:11 – rQ3A5DpqvuJfJFqEpfG3 -->
<!-- 2017-07-20T07:00:01 – ZgaIdZGfeRVwq0S9w55k -->
<!-- 2017-07-20T16:24:50 – eaf19CPNyk2ZXmECtOvd -->
<!-- 2017-07-23T04:25:17 – xuaZ9CFqHXmbvOVwpda0 -->
<!-- 2017-08-05T10:42:26 – qDn4NbhYsONUyRfLVn5S -->
<!-- 2017-08-07T15:25:43 – NiZJjmWHiP8Ql6oUpBr4 -->
<!-- 2017-08-08T06:22:44 – aEsdSrTjmuQyU3eLwe9Z -->
<!-- 2017-08-08T14:26:31 – JJrZmYr0zxp84vARAzk9 -->
<!-- 2017-08-09T21:13:10 – E271E5yxp1Hb4GZUj0fP -->
<!-- 2017-08-10T13:34:15 – m9yCBOSrwfFra9A2VkRs -->
<!-- 2017-08-11T00:24:35 – bsjZluzaGqVqVxQeIfuC -->
<!-- 2017-08-15T07:06:03 – zTQ8leCGw34JtFyMAHrA -->
<!-- 2017-08-17T01:38:02 – 1S9f00xRL3ZN2oJS0ZNx -->
<!-- 2017-08-22T07:51:29 – m6emfdIYYWIsQJMBSu8G -->
<!-- 2017-08-23T05:09:27 – n0gNaz7xFMPKGcn7H4a1 -->
<!-- 2017-08-25T01:04:48 – VUyEtETEVoThNdxk06Sm -->
<!-- 2017-08-26T13:25:24 – 6TsRsc272P8WvrDGdfjN -->
<!-- 2017-08-26T13:39:05 – dvxcP9NdiFCMGamnJRmT -->
<!-- 2017-08-27T09:11:09 – bqAbblkaZBCZxNhmDQSL -->
<!-- 2017-08-28T23:19:51 – Bo4juCdVBAg75vRqekDt -->
<!-- 2017-08-30T17:23:27 – 3RMUr57FIUaoV3f2XEjI -->
<!-- 2017-08-30T23:02:19 – u0C1sW0ynLia1kwIl4PW -->
<!-- 2017-09-06T14:17:23 – JDz1Swq64Z7a5cHsHIeF -->
<!-- 2017-09-06T19:40:08 – i3n9XuvA9SqGB7hMgDd0 -->
<!-- 2017-09-07T19:54:05 – NoRu1W4tNRGfyQ9EgPDN -->
<!-- 2017-09-08T07:50:35 – JIPIelg4jwo3YtOh94kQ -->
<!-- 2017-09-11T13:39:19 – vMUtyoPpWOKjeanjgvox -->
<!-- 2017-09-16T18:11:33 – foyvpny4uyGGmWSU9Ggx -->
<!-- 2017-09-17T07:02:20 – m3TGWCLvLhvZi8BNOKmz -->
<!-- 2017-09-22T21:35:59 – slErqM5sx1sG7Nj7CPs5 -->
<!-- 2017-09-23T20:39:33 – k15sXbmqcerzKmkr7qTJ -->
<!-- 2017-09-26T19:41:32 – V7Zo0FNRrRVhnXbNsPw2 -->
<!-- 2017-09-28T23:37:32 – vaFIXDYzXfjVIbLHcoG4 -->
<!-- 2017-10-01T20:49:25 – p4JmLbgLD93ykTUhr2DX -->
<!-- 2017-10-02T07:02:12 – sofVKyUZcncTJ288x9JX -->
<!-- 2017-10-03T07:57:24 – JYhzrfIkdMmqDfl3fN4L -->
<!-- 2017-10-03T08:50:13 – Ixg5aL0yM4oS3494bnQv -->
<!-- 2017-10-04T09:29:18 – JvYhDAL2D759b7cwkfGP -->
<!-- 2017-10-04T10:06:25 – 3Lax7VpYSzPKAH140vP0 -->
<!-- 2017-10-04T18:22:43 – mA0WOX7FowuADsT2R169 -->
<!-- 2017-10-06T10:40:26 – E5nzatev2P8PCJfs9DOl -->
<!-- 2017-10-07T07:12:38 – Sr9jbG0uKFy67Grz4x2o -->
<!-- 2017-10-07T08:31:28 – Wop6fw2sEqkTXaG3z1QH -->
<!-- 2017-10-11T07:40:34 – lukTxa6GJEOVurFgZ1YF -->
<!-- 2017-10-12T05:04:40 – jFlrsIokVD1b0BMQ3lkr -->
<!-- 2017-10-12T14:33:30 – F0HAkQtmTnkLMKESL2y3 -->
<!-- 2017-10-12T15:56:50 – vcNTHAU7JZ8zir59c0ZP -->
<!-- 2017-10-15T14:43:06 – ZD9q6M9GOASzZZxXp9Ol -->
<!-- 2017-10-16T05:37:19 – zUKgKRmKUJ4fXzhvQUbA -->
<!-- 2017-10-22T02:12:10 – OnFJx0cH6Eu4BiQzV91F -->
<!-- 2017-10-24T11:25:53 – 9UUkfxuBJSPMHBhFWJEL -->
<!-- 2017-10-25T15:56:38 – 0Hj09kMBicwRRhhu2IBF -->
<!-- 2017-10-30T23:19:29 – GtJZYkB0nXSZGDKXAw4y -->
<!-- 2017-11-01T18:56:54 – KFdiRqeSXd0A0jZeF0Uq -->
<!-- 2017-11-06T07:43:43 – lC2wWaFf5Y32imvhWDUM -->
<!-- 2017-11-09T20:52:51 – T8Udydv21gbyLdvEua40 -->
<!-- 2017-11-11T04:33:04 – 1A6kpoU68d0G8ulGIFfy -->
<!-- 2017-11-11T13:56:09 – kY5kOQUWBQDg6rC7Dz4r -->
<!-- 2017-11-16T10:54:18 – eExRnkQBVaaEGVHKqbF3 -->
<!-- 2017-11-19T19:40:26 – CJ8viSXOwwxdHJUgNYsk -->
<!-- 2017-11-24T05:44:18 – Z1cK9dCzjhCab06rek4b -->
<!-- 2017-11-24T17:10:50 – zYcgsCmBmF4s2DCjy4Hw -->
<!-- 2017-11-25T04:01:49 – GG26JNbs9PU19FA8Yt3Q -->
<!-- 2017-11-30T22:02:10 – wZKutehH7Sv5xaxMLAp8 -->
<!-- 2017-12-01T01:05:53 – X9gH54o7iYA16Hmr0ECA -->
<!-- 2017-12-06T06:35:27 – iDi80HPv392YnfZS7GQ5 -->
<!-- 2017-12-06T17:23:51 – bosnt4ndn03xcPZruycO -->
<!-- 2017-12-09T10:09:57 – MOx9QnqIwRMJ9Tdb7aBC -->
<!-- 2017-12-10T03:14:04 – NEBRXu1ZZkbcKuoKBsFV -->
<!-- 2017-12-17T00:42:13 – msL7iJRqTbOrcSbqwEQC -->
<!-- 2017-12-22T12:00:03 – ZiD0t1cNswcUPJVXXEDw -->
<!-- 2017-12-29T02:37:41 – Qp5r8mrKDbgJ6Ib5eW5K -->
<!-- 2017-12-30T00:05:46 – 76SDTiNYJa10JZOEWZAu -->
<!-- 2017-12-30T14:41:16 – miuNBqZUrDZC0B3vXugS -->
<!-- 2018-01-08T05:11:51 – YfGGVDxL5Ealxseratm6 -->
<!-- 2018-01-09T12:37:26 – liBlT0AT67ELhCOkoZtk -->
<!-- 2018-01-16T18:04:08 – 5WjrfoaLqSRelRVxf2zi -->
<!-- 2018-01-18T03:17:29 – KnOjL2KmPj4jX4m2AoW7 -->
<!-- 2018-01-19T20:11:37 – s07UFcXoo4WmiPqcjFZj -->
<!-- 2018-01-27T11:07:02 – aVAQgmmiitXhX3ZeUeHH -->
<!-- 2018-01-29T00:13:17 – kIN1K1sx8f82SKP2lq1o -->
<!-- 2018-01-31T05:09:52 – 5JZHWrOqB46qdMv0b6i4 -->
<!-- 2018-01-31T14:51:54 – SuYombbYuJ5wR8EJ8W7V -->
<!-- 2018-02-05T14:32:28 – OOGjpHCF6aLw8djpzE6U -->
<!-- 2018-02-08T06:44:44 – zYN2sVD1I989VvcATxSq -->
<!-- 2018-02-10T13:24:32 – JhZYXKC9Ecra9WLEouBS -->
<!-- 2018-02-18T15:56:34 – S7OzGOC8rvTdMcoBS8x7 -->
<!-- 2018-02-19T16:12:31 – 5lMvMRewn7beQPfmVCdS -->
<!-- 2018-02-21T16:33:07 – qSqA0bgbbK4qRwAE1i0p -->
<!-- 2018-02-28T20:21:25 – H2fFX7zX5Sev7O1G2IGg -->
<!-- 2018-03-05T16:44:19 – DpSpuq19S7h49pk5Oybt -->
<!-- 2018-03-10T05:52:10 – GuEzrzZR9faxJlDZMUv1 -->
<!-- 2018-03-10T22:16:35 – JB4KXyQ16RljixFO58hg -->
<!-- 2018-03-13T22:37:11 – f5sh65Kr2QxsK8ur4W7e -->
<!-- 2018-03-14T23:55:48 – xbarCpC4Q91bWcuzaTzz -->
<!-- 2018-03-16T09:06:40 – xFRiijmCROnYuGv0Qtp1 -->
<!-- 2018-03-16T16:46:50 – UyPV5gScvGI1bsyvkmfY -->
<!-- 2018-03-18T21:24:45 – 1Jn9muCJHs6WYqDwaBJR -->
<!-- 2018-03-23T21:41:25 – NO8imTpscw2PfmsuIx7z -->
<!-- 2018-03-26T22:13:39 – u1qE52UHbvOSkmGIkEKB -->
<!-- 2018-04-01T02:48:34 – bn7ewDi3sNzEHuo1GC96 -->
<!-- 2018-04-01T03:12:11 – bsuJTuMwsV29ovlM9cUh -->
<!-- 2018-04-01T18:09:21 – BgR1kwK80fLnJPio4bH7 -->
<!-- 2018-04-01T21:55:02 – BKK8IB6luGgF2N08qnRm -->
<!-- 2018-04-08T13:24:12 – Oe5wRuK7XkCarJieP4q4 -->
<!-- 2018-04-13T20:41:50 – WMd06n9PNsAakkAIOW6h -->
<!-- 2018-04-16T10:14:06 – KqSBsEfoo2oE1S6sLvII -->
<!-- 2018-04-20T01:32:25 – 2hmyqlRjMje6GEgp3kkp -->
<!-- 2018-04-25T06:47:30 – 0zTad38muonI0H04XiZ1 -->
<!-- 2018-04-28T07:42:03 – vQJndkqQULFIuVZhWB9W -->
<!-- 2018-05-03T03:07:47 – LxK73OqgyHoFLUaOdIbZ -->
<!-- 2018-05-03T10:41:23 – Wxp41TLIJE6yzFgeAXPd -->
<!-- 2018-05-04T08:47:45 – 0eEO5NCB9C87uTC7pYdS -->
<!-- 2018-05-06T06:53:25 – Axce9LRETcw8k45ODx2S -->
<!-- 2018-05-07T05:55:35 – 9WahqUR8mgAJ9O3nnk68 -->
<!-- 2018-05-14T04:03:18 – ui2pLhTYzvr93csZRYnL -->
<!-- 2018-05-16T06:41:47 – YFpMSDpethbdIyLbEW26 -->
<!-- 2018-05-16T23:12:35 – w55Q4RIKx86hHxnSI4i7 -->
<!-- 2018-05-18T07:54:49 – 6wyTLo2bFctV4GZETUPP -->
<!-- 2018-05-20T22:44:04 – K4qK1ndpTTjvDJmfScKV -->
<!-- 2018-05-21T06:58:17 – xVpLhKt3hgfxJUgQZSj6 -->
<!-- 2018-05-22T01:04:28 – 3rGeDrzBuaiXMCqGgkmt -->
<!-- 2018-05-22T15:38:14 – x3YD3BTzh0rMCePR2iba -->
<!-- 2018-05-23T03:34:42 – EsmzvpmCPdsowMTqIj7h -->
<!-- 2018-05-23T13:59:17 – cA10YZXjhZ39ccdPx51s -->
<!-- 2018-05-24T20:32:57 – eLICFP6Jw5yDe2paVf37 -->
<!-- 2018-06-05T18:37:16 – dO3A20mECBLbluSIIEE9 -->
<!-- 2018-06-08T02:36:35 – DK6XLUAWwxRhEbp5dStN -->
<!-- 2018-06-09T23:54:53 – dlyWjpctvI3uw5Nzq7Fd -->
<!-- 2018-06-10T02:05:23 – UJXNPXU7bmrZV7Wuvy1E -->
<!-- 2018-06-10T09:22:12 – IYpcYD2tweVycVEe00Fy -->
<!-- 2018-06-10T18:58:16 – rV9RbRiPjaQkhcdeQcl2 -->
<!-- 2018-06-12T17:18:49 – BNXeN6pyxj9hiQaWKeik -->
<!-- 2018-06-13T01:48:17 – 2qJ1EKFiWT8HKdz50nMu -->
<!-- 2018-06-13T10:36:10 – LeqsucsFqbZSQtBL2gXY -->
<!-- 2018-06-15T16:34:02 – 1gIqxrBpruBgdLOPuK34 -->
<!-- 2018-06-15T18:21:59 – BWj0R0qGWkzj91NwnUa0 -->
<!-- 2018-06-17T16:14:28 – RE671sGrFxaS76Z9NIqb -->
<!-- 2018-06-21T18:45:03 – 4Hu8CQrS6iMDNsisfNfl -->
<!-- 2018-06-24T13:42:54 – 4vnKc8YMpAijisWUe831 -->
<!-- 2018-06-25T01:51:32 – aTfzOKWsPyws5hBjdEvt -->
<!-- 2018-06-26T06:53:27 – b8GtUkUfSYLRJjstZK5e -->
<!-- 2018-07-01T21:16:10 – HdmkD0SF5grWaTCc54Fl -->
<!-- 2018-07-04T14:51:25 – xiD6l9gjjMyp06N65foU -->
<!-- 2018-07-13T21:19:03 – CkL0QI8KYfX0xAWyv7E6 -->
<!-- 2018-07-20T01:15:04 – iD2fb2dn3Qw4B3tbUs59 -->
<!-- 2018-07-20T21:03:20 – NfyrT2oIvsYT7OKPVt16 -->
<!-- 2018-07-23T02:59:07 – UdZS2ssBoMCO882OlVo0 -->
<!-- 2018-07-28T07:29:23 – T80vZYs5eCMAqMCbrfOG -->
<!-- 2018-07-29T01:27:48 – DoKCrkUZanNuiBytW3Pb -->
<!-- 2018-08-03T03:09:21 – Ua3OB6ZQ5Rl4aqaf0vZF -->
<!-- 2018-08-04T09:13:56 – 61A8aC3AFqOnDAUHepVa -->
<!-- 2018-08-10T22:55:42 – lUYuQhSvZ7uXlsxheJYW -->
<!-- 2018-08-14T07:18:34 – yRFfnitVaMP2ZQS33zlN -->
<!-- 2018-08-19T17:47:23 – WBBk1iGxtbaSgpXC0cr0 -->
<!-- 2018-08-19T19:30:37 – DNZbXS2cDV3414OxQEVC -->
<!-- 2018-08-22T17:01:37 – jEi3fGT80QaRNiKwTTuA -->
<!-- 2018-08-26T05:02:14 – iCrayAEsfTm3ORq0ywDo -->
<!-- 2018-08-31T19:07:39 – dWSBdtQOvEO0pi52nIAa -->
<!-- 2018-09-04T20:53:47 – qqcVSlflzKTYdBfvsksj -->
<!-- 2018-09-05T18:00:49 – EkTQQSTejZuF7YO1YQv9 -->
<!-- 2018-09-06T14:55:46 – rRZuU8kooIQB36XgTAgC -->
<!-- 2018-09-11T15:27:18 – JSIUDUiK2cBdQ8lG6dzJ -->
<!-- 2018-09-13T02:42:58 – 1IvB8Hi58iig1eE7oyVk -->
<!-- 2018-09-15T09:50:01 – D5wwsDTLZcEI9pK2Gj0l -->
<!-- 2018-09-19T14:19:38 – GY4Hqg6ReTQbKH08Wgjo -->
<!-- 2018-09-20T17:59:11 – 0obS71j6I28naBaXLd7c -->
<!-- 2018-09-21T04:09:36 – rhClEsEpRIni6Tznc2V6 -->
<!-- 2018-09-22T21:04:02 – POwZqwYdEvgOUzwKIRUH -->
<!-- 2018-09-27T15:37:55 – rkIcs47oo18itp6HM1T0 -->
<!-- 2018-09-30T18:40:19 – knR9ZH9RhaPL5A7OPBtw -->
<!-- 2018-10-02T18:21:17 – dNGIwliwfngBJL07n3Wq -->
<!-- 2018-10-02T19:06:51 – gbo2AFty3TWq9O60ngky -->
<!-- 2018-10-06T02:08:11 – DaiNAqNwRhg8hWBmE1lT -->
<!-- 2018-10-07T06:49:10 – VI3gD6lYDvuG5JxCSQAO -->
<!-- 2018-10-12T00:17:58 – D8KtdVGzSapeijfmctbR -->
<!-- 2018-10-15T14:06:02 – DEaNzsbA4oDsJHiqA301 -->
<!-- 2018-10-21T04:22:25 – S81l2TrX2QQ5c0TOgkik -->
<!-- 2018-10-21T21:44:51 – 8mmUBP0wNqZzZ4JGbQXY -->
<!-- 2018-10-23T02:58:56 – MWp2WGBoMV900x34TpIO -->
<!-- 2018-10-23T20:04:47 – 7QDtQ9Pydknf4Etbgp5n -->
<!-- 2018-11-01T00:02:13 – PjPybRnGjnsZTbxxq7ic -->
<!-- 2018-11-02T15:35:34 – 7qlqgZYqR5JnRAfQqjvl -->
<!-- 2018-11-04T02:58:16 – 9yx3Xma3rv0JhHw3b5Sx -->
<!-- 2018-11-10T01:01:33 – XqhTvZvgNxuIYdK4DoIu -->
<!-- 2018-11-13T03:54:08 – ldNo4ABohq0H7gFUdXjc -->
<!-- 2018-11-14T00:23:29 – qXx96VY0RPFIXWMZq0Nc -->
<!-- 2018-11-15T06:26:38 – aeGafQHA63cSVcagVvAc -->
<!-- 2018-11-18T03:14:10 – 2uuKSZhdhY6Ll1rzVAwg -->
<!-- 2018-11-26T16:50:01 – kNpmW8a8dFEiwRPbinUb -->
<!-- 2018-12-04T12:33:18 – fJ0gioASRuKERzNc4SRt -->
<!-- 2018-12-08T02:00:33 – 9RAkLTeOBVoxQiTIa5fg -->
<!-- 2018-12-17T10:10:17 – KLsrqQQCbmiI5sJI3AhR -->
<!-- 2018-12-28T09:33:01 – ahxjTe0Te6u1mJWL6oNE -->
<!-- 2018-12-28T16:36:43 – qvavpboL2OLACeG878Da -->
<!-- 2018-12-29T05:51:16 – N1MrNhG5rNbzdrX4XfQ8 -->
<!-- 2018-12-29T15:37:26 – WKUm1cwdcTZ69xfwOuDs -->
<!-- 2018-12-30T14:50:52 – JXiLIdVVkggzq5EFxojM -->
<!-- 2018-12-31T11:08:16 – NgHen2DycsPccoxn8buG -->
<!-- 2019-01-05T19:14:43 – 3LD7nO6UHek6dmDWvziV -->
<!-- 2019-01-10T11:37:57 – jbf9XJ27iejrkenbIzQm -->
<!-- 2019-01-10T22:32:28 – PTb1ThxHkNpcnk8Lx1Ge -->
