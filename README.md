# Method for Eulerswap Liquidity Optimization

![Project Banner](https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_AMM.png)
<video src="img/data_viz/USD_DAI_cex_1_minute.mp4" controls width="500"></video>
## Objective
What should we set our liquidity parameters on Eulerswap to? We study the oldest AMM stablecoin pools on Uniswap to answer this question and focus on the biggest stablecoins: USDC, USDT, DAI.
## Description 
Method for optimizing liquidity parameters for Eulerswap with primary focus on stablecoins.
- Eulerswap invariant analysis of LP Payoff, price impact, liquidity allocation across price space.
-   -  
- Statistical tools for analyzing stablecoin pool behavior
-     -
-     -
- 
- Tool for optimizing Eulerswap liquidity parameters based on one's forecast or historical data.
- Approximation of Eulerswap with discrete LP positions to exit 100% out of a token.


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

Further research section:
liquidity flows , inverse fft

## Data Retrieval

### Cryo
Instructions for retrieving data using Cryo:

```bash
# Example Cryo command
cryo fetch --source eulerswap --output data/
```

### SQL

Instructions for retrieving data using SQL:

```sql
SELECT * FROM liquidity_pools
WHERE protocol = 'Eulerswap';
```

## Images

<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_liquidity_distributions.jpg" alt="Sample Image 1" width="1000"/>




<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/img_USDC_USDT_TAIL.png?raw=true" alt="Sample Image 1" width="1000"/>
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/USDC_USDT_Histogram.png?raw=true" alt="Stats" width="1000"/>


<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Stablecoin_Frequencies.png?raw=true" alt="Sample Image 1" width="1000"/>
From our previous work on Uniswap pools we also observe patterns linked to NYSE liquidity and bot activity at UTC-00:00
<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/uniswap_trade_activity.jpeg?raw=true" alt="Sample Image 1" width="1000"/>



## License



MIT



