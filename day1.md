# Aave V3 : Notes & Lessons from Audit Findings

##  Personal Study

I started by studying Aave V3 through Cyfrin’s Updraft course to understand how the protocol works — supply, borrow, repay, liquidations, flash loans, and interest models. Once I got the full picture, I dived into real audit findings on Solodit to see how vulnerabilities show up in live code.

##  Lessons Learned from Report Findings

### 1. Handle refunds carefully
If a user's refund amount is smaller than the penalty they owe, don’t still give them a partial refund. Clear out the value properly. Failing to do this can create imbalances in protocol funds and let users walk away with more than they should.

### 2. Don't trust timestamps blindly
Just checking that a timestamp exists isn’t enough. Prices from oracles can still be stale even if the timestamp looks valid. Always define and enforce a freshness threshold.

### 3. Be consistent when using oracles
When pulling prices from different feeds, make sure they both represent the same time period. Mixing a historical price with a live one leads to incorrect calculations and false outcomes.

### 4. Don’t assume state transitions
Even if a condition suggests a state should’ve changed (like rewards being ready to claim), don’t assume it actually happened. Wait for the proper function to be called before updating anything.

### 5. Don’t overcomplicate pricing when tokens expose internal logic
If a token already provides a way to calculate its value internally, use it. Adding extra layers through external oracles not only adds complexity, but can also bring in risks like stale prices or math mismatches.

##  Takeaway Summary

- Always double-check refund and fee handling logic for edge cases
- Validate the freshness of all oracle-sourced data
- Don’t mix latest and historical data across feeds
- Make state changes only when the actual function has been executed
- Use internal token methods when available for conversions and pricing


*By [@TactfulCipher](https://github.com/Tactfulgalaxy)*  
Learning from real protocols, one bug at a time.