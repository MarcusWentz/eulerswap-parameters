# Method for Eulerswap Liquidity Optimization
![Project Banner](https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/eulerswap_pool_parameters.png?raw=true)

## What should we set Eulerswap liquidity parameters to?
We provide a method for selecting parameters by studying the oldest AMM stablecoin pools on Uniswap to answer this question and focus on the biggest stablecoins for Euler: USDC, USDT, DAI.

## Description 

Method for optimizing liquidity parameters for Eulerswap with primary focus on stablecoins to get the best capital efficiency.
- Eulerswap AMM and its liquidity distribution.
- Example with USDT/USDC.
  - Stablecoin Pool Tail Analysis 
  - Stablecoin Pool Tail Fit
  - Eulerswap Parameter Fit Tool
- Further Potential Optimization Directions
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
  - 1B. Liquidity provider's payoff / value function to see divergence loss and sensitivity with Greeks in [desmos](https://www.desmos.com/calculator/8f6bcdcb41).
  - 1C. Liquidity fingerprint to see where liquidity is provided to capture trades.
  - 1D. Price impact function to see how concentration affects pool price change.

We are mainly focused on the liquidity fingerprint in Figure 1C since we can use it to optimize the fit for our empirical observations of stablecoin price behavoir. 
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_AMM.png" alt="Sample Image 1" width="1000"/>

The liquidity fingerprint L(x) in price space with scale S and parameters c can be derived with a little bit of calculus and can be confirmed with [desmos](https://www.desmos.com/calculator/8f6bcdcb41) to be the following equation.

<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_liquidity_distributions.jpg" alt="Sample Image 1" width="1000"/>

We can fit the Eulerswap liquidity fingerprint to previous historic data of stablecoin price movement. In our case we fit the Eulerswap parameters based on empirical observation of the following stablecoin pool addresses using as much onchain data as possible for the past five years with data backed on our [Google drive](https://drive.google.com/drive/folders/1AzrlKZApBz60PD6itQ5ry6MnSiqOLOAe?usp=sharing):

```
    USDC_DAI_0x5777d92f208679DB4b9778590Fa3CAB3aC9e2168
    USDT_DAI_0x48da0965ab2d2cbf1c17c09cfb5cbe67ad5b1406
    USDC_USDT_0x3416cF6C708Da44DB2624D63ea0AAef7113527C6 
```

We focus in this section just on the USDC/USDT pair below, but the other pool data output are also available in the img folder.

## Stablecoin Pool Tail Analysis

We look at the sqrtpricex96 of USDC/USDT and adjust the decimals in plot A to normalize it at $1. The histogram in B confirms asymmetry in our pool with symmetric fat tailed distributions not being a good fit meaning we have to have separate parametrs for c_1 and c_2. Plots C and D confirm asymmetry in the tails. Plots E and F examine the slope of the returns as one descends away from the peak and require the [power law](https://pypi.org/project/powerlaw/) library. Plot F also tells us which expected moments have any significance or not. In our case we get some startling fat tailed results.
```python
  pip install powerlaw
```
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/USDC_USDT_Histogram.png?raw=true" alt="Stats" width="1000"/>

## Tail fit
We fit two different distributions (power law (Pareto) and stretched exponential) and find that the power law is statistically significant both in its tail of alpha=1.62 for both tails (indicating a distribution so fat that the mean is undefined, but the median is) and also find that the Pareto distribution is the better fit over the stretched exponential. This alpha tail result is surprising, considering how stablecoins on CEX have alpha ranging between 2 and 4 from our previous research.

<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/img_USDC_USDT_TAIL.png?raw=true" alt="Sample Image 1" width="1000"/>

## Eulerswap Parameter optimization

From our tail fit alpha value we have determined that we can only use the median as the central point for our liquidity concentration (mean and standard deviation are undefined). We now fit the empirical data using scipy, if it fails, we default to least squared approach:

<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/data_viz/USDC_USDT_PARAMETERS.png?raw=true"  width="1000"/>

Our liquidity optimization tool tell us to structure our peak at the median of $0.999866 and for the left tail a c_down=0.9999999847 with much more liquidity S_down than S_up which has a c_up=0.9996806442.

## Future Potential and Further Optimization

We have used this method to optimize liquidity for three stablecoin pairs (USDT/USDC, USDC/DAI, USDT/DAI) with Eulerswap, but the future potential of this method can extend to all pairs (doing so would require downloading and running an entire archival node with cryo with a good connection, which we are currently working on).

Additionally, we ran out of time, but our method can be further improved for these parameters given the following discoveries during our work:

### Phase Space Neural Network Case
While we were looking at the phase space behavoir across time, we notice asymmetries like prices between $[1...1.05] do not stay there for more than 100 blocks so concentration parameters can be adjusted for the right tail. The potential future direction here is to actually express the phase space heatmap for each lag and train it to a corresponding c value and then feed it new price data for a dynamic adjustment of concentration parameters.
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/phases_example.jpeg?raw=true" alt="Stats" width="1000"/>
t+5 (5 blocks equivalent to 1 minute notice the prices above $1 on the diagonal)
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/data_viz/USDC_USDT_phase_space_block1.jpg?raw=true" alt="Stats" width="1000"/>
t+300 (300 blocks equivalent to 1 hour as time goes on the moves away from $1 converge to a cross)
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/data_viz/USDC_USDT_phase_space_block300.jpg?raw=true" alt="Stats" width="1000"/>
[We have a video in the /img folder and Google Drive that shows this behavior across time](img/data_viz/USDC_USDT_phase_space_video.mp4)

### Uniswap Liquidity Distribution Case

Alternatively, instead of training on past data, one can tap into the wisdom of the crowd on a uniswap pool by extracting the liquidity distribution. Such an approach would mean though that we would be at least two blocks behind (one for reading the data of current liquidity block and one for adjusting Eulerswap parameters to fit the liquidity distribution on the next block). This is a lagging approach.

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


### Time Optimization Refinement

We can further decompose the data across time to see if we can pick any insights for adjusting liquidity.

If stablecoin data were truly random, then the Hurst exponent would be flat, the autocorrelation would be flat, the periodogram (which uses Euler's formula during the Fourier transform decomposition btw!) would also be flat, and the spectrogram across time would resemble white noise across time. 

Instead we see Hurst exponents for our stablecoin pools below 0.5 indicating anti-correlation, the autocorrelation plot further confirms this (negative correlation on the next block, followed by dampening), the periodogram highlights periodic oscillations in the USDT/DAI pool, indicating low frequency oscillations in price moves also visible with equal spacings in red in the spectrogram. The spectrogram also shows us clear vertical columns instead of randomness which pinpoints to volatility clustering in the stablecoin pools. 

All of this points to the fact that there are still signals just from the price movement that can be extracted to further optimize Eulerswap parameters across time.
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Stablecoin_Frequencies.png?raw=true" alt="Sample Image 1" width="1000"/>

For example, from our previous work on Uniswap v3 pools at ETHNY we also observed patterns linked to NYSE liquidity and bot activity at UTC-00:00, so one could optimize Eulerswap parameters across daily cycles. For example, the data can be further refined by looking at the blocks prior to NYSE open and UTC 0:00 noting that if prices start to move in US markets negatively, they will impact most likely crypto negatively as shown by the work of [Gordon Laio here](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4910537), but stablecoins would then have a positive rise in liquidity, which would lead to an adjustment of the c_1 and c_2 parameters.
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/uniswap_trade_activity.jpeg?raw=true" alt="Sample Image 1" width="1000"/>



## Data Retrieval

Data gathered for the following stablecoin pool addresses:

```
    USDC_DAI_0x5777d92f208679DB4b9778590Fa3CAB3aC9e2168
    USDT_DAI_0x48da0965ab2d2cbf1c17c09cfb5cbe67ad5b1406
    USDC_USDT_0x3416cF6C708Da44DB2624D63ea0AAef7113527C6  
```

There are two approaches to data retrieval: Cryo with RPC, Cryo with home node, Dune SQL query.

### Cryo
Instructions for retrieving data using Cryo with RPC and home node:

```bash
cryo logs --rpc https://eth.drpc.org --contract 0x88e6A0c2dDD26FEEb64F039a2c41296FcB3f5640 --event-signature "Swap(address indexed sender, address indexed recipient, int256 amount0, int256 amount1, uint160 sqrtPriceX96, uint128 liquidity, int24 tick)" --topic0 0xc42079f94a6350d7e6235f29174924f928cc2ac818eb64fed8004e115fbcca67 --blocks 18050000:18060000 --requests-per-second 10 --max-concurrent-requests 5 --output-dir ./Python/data_cryo
```

### SQL

Instructions for retrieving data using SQL Dune query (create a free account, insert API KEY and copy API dune query url):

```sql
SELECT *
FROM uniswap_v3_ethereum.Pair_evt_Swap
WHERE contract_address = 0x3416cF6C708Da44DB2624D63ea0AAef7113527C6  
  AND evt_block_time >= DATE '2022-01-01' 
  AND evt_block_time <= DATE '2023-01-01'

curl -H "X-Dune-API-Key:_____________________" "https://api.dune.com/api/v1/query/xxxxxx/results/csv?limit=15000" > dune_output2023_2022_dai_usdt.csv
```



