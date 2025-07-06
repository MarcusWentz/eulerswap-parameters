# Method for Eulerswap Liquidity Optimization
![Project Banner](https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/eulerswap_pool_parameters.png?raw=true)

## What should we set Eulerswap liquidity parameters to?
We provide a method for selecting parameters by studying the oldest AMM stablecoin pools on Uniswap to answer this question and focus on the biggest stablecoins: USDC, USDT, DAI.

## Description 

Method of for optimizing liquidity parameters for Eulerswap with primary focus on stablecoins to get the best capital efficiency.
- Eulerswap AMM and its liquidity distribution.
- Example with USDT/USDC.
  - Stablecoin Pool Tail Analysis 
  - Stablecoin Pool Tail Fit
  - Eulerswap Parameter Fit Tool
- Further Optimization Directions
  - Uniswap Liquidity Distribution
  - Phase Space Neural Network
  - Time-based Concentration
- Approximation of Eulerswap with discrete LP positions to exit 100% out of a token.
  - Method for having a hard cutoff for Eulerswap with univ3 [url desmos](https://www.desmos.com/calculator/02c7569f86)
  - If one is interested in LPing in non-stablecoin pools there is also a delta-neutral optimization available in the file.


# Eulerswap AMM
### Overview

Eulerswap is a piece-wise AMM and can be dissected into two functions (red and blue) along four frameworks:
  - 1A. Reserve space to see invariant / trading function (best viewed in log-log).
  - 1B. Liquidity provider's payoff / value function to see divergence loss and curvature sensitivity with Greeks in [desmos](https://www.desmos.com/calculator/8f6bcdcb41).
  - 1C. Liquidity fingerprint to see where liquidity is provided to capture trades.
  - 1D. Price impact function to see how concentration affects pool price change.

We are mainly focused on the liquidity fingerprint in Figure 1C since we can use it to optimize the fit for our empirical observations of stablecoin price behavoir. An interactive version of Eulerswap is available in [interactive desmos url](https://www.desmos.com/calculator/8f6bcdcb41).
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_AMM.png" alt="Sample Image 1" width="1000"/>

The liquidity fingerprint can be derived with a little bit of calculus and can be confirmed with [desmos](https://www.desmos.com/calculator/8f6bcdcb41) to be the following equation.

<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_liquidity_distributions.jpg" alt="Sample Image 1" width="1000"/>

We can fit the Eulerswap liquidity fingerprint to previous historic AMMs with the most frequently traded stablecoin pairs to maximize capital efficiency.

### Overview
Why USDT/DAI may be best pool, the constant mint and burn of the stablecoin will cause changes in DAI price (*thereby generating  more in fees!!), which would counter the stale price of USDT or USDC which may deviate and generate fees more in times of unforeseen crises rather than simply due to mint/burn mechanisms of DAI-like stablecoins.

An important thing to note is anti-correlation, a rapid up move in a block can expect the next block to be a rapid down move.

## Installation
To set up the project, follow these steps:

```bash
# Clone the repository
git clone https://github.com/your-username/eulerswap-liquidity-optimization.git
cd eulerswap-liquidity-optimization
```




## Data Retrieval

In our case we fit the Eulerswap parameters based on empirical observation of the following stablecoin pool addresses using as much data as possible:

```
    USDC_DAI_0x5777d92f208679DB4b9778590Fa3CAB3aC9e2168
    USDT_DAI_0x48da0965ab2d2cbf1c17c09cfb5cbe67ad5b1406
    USDC_USDT_0x3416cF6C708Da44DB2624D63ea0AAef7113527C6  
```


### Cryo
Instructions for retrieving data using Cryo:

```bash
cryo logs --rpc https://eth.drpc.org --contract 0x88e6A0c2dDD26FEEb64F039a2c41296FcB3f5640 --event-signature "Swap(address indexed sender, address indexed recipient, int256 amount0, int256 amount1, uint160 sqrtPriceX96, uint128 liquidity, int24 tick)" --topic0 0xc42079f94a6350d7e6235f29174924f928cc2ac818eb64fed8004e115fbcca67 --blocks 18050000:18060000 --requests-per-second 10 --max-concurrent-requests 5 --output-dir ./Python/data_cryo
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


## Tail analysis
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/USDC_USDT_Histogram.png?raw=true" alt="Stats" width="1000"/>
## Tail fit


side not on tails - alpha 3-4 for erc-20 CEX non stables 2-4 for stables CEX, and apparently <2 for stables on DEX!

CEX data vs DEX data over same time interval
point out stablecoin arbitrage and strategies possible because alphas don't match


<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/img_USDC_USDT_TAIL.png?raw=true" alt="Sample Image 1" width="1000"/>
## Conclusion optimization

https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/eulerswap_pool_parameters.png?raw=true






## Further Optimization



- how to spot a depeg - sources: general market liquidity (point gordon liao study) and its flow to stables, point out liquidity changes in past AMM uni v3 heatmap.
-does depeg continue? What is the Hurst exponent? What is the autocorrelation?
-what to do when depeg - set lower c to univ2
-point out tails of depeg


### Phase Space Neural Network
very basic strat buy above 1 wait for 10 min, if not 1 sell - potential future direction - take the heatmap, express it as a matrix, use neural network to train it on the value of c!

extract usdc/usdt block sqrtpricex96 from univ3

t+5 (5 blocks equivalent to 1 minute)
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/data_viz/USDC_USDT_phase_space_block1.jpg?raw=true" alt="Stats" width="1000"/>
t+300 (300 blocks equivalent to 1 hour)
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/data_viz/USDC_USDT_phase_space_block300.jpg?raw=true" alt="Stats" width="1000"/>
[Watch the demo video](img/data_viz/USD_DAI_cex_1_minute.mp4)


### Uniswap Liquidity Distribution Chase

Alternative method: fit the value c to the tick data from uniswap v3 pool as well

Alternative optimization - histogram from liquidity of univ3 - wisdom of the crowd with cryo:
Instructions for retrieving data using Cryo:

### Time optimization

Cases of volatility lasting: windowed fourier transform, autocorrelation negative, hurst exponent below 0.5

For example, just as the procedure we used above by extracting the empirical price movement of the pool, 
we can extract the overall historic liquidity distribution, and simply mimic the behavior - such an approach would mean though that we would be at least two blocks behind (one for reading the data of current liquidity block and one for adjusting eulerswap parameters to fit the liquidity distribution on the next block) 


```bash
cryo logs \
--rpc API_KEY \
--contract 0x5777d92f208679DB4b9778590Fa3CAB3aC9e2168 \
--event-signature "Mint(address sender, address indexed owner, int24 indexed tickLower, int24 indexed tickUpper, uint128 amount, int256 amount0, int256 amount1)" \
--event-signature "Burn(address indexed owner, int24 indexed tickLower, int24 indexed tickUpper, uint128 amount, int256 amount0, int256 amount1)" \
--blocks 12376729:12377729 \
--output-dir ./Python/data_cryo
```

If truly random, then the spectrogram would give us random noise with no patterns, yet we see vertical columns, mention red sinusoidal pattern
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Stablecoin_Frequencies.png?raw=true" alt="Sample Image 1" width="1000"/>

From our previous work on Uniswap pools we also observe patterns linked to NYSE liquidity and bot activity at UTC-00:00, so one could optimize eulerswap parameters not just across the price space, but also time spectrum.
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/uniswap_trade_activity.jpeg?raw=true" alt="Sample Image 1" width="1000"/>
