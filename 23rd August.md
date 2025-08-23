# Smart Contract Auditing Lessons



## 1. Don’t Trust ERC20 Return Values Blindly

Not all ERC20 tokens follow the standard strictly. Some don’t revert when a transfer fails — they just return `false`.  
If your contract ignores that and assumes success, tokens can get stuck.

**Lesson:** Always use `SafeERC20`. It forces failures to revert instead of being silently ignored.



## 2. Upgrades Must Handle Empty States

Upgrade functions sometimes assume that balances or counters are non-zero.  
If called when those values are zero, the protocol can lock up or revert permanently.

**Lesson:** Code upgrade paths so they succeed even when balances are empty. Empty state is still a valid state.



## 3. Reset State When Replacing External Contracts

Some contracts depend on external services (reward managers, oracles, data providers).  
If you replace the external contract without resetting internal baselines, calculations can break or underflow.

**Lesson:** When swapping dependencies, sync the state so internal and external counters align.



## 4. One-Time Functions Should Never Brick the System

Initialization and migration functions are usually called once.  
If they fail due to strict requirements (like “must have non-zero balance”), the protocol can be stuck in limbo forever.

**Lesson:** Always design one-time setup functions to complete gracefully, even when conditions are less than ideal.



## 5. Consistency in Code Paths Prevents Hidden Bugs

In some contracts, different functions that perform similar tasks don’t always use the same safety checks.  
For example, one uses `SafeERC20` but another uses raw `transfer()`.  
This inconsistency creates subtle vulnerabilities.

**Lesson:** Follow consistent coding patterns across all functions. If a safe pattern is needed in one place, it’s probably needed everywhere.



## 6. Don’t Assume External Contracts Behave Forever

When a protocol relies on another contract (lending pools, distributors, oracles), it assumes the external logic will keep working.  
But if that contract changes, upgrades, or gets replaced, your assumptions can collapse.

**Lesson:** Whenever you depend on another contract, build in checks or migration paths that handle changes safely.



## 7. User Funds Should Never Depend on Admin Mistakes

A common failure mode: admin calls an upgrade or replacement function incorrectly, and suddenly all user deposits are stuck.

**Lesson:** Always guard upgrade functions with proper state syncing and safety checks.  
Admin actions should not be able to accidentally lock user funds.

