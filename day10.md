#  Audit Lessons for Restaking + Stablecoin Protocols

## 1. Never Assume Staked Collateral Maintains Its Value During Market Stress
Staked collateral can lose value rapidly during volatile markets, especially when the same assets are used in multiple services through restaking.  
A drop in value in one system can cascade into others, amplifying losses.



## 2. Slashing Events Can Trigger Instant Depegging
If collateral is slashed, the backing for a stablecoin is immediately reduced.  
This can cause a sudden loss of peg. Independent reserve buffers should be kept for each external service to avoid chain-reaction depegging.



## 3. Operator Delegation Creates Centralization Risks
Delegating too much stake to a small set of operators concentrates power.  
If those operators fail or act maliciously, the entire collateral base may be at risk.  
Stake distribution caps and monitoring should be enforced.



## 4. Define Clear Precedence Rules for Cross-Protocol Penalty Conditions
When multiple staking protocols have different slashing rules, penalties may conflict, making liquidations unmanageable.  
Protocol logic should have explicit rules on which penalties take priority.



## 5. Account for Withdrawal Delays in Staking Protocols
Unbonding or withdrawal delays can make it impossible to access collateral quickly during emergencies.  
A separate pool of liquid reserves should be maintained for instant withdrawals.



## 6. Isolate Critical Collateral from High-Risk Dependencies
Collateral that is essential for stablecoin backing should not be tied to experimental or unstable protocols.  
Isolation reduces the chance of contagion from failures in external systems.



## 7. Fiat-Backed Assets Can Fail During Traditional Banking Crises
Even stablecoins backed by fiat reserves are exposed to off-chain risks.  
If a bank freezes withdrawals or fails, those reserves may become inaccessible, affecting the stablecoin’s stability.



## 8. Over-Collateralization Must Cover All Penalty Scenarios
Collateral ratios should be calculated with the assumption that multiple penalties or losses may occur simultaneously.  
Using standard single-risk ratios underestimates the real exposure in restaking.



## 9. Pure Algorithmic Pegging Is Vulnerable to Confidence Collapse
If a stablecoin is pegged solely through algorithmic adjustments without real collateral,  
a loss of market confidence can cause rapid and total collapse of value.



## 10. Market Crashes Amplify Depegging via Mass Liquidations
In severe downturns, mass liquidations can push the stablecoin further off its peg.  
Liquidation systems must be built to handle high volumes efficiently, even under network congestion.