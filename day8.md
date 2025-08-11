# 7 Lessons for Auditors from the day's audit  Report study 

1. **Always check if a token address has deployed code before interacting**  
   - Functions like `safeTransferFrom` or `transfer` should revert if the token contract code doesn’t exist. Without this, attackers can use undeployed addresses to manipulate balances.

2. **Understand how `CREATE2` enables precomputed addresses**  
   - Attackers can know the address of a token before deployment (especially with `CREATE2`) and prepare exploits in advance.

3. **Watch for internal accounting updates without real asset transfers**  
   - If the code assumes a transfer succeeded without confirming actual balance changes on-chain, attackers can “mint” fake balances.

4. **Consider front-running opportunities around token deployment**  
   - A malicious actor can front-run a market creation or deposit action using a fake token address, then later exploit it after the real token is deployed.

5. **Review both loan and collateral handling for symmetry of checks**  
   - If a vulnerability affects loan tokens, it may also apply to collateral tokens; auditors must test both paths.

6. **Don’t trust external components (e.g., oracles) to validate tokens**  
   - Oracles may accept non-existent token addresses, which can make it easier for attackers to pass off fake tokens.

7. **Verify post-conditions of token interactions**  
   - After transfers, deposits, or supplies, ensure that actual on-chain balances match internal accounting to prevent fake supply injections.