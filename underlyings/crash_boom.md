# Crash/Boom Index

## Description
The Crash/Boom Index are indices which will go up (down for Boom) in small steps with high probabilities, and will have a big drop (jump for Boom) on average after $N$ ticks. For example, Crash 1000 will have a big drop on average after 1000 ticks. Currently we offer Crash/Boom 300/500/1000 Index. Tick values are generated every 1 second. 
 
## Behaviour
The Crash/Boom Index simulates a bullish (Crash) or a bearish (Boom) market movement until there there is an event with $\frac{1}{N}$ chance each tick that can cause a big drop (Crash) or a big jump (Boom), such as economic events.

## Construction
The step of the Crash/Boom Index is generated based on the formula below:

$$S_{t} =
  \begin{cases}
    S_{t-1}\exp \left(MUT \cdot \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \cdot \sqrt{dt} \right)       & \quad \text{if } x > \text{up probability}\\
    S_{t-1}\exp \left(MDT \cdot \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \cdot \sqrt{dt} \right)  & \quad \text{else} 
  \end{cases}$$

where
* $S_{t-1}$ = spot price at $T=t-1$;
* $S_t$ = spot price at $T=t$;
* $MDT/MUT$ = Mean Up Tick ("MUT") and Mean Down Tick ("MDT"); 
* $\lvert\mathcal{N}(1,1)\rvert$ = sample from a folded normal distribution with location $m=1$ and scale $s^2=1$;
* $\mu$ = long term mean of $\lvert\mathcal{N}(m=1,s^2=1)\rvert$ $=s\times\sqrt{\frac{2}{\pi}}\times\exp\left(-\frac{m^2}{2s^2}\right)+m\times(1-2\times\Phi(-\frac{m}{s}))$ , where $\Phi$ is the CDF of a standard normal distribution;
* $dt$ = 1 seconds in year fraction, i.e. $dt=\frac{1}{(365\cdot24\cdot60\cdot60)}$;
* $x\sim\mathcal{U}(0,1)$ = sample from a standard uniform distribution;
* $\text{up probability}$ is defined for each Crash/Boom Index. E.g. Crash 1000 has up probability $1-1/1000 = 0.999$.


## More about MDT/MUT

Here uses Crash 1000 as an example. For Crash 1000, every crash happened to be 0.1% of previous spot on average.
So in long run we have:

$$
\begin{align*}
S_t &= S_{t-1}\exp \left(MDT \cdot \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \cdot \sqrt{dt} \right) \\
0.999 \cdot S_{t-1} &= S_{t-1}\exp \left(MDT \cdot \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \cdot \sqrt{dt} \right) \\
0.999 &= \exp \left(MDT \cdot \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \cdot \sqrt{dt} \right) \\
\log(0.999) &= MDT \cdot \sqrt{dt} \quad \text{where  } \mathbb{E}\left(\lvert\mathcal{N}(1,1)\rvert\right)=\mu
\end{align*}
$$

Therefore:
$$MDT=\frac{\log(0.999)}{\sqrt{dt}}=-5.619$$

Althought the above method justifies why -5.619 is chosen for Crash 500/1000 MDT and 5.619 for Boom 500/1000 MUT, this number can be arbitrary. Once the value for $MDT$ (or $MUT$) is determined, $MUT$ (or $MDT$) needs to satisfy the below equation in order to avoid non-zero drift in the long run.

$$
\begin{align*}
\mathbb{E}\left(S_{t} \right) &= P_u \times \mathbb{E} \left[S_0\exp \left(MUT \times \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \times \sqrt{dt} \right) \right] + P_d \times \mathbb{E} \left[S_0\exp \left(MDT \times \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \times \sqrt{dt} \right) \right] \\
\mathbb{E}\left(\frac{S_{t}}{S_0} \right) &= P_u \times \mathbb{E} \left[\exp \left(MUT \times \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \times \sqrt{dt} \right) \right] + P_d \times \mathbb{E} \left[\exp \left(MDT \times \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \times \sqrt{dt} \right) \right] \\
1 &= P_u \times \mathbb{E} \left[\exp \left(MUT \times \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \times \sqrt{dt} \right) \right] + P_d \times \mathbb{E} \left[\exp \left(MDT \times \frac{\lvert\mathcal{N}(1,1)\rvert}{\mu} \times \sqrt{dt} \right) \right]
\end{align*}
$$

We can see that the above expectations are instances of the moment generating function $M_X(t) = \mathbb{E}[e^{tX}]$ where $X$ is the folded normal random variable. We can rewrite the equation using the moment-generating functions like the following

$$ P_u \times M_{|\mathcal{N}(1,1)|}(t_1) + P_d \times M_{|\mathcal{N}(1,1)|}(t_2) = 1
$$

Where the moment generating function of the folded normal distribution is:

$$ M_{|\mathcal{N}(1,1)|}(t) = \exp \left({\frac{\sigma^2t^2}{2}+\mu t} \right) \phi{\left(\frac{\mu}{\sigma}+\sigma t \right)} + \exp \left({\frac{\sigma^2t^2}{2}-\mu t} \right) \phi{\left(\frac{-\mu}{\sigma}+\sigma t \right)} 
$$
And 
$$
\begin{aligned}
t_1 &= \frac{MUT\sqrt{dt}}{\mu} \\
t_2 &= \frac{MDT\sqrt{dt}}{\mu}
\end{aligned}
$$

To obtain MUT given MDT, we use Python to solve for $t_1$ given $t_2$ and vice versa for MDT given MUT.

## Offerings
For more details, please refer to the main table [here](https://wikijs.deriv.cloud/en/Trading/Model-Validation_Engineering/ModelValidation/Indices/offerings-underlying-table).

## Trading Condition

The following is a summary of how the trading conditions are determined.

### MT5 Spread

The MT5 spread for Crash/Boom Index is variable, depending on the current price and pre-determined spread percentage, i.e. 

$$\text{MT5 spread} = \text{current price} \times \text{spread percentage}$$

The spread percentage is defined based on the simulation to protect us from exploit. The spread percentages of each index are:

| Index 				|   spread percentage	|  
|-----------		|	:--------:	| 
| Crash/Boom 1000  | 0.0010%|
| Crash/Boom 500 |0.0014%|
| Crash/Boom 300 |0.0050%|

There might be a mark-up spread on top of this MT5 spread due to various business decisions such as extra mark-up for high-risk clients.

The bid-ask spread is the summation of MT5 spread + mark-up spread. 

### MT5 Swap Rate
MT5 Swap Rate is approximately 1/3 of the long term volatility of each index as the spread are much lower in term of expected ticks, e.g. 21% volatility for Crash 1000 index. 
Latest swap rate can be found here : https://docs.google.com/spreadsheets/d/1M92ZIFpImvJ68EemeXiZBBoSVnpfDEH1/edit#gid=865395382

### MT5 Lot Size

#### Min Lot Size
Min lot size is defined to have IB’s commission at least $0.01 for min lot. 

#### Max Lot Size
Max lot size is set to make volatility-adjusted USD Volume approximately equal across all Synthetic assets.


## GitHub Link
* <b> Index Generation </b>: https://github.com/regentmarkets/perl-Feed-Index-CrashBoom/blob/master/lib/Feed/Index/CrashBoom.pm
* <b> Configuration </b>: https://github.com/regentmarkets/perl-Feed-Index-CrashBoom/blob/master/share/crashboom.yml
* <b> Latest Adjustment on Feed Generation </b>: https://redmine.deriv.cloud/issues/31261#Development_spec_for_low_leverage_crash/boom_indices


## Python Code For Simple Illustration
The python script is on the same path.
