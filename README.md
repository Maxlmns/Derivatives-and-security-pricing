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
A consistent term structure of \(B(0,T)\) and \(D(0,T)\) was obtained for the option maturities.  

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
- Lists of \(\sigma_i^{\text{bid}}\) and \(\sigma_i^{\text{ask}}\) were computed across maturities.  
- The implied volatility term structure was plotted.  

**Interpretation**  
The term structure highlights how short-maturity options imply different volatilities than long-maturity ones.  
Numerical root-finding showed sensitivity, but the approach successfully estimated the volatility curve.

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
- Solved for growth parameter \(g\) ensuring correct initial valuation.  
- Used constrained optimization (SLSQP, differential evolution) to determine option weights.  
- Compared replicated vs. target payoff graphically.  

**Results**
- **\(\alpha = 0.25\)**  
  - Replication error: **-16.77**  
  - Portfolio price: **4464.47**

- **\(\alpha = 0.50\)**  
  - Replication error: **-0.00179**  
  - Portfolio price: **4422.92**

- **\(\alpha = 0.75\)**  
  - Replication error: **0.3865**  
  - Portfolio price: **4374.76**

**Interpretation**  
- Replication worked best for \(\alpha = 0.50\), with negligible replication error.  
- For \(\alpha = 0.25\), the strategy overshot, producing a larger error and higher portfolio value.  
- For \(\alpha = 0.75\), the replication was less stable, requiring large zero-coupon bond weight (~605).  
- Overall, static replication was achieved, but sensitivity to parameters and optimization suggests the need for careful numerical tuning.

---

## Conclusion

The assignment demonstrates practical derivative pricing tools:  
- Put-call parity enables recovery of discount and dividend factors.  
- Forward price alignment and root-solving extract implied volatility term structures.  
- Exotic payoffs can be replicated using static portfolios of European options and bonds.  

The replication results show both the feasibility and the numerical challenges of static replication.  
Errors vary by payoff shape (\(\alpha\)), highlighting model sensitivity.

---

## Dependencies

- Python 3.x  
- pandas  
- numpy  
- scipy  
- matplotlib
