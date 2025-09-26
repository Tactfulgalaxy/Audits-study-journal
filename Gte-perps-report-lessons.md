# Smart Contract Security Lessons

A list of some of the lessons i learnt from going through the valid submissions from the gte contest.



## 1. Queues, Loops, and Gas-Based DoS

- Always treat unbounded loops or user-controlled data structures as potential gas-based Denial of Service (DoS) risks.  
- Never design queue operations or batch-processing functions that scale linearly with unbounded growth.  
- Avoid O(L) array rewrites or operations like copying, shifting, or slicing in Solidity state. Use pointer-based queues or mapping + pointers instead.  
- Cap batch sizes and enforce per-user/global queue limits to prevent attackers from bloating data structures.  
- Enforce **minimum request/withdrawal sizes** to prevent spam attacks with tiny entries that inflate gas usage.  
- Prevent griefing by avoiding designs where every new operation requires rebuilding or iterating over the entire queue.  
- Always design batch-processing to **scale safely** and remain resumable.  
- Isolate external calls within batches (using `try/catch` or pull-based models) so a single failed transfer cannot freeze the system.  
- Provide an **admin escape hatch** or upgrade-safe fallback to ensure one unprocessable item never blocks forward progress.



## 2. Handling External Calls and Input Arrays

- Never accept unbounded, user-supplied arrays that trigger per-element `SSTORE`s without limits. Use batching, caps, or packed/bitmap storage.  
- When performing per-item external calls (e.g., ERC-20 transfers), ensure failures cannot revert the entire batch.  
- Maintain liquid reserves so normal admin operations (like withdrawals) cannot be frozen by external failures.



## 3. AMMs, Liquidity, and Token Safety

- Never assume AMM reserves are “clean.” Validate and handle edge cases such as one-sided liquidity.  
- Always verify that `CREATE2`-derived addresses (e.g., AMM pairs) use the **exact same salt and init code hash** as the factory contract. Mismatches can permanently break the protocol.  
- Validate user-supplied tokens in reward or liquidity systems. Only update accounting if the **actual token transfer succeeds**.  
- Never trust caller-supplied token addresses. Always confirm that the token matches the expected pool asset and that the contract balance actually increased before recording deposits.  
- Never use raw token balances for invariants when protocol fees are present. Always subtract reserved protocol amounts to avoid miscounting usable liquidity.


## 4. Liquidations and Accounting Consistency

- A liquidation must clear **all exposures** (positions and outstanding orders) for the same account/subaccount. Leaving lingering orders can recreate uncollateralized positions and cause bad debt.  
- Keep mint and burn logic consistent. Exclude protocol fees or pending rewards from distribution amounts. Otherwise, attackers can mint “clean” shares and burn them for “dirty” balances to steal unclaimed fees.



## Summary

These issues from the contest emphasize **constant-time operations, safe batching, strict validation, and accounting consistency and so on to avoid issues**  
Following them ensures contracts remain resilient against gas griefing, DoS, liquidity mismanagement, and accounting exploits.
