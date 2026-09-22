\# README — Pricing Group Assignment



\*\*Course\*\*: 37005 Fundamentals of Derivative Security Pricing  

\*\*Assignment\*\*: Group Assignment (Due 3 November 2024)  

\*\*Students\*\*: Raphael Demare, Maximilien Striebig, Hugo Chen  



\---



\## Description



This submission provides solutions to the \*\*Pricing Group Assignment\*\*.



The project uses S\&P 500 European option data to calibrate market inputs, derive the price of a structured guaranteed-return payoff, determine fair guarantee levels, and construct static hedges using market-traded options.



The analysis focuses on:



\- \*\*Put–call parity and discount factor calibration\*\*

\- \*\*Black–Scholes implied volatility term structure\*\*

\- \*\*Pricing of a guaranteed-return structured payoff\*\*

\- \*\*Fair guarantee levels for different client participation rates\*\*

\- \*\*Static replication using standard options\*\*

\- \*\*Interpretation of guarantee levels and replication costs\*\*



The underlying S\&P 500 index level used in the assignment is \*\*S₀ = 4170.7002\*\*, with deterministic time-dependent interest rates and a deterministic continuous dividend rate.



\---



\## Tasks



\### Task 1 — Put/Call Parity and Discount Factors (1 mark)



\- \*\*Data\*\*: S\&P 500 European call and put options from the provided market dataset.

\- \*\*Method\*\*: Used mid-prices from the bid/ask quotes and calibrated the zero-coupon bond price \*\*B(0,T)\*\* and dividend discount factor \*\*D(0,T)\*\* for each maturity.

\- \*\*Approach\*\*: Minimized the sum of squared deviations from the put–call parity relationship:



&#x20; \\\[

&#x20; C(K,T)-P(K,T)=D(0,T)S(0)-B(0,T)K

&#x20; \\]



\- \*\*Implementation\*\*: Used the `minimize` function from SciPy to obtain the optimal discount factors for each maturity.



\---



\### Task 2 — Implied Volatility Term Structure (4 marks)



\- \*\*Method\*\*: Calculated the forward price of the S\&P 500 for each maturity using the calibrated discount factors:



&#x20; \\\[

&#x20; S(T)=S\_0\\frac{D(0,T)}{B(0,T)}

&#x20; \\]



\- \*\*Strike selection\*\*: For each maturity, selected the strike closest to the corresponding forward price.

\- \*\*Calibration\*\*: Used the Black–Scholes model to determine the implied volatility matching the observed bid and ask prices.

\- \*\*Result\*\*: Obtained piecewise-constant volatility functions \*\*σbid(t)\*\* and \*\*σask(t)\*\* across the option maturities.

\- \*\*Implementation\*\*: Used SciPy's `fsolve` function to solve for the implied volatilities.



\---



\### Task 3 — Pricing the Guaranteed-Return Payoff (5 marks)



\- \*\*Payoff\*\*: Derived the time-0 value of the structured payoff:



&#x20; \\\[

&#x20; V=\\max\\left(S(0)e^{gT},\\,S(0)^\\alpha S(T)^{1-\\alpha}\\right)

&#x20; \\]



\- \*\*Model\*\*: Assumed Black–Scholes dynamics with deterministic time-dependent volatility, interest rates, and dividend rates.

\- \*\*Method\*\*: Derived a closed-form pricing formula using the calibrated market inputs and the integrated volatility term structure.

\- \*\*Result\*\*: Obtained a formula expressed in terms of normal cumulative distribution functions and the parameters \*\*α\*\*, \*\*g\*\*, \*\*B(0,T)\*\*, \*\*D(0,T)\*\*, and the volatility term structure.



\---



\### Task 4 — Fair Guarantee Level (3 marks)



\- \*\*Maturity\*\*: 18 December 2026.

\- \*\*Method\*\*: Used the pricing formula derived in Task 3 and solved for the guarantee level \*\*g\*\* using SciPy's `fsolve`.

\- \*\*Result\*\*:



| α | Fair guarantee level g |

|---:|---:|

| 0.25 | -0.0218 |

| 0.50 | 0.0047 |

| 0.75 | 0.0196 |



The calculated guarantee level increases as \*\*α\*\* increases.



\---



\### Task 5 — Static Hedge and Replication Cost (4 marks)



\- \*\*Objective\*\*: Construct a static hedge for the structured payoff using market-traded standard options expiring on 18 December 2026.

\- \*\*Method\*\*: Optimized the option weights to minimize the initial replication cost while ensuring that the hedge payoff remains non-negative relative to the structured payoff.

\- \*\*Numerical approach\*\*:

&#x20; - Evaluated the payoff over \*\*10,000 points\*\* across the relevant range of S(T).

&#x20; - Minimized the squared differences where the hedge payoff could become negative.

&#x20; - Used `differential\_evolution` to identify the minimum payoff difference.

&#x20; - Added zero-coupon bonds when necessary to eliminate remaining negative payoff differences.

\- \*\*Resulting replication costs\*\*:



| α | Replication cost |

|---:|---:|

| 0.25 | 4259.896 |

| 0.50 | 4191.914 |

| 0.75 | 4170.323 |



The hedge consists of call and put positions across different strikes, together with a zero-coupon bond position.



\---



\### Task 6 — Interpretation of Results (3 marks)



The results show a relationship between the client's participation rate \*\*α\*\*, the guarantee level \*\*g\*\*, and the cost of the static hedge.



\- As \*\*α increases\*\*, the fair guarantee level \*\*g increases\*\*.

\- As \*\*α increases\*\*, the static replication cost \*\*decreases\*\*.

\- A higher α gives the client a larger share of returns above the guaranteed level, reducing the bank's exposure to the upside.

\- Consequently, the bank can offer a higher guarantee while requiring a lower replication cost.



\---



\## Outputs



\- \*\*Calibrated discount factors\*\* `B(0,T)` and `D(0,T)` for each maturity.

\- \*\*Bid and ask implied volatility term structures\*\*.

\- \*\*Closed-form pricing formula\*\* for the structured guaranteed-return payoff.

\- \*\*Fair guarantee levels\*\* for α = 0.25, 0.50 and 0.75.

\- \*\*Static hedge portfolios\*\* composed of calls, puts and zero-coupon bonds.

\- \*\*Replication cost and hedge payoff analysis\*\*.

\- \*\*Plots\*\* illustrating the implied volatility and static replication results.



\---



\## How to Run



1\. Place the provided S\&P 500 option market data file in the same directory as the Python implementation.

2\. Run the Python scripts/notebooks used to perform the calibration and pricing calculations.

3\. Execute the calculations for Tasks 1–6 sequentially.

4\. Review the calibrated discount factors, implied volatility term structure, fair guarantee levels and static hedge results.

5\. Review the generated plots and hedge payoff results.



The original assignment requires all programming used to produce the report to be submitted in Python.



\---



\## Dependencies



\- Python 3.x

\- NumPy

\- SciPy

\- Matplotlib



Key SciPy functions used in the implementation include:



\- `minimize` — calibration and portfolio optimization

\- `fsolve` — implied volatility and guarantee-level calculations

\- `differential\_evolution` — verification and adjustment of static hedge payoffs



\---

