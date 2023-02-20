# Jump (Diffusion) Index

## Description

The Jump index is based on the existing volatility index, but it can make large jumps  every 20 minutes on average. Offerings given are Jump 10, 25, 50, 75 and 100. Each number refers to the volatility of the model in percentage. Ticks are generated every 1 second.

## Behaviour
The Jump index mimics a typical market where shocks can occur from news events. 

For example, the spot jumping downwards represents a bad news event, while a jump upwards represents a shock from a good news event.

## Construction

There are two cases on each tick: If $U$ is a uniformly generated random number between 0 and 1, we define the jump probability (Poisson) as:

$$
P=\frac{\lambda}{86400}\exp \left(-\frac{\lambda}{86400}\right)\approx0.08326\%
$$

Where $\lambda=72$ represents the number of jumps per day.

1. If $U>P$, we have typical Geometric Brownian Motion:

$$
S_t=S_{t-1}\exp\left[-\frac{\sigma^2}{2}dt+\sigma\sqrt{dt}\cdot X_1\right]
$$

2. If $U < P$, we have Geometric Brownian Motion augmented with a jump
$$
S_t=S_{t-1}\exp\left[\left(\mu-\frac{\sigma^2}{2}\right)dt+\sigma\sqrt{dt}\cdot X_1+J\sigma\sqrt{dt}\cdot X_2\right]
$$

where
* $S_t$ = spot price at $T=t$;
* $S_{t-1}$ = previous spot price $T=t-1$;
* $J$ = Jump factor, fixed at $30$;
* $\sigma$ = volatility offered at $(10\%,25\%,50\%,75\%,100\%)$;
* $\mu$ = Drift correction of jump process, i.e $\mu=-\frac{J^2\sigma^2}{2}$;
* $dt$ = 1 second per year, i.e. $dt=\frac{1}{365\cdot24\cdot60\cdot60}$;
* $X_1,X_2\sim\mathcal{N}(0,1)$ = sample from a standard normal distribution.


## Offerings

For more details, please refer to the main table [here](https://wikijs.deriv.cloud/en/Trading/Model-Validation_Engineering/ModelValidation/Indices/offerings-underlying-table).

## Trading Condition

The following is a summary of how the trading conditions are determined.

### MT5 Spread

The MT5 spread for Jump Index is variable, depending on the current price and pre-determined spread percentage, i.e. 

$$\text{MT5 spread}=\text{current price}\times\text{spread percentage}$$

The spread percentage is defined based on the simulation to protect us from exploit. The spread percentages of each index are:

| Index  |  spread percentage	|  
|---------|	:----:	| 
| Jump 10  |0.0024%|
| Jump 25  |0.0059%|
| Jump 50  |0.0118%|
| Jump 75  |0.0177%|
| Jump 100 |0.0236%|

There might be a mark-up spread on top of this MT5 spread due to various business decisions such as extra mark-up for high-risk clients.

The bid-ask spread is the summation of MT5 spread + mark-up spread. 

### MT5 Swap Rate
$$\text{swap rate}=\text{volatility}\times10\% $$

Swap fee is charged when a client holds the position overnight. Use Jump 75 index as an example, the amount a client will be charged is :

$$\text{swap fee in dollar}=\text{volume in lot}\times\text{current price}\times \frac{7.5\%}{365}$$
Latest swap rate can be found here : https://docs.google.com/spreadsheets/d/1M92ZIFpImvJ68EemeXiZBBoSVnpfDEH1/edit#gid=865395382

### MT5 Lot Size

#### Min Lot Size
Min lot size is defined to have IB’s commission at least $0.01 for min lot. 

#### Max Lot Size
Max lot size is set to make volatility-adjusted USD Volume approximately equal across all Synthetic assets.

## GitHub Links
**Index code**: https://github.com/regentmarkets/perl-Feed-Index-JumpDiffusion

**Parameters**: https://github.com/regentmarkets/cpan-private/blob/280aa93f1101907b0f7a64b5334a909051bf4445/local/lib/perl5/auto/share/dist/Feed-Index-JumpDiffusion/jumpdiffusion.yml


## Python Code For Simple Illustration
The python script is on the same path.