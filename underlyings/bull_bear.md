# Bull/Bear Market Index

## Description

The Bull/Bear Market Index simulates the financial markets with constant drift and constant volatiliy. The Bull/Bear Market Index is generated every 2 seconds. Theoretically, the Bull/Bear Market Index can explode or arrive to zero in long run, therefore it is reset daily at GMT 0.


## Behaviour
The Bull/Bear Market Index simulates the financial markets when the expectation on the indices is trending upward or downward, for example, when there is positive/negative carry (net interest rate differential) for a FX pair.

## Construction
The Bull/Bear Market Index is generated based on Geometric Brownian Motion with positive or negative drift (i.e dividend rate is negative or positive)

$$
S_t=S_{t-1}\exp\left[\left(-d-\frac{\sigma^2}{2}\right)dt+\sigma\sqrt{dt}\cdot X\right]
$$

where
* $S_{t-1}$ = spot price at time &nbsp;$t-1$;
* $S_t$ = spot price at time &nbsp;$t$;
* $d$ = dividend rate = -35 for Bull Market index or 20 for Bear Market Index
* $\sigma$ = volatility. 175% for Bull Market index or 155% for Bear Market Index
* $dt$ = 2 seconds in day fraction, i.e. $dt = \frac{2}{(365 \cdot 24 \cdot 60 \cdot 60)}$
* $x \sim \mathcal{N}(0,1)$ = sample from a standard normal distribution


## Offerings

For more details, please refers to the main table [here](https://wikijs.deriv.cloud/en/Trading/Model-Validation_Engineering/ModelValidation/Indices/offerings-underlying-table)

## GitHub Link
<b>Index Generation </b>: https://github.com/regentmarkets/perl-feed-index-volatility

<b> Interest rate & Volatility </b>: https://github.com/regentmarkets/perl-feed-index-volatility/blob/1a1985963d3a1d66d20e753256b3b42b9b706ef1/share/volatility.yml

## Python Code For Simple Illustration
The python script is on the same path.