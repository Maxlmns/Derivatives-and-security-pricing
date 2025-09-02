# README - Assignment Part 3

**Course**: 37005 Fundamentals of Derivative Security Pricing - Spring 2024  
**Instructor**: Erik Schlögl, University of Technology Sydney  
**Student**: Maximilien Striebig  

---

## Description

This notebook analyzes European options on the S&P 500 index (CBOE, 8 March 2022).  
It implements option pricing and replication methods using market bid/ask data.  
The work covers calibration of discount factors via put-call parity, estimation of forward prices,  
implied volatility term structure, and static replication of exotic payoffs.

---

## Tasks

### Task 1 – Estimation of Discount Factors (1 mark)

**Objective**  
Determine the zero-coupon bond prices \(B(0,T)\) and dividend discount factors \(D(0,T)\)  
by enforcing put-call parity across strikes and maturities.

**Method**
- Used mid-prices (average of bid/ask) for calls and puts.  
- Defined squared deviation objective from put-call parity identity.  
- Optimized \(B(0,T)\) and \(D(0,T)\) for each maturity.  

**Result**  
A consistent term structure of \(B(0,T)\) and \(D(0,T)\) was obtained.  
(Displayed in `term_structure` DataFrame within the notebook.)

**Interpretation**  
This step extracts the effective risk-free discounting and dividend yield curve from market option prices.

---

### Task 2 – Implied Volatility Term Structure (2 marks)

**Objective**  
Estimate piecewise constant volatilities from near-forward strikes across maturities.

**Method**
- Identified strikes closest to the forward price \( F(0,T) = S_0 D(0,T)/B(0,T) \).  
- Used bid/ask option prices at those strikes.  
- Solved for segment volatilities \(\sigma_i\) using nonlinear root-finding (`fsolve`).  

**Results**  
- Produced lists of \(\sigma_i^{\text{bid}}\) and \(\sigma_i^{\text{ask}}\) across maturities.  
- Plotted the implied volatility term structure.  

**Interpretation**  
The term structure highlights how short-maturity options imply higher or lower volatility than longer-dated ones,  
though solver warnings suggest numerical sensitivity.

---

### Task 3 – Exotic Payoff Replication (2 marks)

**Objective**  
Replicate exotic payoffs of the form  
\[
\max\!\big(S_0 e^{gT},\, S_0^\alpha S^{1-\alpha}\big)
\]  
using static portfolios of vanilla options and zero-coupon bonds.

**Method**
- Defined payoff function depending on parameter \(\alpha \in \{0.25, 0.5, 0.75\}\).  
- Computed cumulative variance \(\sigma^2\) from calibrated segment volatilities.  
- Solved for parameter \(g\) ensuring correct initial valuation.  
- Used constrained optimization (SLSQP, differential evolution) to determine option weights.  
- Compared replicated vs. target payoff graphically.  

**Results**
- Estimated growth parameters:  
  - \(\alpha = 0.25 \Rightarrow g \approx\) printed in notebook  
  - \(\alpha = 0.50 \Rightarrow g \approx\) printed in notebook  
  - \(\alpha = 0.75 \Rightarrow g \approx\) printed in notebook  
- Replication strategies produced payoff curves closely matching the exotic target payoff.  
- Portfolio prices around 4190–4260 were obtained, close to \(S_0 = 4170.7\).  

**Interpretation**  
Static replication is feasible with reasonable option weights, though numerical optimization requires care.  
Replication error is small and visualized in the payoff comparison plots.

---

## Conclusion

The assignment demonstrates practical derivative pricing tools:  
- Put-call parity enables recovery of discount and dividend factors.  
- Forward price alignment and root-solving extract implied volatility term structures.  
- Exotic payoffs can be replicated using static portfolios of European options and bonds.  

The results confirm theoretical relationships and illustrate how market option data supports calibration  
of discounting, dividends, volatility, and structured payoff replication.

---

## Dependencies

- Python 3.x  
- pandas  
- numpy  
- scipy  
- matplotlib
