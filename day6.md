# Security Lessons for the Day

## 1. Enforce Slippage Parameters on Final Step  
Enforcing slippage parameters for intermediate swaps but not on the final step can result in users receiving fewer tokens than expected.  
**Lesson:** Always enforce slippage checks at the very last step before transferring funds to the user.



## 2. Avoid Hard-Coded Fee Tier Parameters  
In AMM protocols like Uniswap V3, liquidity can be spread across multiple fee tiers. Hard-coding a fee tier limits flexibility and can lead to poor trade execution or routing issues.  
**Lesson:** Allow users (or calling functions) to pass the fee tier parameter instead of hard-coding it.



## 3. Handle External Call Failures Gracefully  
Failing to handle reverts or unexpected results from external protocol calls can break core functionality or cause unintended state changes.  
**Lesson:** Always implement proper error handling and fallback logic for external interactions.



## 4. Protect Against Excessive Gas Consumption  
Functions that loop over large arrays or unbounded user inputs can consume excessive gas and become uncallable.  
**Lesson:** Avoid unbounded loops and validate input sizes to ensure functions remain executable under gas limits.



## 5. Avoid Price Manipulation Vulnerabilities  
Relying on a single on-chain source for price data without safeguards can allow attackers to manipulate prices and execute profitable trades at the expense of the protocol.  
**Lesson:** Use time-weighted average prices (TWAP) or multiple price sources to mitigate manipulation risks.



## 6. Implement Access Control for Critical Functions  
Critical functions without proper access control can be called by anyone, potentially draining funds or altering protocol parameters.  
**Lesson:** Apply strict access control modifiers to sensitive functions and review them regularly.



## 7. Validate Token Transfer Results  
Some ERC-20 tokens do not revert on failure but instead return false. If the return value is not checked, failed transfers might go unnoticed.  
**Lesson:** Always validate token transfer results to ensure expected outcomes.



## 8. Prevent Unchecked External Token Calls  
Using `approve` or `transferFrom` without proper validation or without clearing allowances first can create attack vectors.  
**Lesson:** Reset allowances to zero before changing them and validate all token interactions.