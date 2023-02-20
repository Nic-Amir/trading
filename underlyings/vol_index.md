# Volatility Index

## Description
The Volatility Index simulates the financial markets with constant volatilities and without any drift. The Volatility Index can be generated every 1 second or 2 seconds. Volatility Index with (1s) meaning they are generated 1 tick in 1 second.


## Behaviour
These indices correspond to simulated financial markets with constant volatilities of 10%, 25%, 50%, 75%, 100%, 200%,and 300%. The benefits of these indices are:

1. Allow the clients to trade under a simulated market with exact volatility within their expectation, whereas the volatility in financial market is dynamic and subject to economic factors, market sentiments etc.
2. Provide 24/7 liquidity to the clients.


## Construction
The Volatility Index is generated based on Geometric Brownian Motion without any drift.

$$
S_t = S_{t-1}\exp \left( -\frac{\sigma^2}{2} dt + \sigma\sqrt{dt} \cdot X \right)
$$

where

* $S_{t-1}$ = spot price at time $t-1$;

* $S_t$ = spot price at time $t$;

* $\sigma$ = flat volatility depending on the setting of the volatility index, e.g. $\sigma$ = 75% for volatility 75;

* $dt$ = 1 second or 2 second in year fraction, i.e. $dt = \frac{1}{(365 \cdot 24 \cdot 60 \cdot 60)}$ for 1 second;

* $X \sim \mathcal{N}(0,1)$ = sample from a standard normal distribution;


## Offerings

| Index 				|   1 second	|  2 seconds	|
|-----------		|	:--------:	| :---:				|
| Volatility 10 |:heavy_check_mark:|:heavy_check_mark:| 
| Volatility 25 |:heavy_check_mark:|:heavy_check_mark:| 
| Volatility 50 |:heavy_check_mark:|:heavy_check_mark:| 
| Volatility 75 |:heavy_check_mark:|:heavy_check_mark:| 
| Volatility 100 |:heavy_check_mark:|:heavy_check_mark:| 
| Volatility 200 |:heavy_check_mark:|:x:| 
| Volatility 300 |:heavy_check_mark: |:x:| 

For more details, please refers to the main table [here](https://wikijs.deriv.cloud/en/Trading/Model-Validation_Engineering/ModelValidation/Indices/offerings-underlying-table).

## Trading Condition

Here summarize how the trading conditions are determined.

### MT5 Spread

Firstly we calculate the expected price change in the next 2 seconds. Then the MT5 spread is determined around the expected price change.

$$\text{expected price change} = \text{current price} \times \text{volatility} \times \sqrt{dt} $$

Use volatility 75 index (2s) as an example, the expected range is calculated as follow:

$$\text{expected price change} = 551266 \times 75\% \times \sqrt{\frac{2}{86400*365}} = 104.12$$

Based on this, the MT5 spread is set at 115, which is around 110% of the expected price change. Note that the MT5 spread will be reviewed from time to time given that the current price is changing too.

The spread will be converted into spread points depending on each symbol digit specification. For this example, the spread point is 11500 as the symbol digit is configured as 2. 

There might be mark-up spread on top of this MT5 spread due to various business decisions such as extra mark-up for high-risk clients.

The bid-ask spread is the summation of MT5 spread + mark-up spread. 

Latest condition : https://docs.google.com/spreadsheets/d/1M92ZIFpImvJ68EemeXiZBBoSVnpfDEH1/edit#gid=865395382

### MT5 Swap Rate

Swap Rate is determined as follow 

$$\text{swap rate} = \text{volatility} \times 10\% $$

Swap fee is charged when a client holds the position overnight. Use volatility 75 index (2s) as an example, the amount a client will be charged is :

$$\text{swap fee in dollar} = \text{volume in lot} \times \text{current price} \times \frac{75\%}{365}$$


### MT5 Lot Size

#### Min Lot Size
Min lot size is defined to have IB’s commission at least $0.01 for min lot. 

#### Max Lot Size
Max lot size is set to make volatility-adjusted USD Volume approximately equal across all Synthetic assets. The only exception is Volatility 75 index, where we set higher Max Volume limits as this Index is the most popular.


## GitHub Link
https://github.com/regentmarkets/perl-feed-index-volatility


## Python Code For Simple Illustration
The python script is on the same path.

