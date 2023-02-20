# Step Index

## Description

Step index goes up and down with a fixed step size of 0.1 per tick with equal probability.


## Behaviour
This index does not simulate the financial markets.

## Construction

Referring to the random number generated from uniform distribution, should the random number > 0.5, the index will go up, and down otherwise. The tick is generated every second.

$$
S_t = S_{t-1} + \textrm{step}
$$

where
* $S_t$ = spot price at time $~t$;
* $S_{t-1}$ = spot price at time $t-1$;
* $\textrm{step} = 0.1$ if $X > 0.5$ else $-0.1$;
* $dt$ = 1 second in year fraction, i.e. $dt = \frac{1}{(365 \cdot 24 \cdot 60 \cdot 60)}$;
* $X \sim \mathcal{U}(0,1)$ = sample from a standard uniform distribution.

## Offerings

For more details, please refer to the main table [here](https://wikijs.deriv.cloud/en/Trading/Model-Validation_Engineering/ModelValidation/Indices/offerings-underlying-table)

## Trading Condition

The following is a summary of how the trading conditions are determined.

### MT5 Spread

The MT5 spread for the step index is constant at 0.1. MT5 offers 

$$\begin{align*}
price_{bid} &= price_{spot} \\
price_{ask} &= price_{spot} + 0.1
\end{align*}
$$

There might be a mark-up spread on top of this MT5 spread due to various business decisions such as extra mark-up for high-risk clients.

The bid-ask spread is the summation of MT5 spread + mark-up spread. 

### MT5 Swap Rate
MT5 Swap Rate for the index is supposed to be fixed as $1 per lot.

The current swap rate however is in percentage terms and is variable.

### MT5 Lot Size

#### Min Lot Size
Min lot size is defined to have IB’s commission at least $0.01 for min lot. 

#### Max Lot Size
Max lot size is set to make volatility-adjusted USD Volume approximately equal across all Synthetic assets.


## GitHub Link
<b>Index Generation </b>: https://github.com/regentmarkets/perl-feed-index-steprng


## Python Code For Simple Illustration
The python script is on the same path.