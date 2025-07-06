# Eulerswap Liquidity Optimization

![Project Banner]([https://via.placeholder.com/800x200.png?text=Eulerswap+Liquidity+Optimization](https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_AMM.png))

## Description
Method for optimizing liquidity parameters for Eulerswap with primary focus on stablecoins.

## Features
- Eulerswap invariant analysis of LP Payoff, price impact, liquidity allocation across price space.
- Statistical tools for analyzing stablecoin pool behavior
- Tool for optimizing Eulerswap liquidity parameters based on one's forecast or historical data.


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

<img src="https://github.com/MarcusWentz/eulerswap-parameters/blob/main/img/Eulerswap_AMM.png" alt="Sample Image 1" width="400"/>
## License

MIT



