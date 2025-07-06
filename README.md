# Method for Eulerswap Liquidity Optimization
![Project Banner](https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/data_viz/USDC_USDT_PARAMETERS.png?raw=true)

## What should we set Eulerswap liquidity parameters to?
We study the oldest AMM stablecoin pools on Uniswap to answer this question and focus on the biggest stablecoins: USDC, USDT, DAI.

## Description 

Method for optimizing liquidity parameters for Eulerswap with primary focus on stablecoins.
- Eulerswap invariant analysis of LP Payoff, price impact, liquidity allocation across price space ([interactive visualization](https://www.desmos.com/calculator/8f6bcdcb41))
  - Subitem 1.1
  - Subitem 1.2
- Statistical tools for analyzing stablecoin pool behavior
  - Subitem 1.1
  - Subitem 1.2
- Tool for optimizing Eulerswap liquidity parameters based on one's forecast or historical data.
- Approximation of Eulerswap with discrete LP positions to exit 100% out of a token.
  - If one is interested in LPing in non-stablecoin pools there is also a delta-neutral optimization available in the file.
  - Method for having a hard cutoff for Eulerswap with univ3 [url desmos](https://www.desmos.com/calculator/02c7569f86)


# Eulerswap AMM
Overview
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_AMM.png" alt="Sample Image 1" width="1000"/>

Liquidity fingerprint is and can be confirmed with desmos
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_liquidity_distributions.jpg" alt="Sample Image 1" width="1000"/>

We can fit 
fit previous historic AMMs with the most frequently traded stablecoin pairs.

*image of gamma* One can specify one's own risk profile - manage "c" based on one's preference.
How can we fit the distribution of 

- how to spot a depeg - sources: general market liquidity (point gordon liao study) and its flow to stables, point out liquidity changes in past AMM uni v3 heatmap.
-does depeg continue? What is the Hurst exponent? What is the autocorrelation?
-what to do when depeg - set lower c to univ2
-point out tails of depeg


very basic strat buy above 1 wait for 10 min, if not 1 sell - potential future direction - take the heatmap, express it as a matrix, use neural network to train it on the value of c!

Alternative method: fit the value c to the tick data from uniswap v3 pool as well

Why USDT/DAI may be best pool, the constant mint and burn of the stablecoin will cause changes in DAI price (*thereby generating  more in fees!!), which would counter the stale price of USDT or USDC which may deviate and generate fees more in times of unforeseen crises rather than simply due to mint/burn mechanisms of DAI-like stablecoins.

Method for having a hard cutoff for Eulerswap with univ3 https://www.desmos.com/calculator/02c7569f86

An important thing to note is anti-correlation, a rapid up move in a block can expect the next block to be a rapid down move.

-----

side not on tails - alpha 3-4 for erc-20 CEX non stables 2-4 for stables CEX, and apparently <2 for stables on DEX!

examine stablecoins as it is a growing segment of defi - focus on largest three stablecoins USDC, USDC, DAI

FURTHER RESEARCH:

In our case we fit the Eulerswap parameters based on empirical observation using as much data as possible.

Cases of volatility lasting: windowed fourier transform, autocorrelation negative, hurst exponent below 0.5

For example, just as the procedure we used above by extracting the empirical price movement of the pool, 
we can extract the overall historic liquidity distribution, and simply mimic the behavior - such an approach would mean though that we would be at least two blocks behind (one for reading the data of current liquidity block and one for adjusting eulerswap parameters to fit the liquidity distribution on the next block) 


CEX data vs DEX data over same time interval
point out stablecoin arbitrage and strategies possible 



## Installation
To set up the project, follow these steps:

```bash
# Clone the repository
git clone https://github.com/your-username/eulerswap-liquidity-optimization.git
cd eulerswap-liquidity-optimization
```

## Further Optimization

optimize c_1 and c_2 parameters for eulerswap pool
dl full eth node
extract usdc/usdt block sqrtpricex96 from univ3
statistical fit in python for liquidity fingerprint of eulerswap for c_1 and c_2 

[Watch the demo video](img/data_viz/USD_DAI_cex_1_minute.mp4)


## Data Retrieval

### Cryo
Instructions for retrieving data using Cryo:

```bash
cryo logs \
--rpc API_KEY \
--contract 0x5777d92f208679DB4b9778590Fa3CAB3aC9e2168 \
--event-signature "Mint(address sender, address indexed owner, int24 indexed tickLower, int24 indexed tickUpper, uint128 amount, int256 amount0, int256 amount1)" \
--event-signature "Burn(address indexed owner, int24 indexed tickLower, int24 indexed tickUpper, uint128 amount, int256 amount0, int256 amount1)" \
--blocks 12376729:12377729 \
--output-dir ./Python/data_cryo
```

### SQL

Instructions for retrieving data using SQL:

```sql
SELECT *
FROM uniswap_v3_ethereum.Pair_evt_Swap
WHERE contract_address = 0x48DA0965ab2d2cbf1C17C09cFB5Cbe67Ad5B1406
  AND evt_block_time >= DATE '2022-01-01'
  AND evt_block_time <= DATE '2023-01-01'

curl -H "X-Dune-API-Key:_____________________" "https://api.dune.com/api/v1/query/5365625/results/csv?limit=15000" > dune_output2023_2022_dai_usdt.csv
```

## Images




<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/img_USDC_USDT_TAIL.png?raw=true" alt="Sample Image 1" width="1000"/>
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/USDC_USDT_Histogram.png?raw=true" alt="Stats" width="1000"/>


<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Stablecoin_Frequencies.png?raw=true" alt="Sample Image 1" width="1000"/>
From our previous work on Uniswap pools we also observe patterns linked to NYSE liquidity and bot activity at UTC-00:00
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/uniswap_trade_activity.jpeg?raw=true" alt="Sample Image 1" width="1000"/>



## License



MIT



