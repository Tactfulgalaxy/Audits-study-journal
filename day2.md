# Common Security Issues in Lending & CDP Protocols

These are recurring issues found across various lending and CDP protocols based on audits, incident reviews, and protocol postmortems. I learnt somethings while going through a blog on lending protocols. 

## 1. Collateral Valuation Manipulation

Several protocols calculated collateral value using internal balances like `balanceOf()` or raw pool reserves. These figures can be manipulated within a flash loan.

**Example:**  
In 0VIX, the valuation of interest-bearing tokens was derived from on-chain balance logic. An attacker inflated the perceived value using flash loans and borrowed against it.

**Lesson:**  
Don’t rely on internal balances to determine value. Use oracles. If internal logic is necessary, enforce price sanity checks and TWAPs with proper delay windows.



## 2. Missing Health Checks in Sensitive Functions

Some functions that affected user positions skipped necessary health checks.

**Example:**  
Euler's `donateToReserves()` was designed to reduce debt but didn’t check if the account was still healthy afterward. That allowed users to withdraw collateral that should’ve remained locked.

**Lesson:**  
Every function that affects debt, collateral, or protocol state must verify vault health before and after execution.



## 3. Assumptions About Token Standards

Not all tokens behave like standard ERC-20s or ERC-721s.

**Example:**  
CryptoPunks don’t follow the ERC-721 standard. One protocol accepted them without proper handling, leading to frontrunning during NFT liquidations.

**Lesson:**  
Don’t assume interface compliance. Wrap non-standard tokens or explicitly block unsupported ones.



## 4. Unsafe NFT Transfer Logic

Some protocols transfer NFTs before validating a vault’s health. If the NFT uses `onERC721Received()`, an attacker could re-enter and bypass logic.

**Lesson:**  
Check vault safety before any external calls. Never place critical checks after token transfers.



## 5. Stablecoin Arbitrage Through Assumed Parity

Protocols sometimes allowed users to deposit one stablecoin and withdraw another at a 1:1 rate.

**Lesson:**  
Even small depegs or transfer fees can be exploited. Always check and update exchange rates, and avoid assuming stablecoins are perfectly interchangeable.



## 6. LP Token Pricing Issues

LP tokens were accepted as collateral without validating their source pools. Some protocols priced them using volatile on-chain reserve data.

**Example:**  
Warp Finance used `getReserves()` for LP token pricing. This allowed flash loan manipulation.

**Lesson:**  
Use external oracles or long TWAPs. Always validate the origin and token pair of LP tokens.



## 7. Tokens With Special Behaviors

- Some tokens charge fees on transfer.
- Others don’t revert on failed transfers.
- Some are blacklisted or paused by issuers.

**Lesson:**  
Use SafeERC20. Account for token-specific behavior, especially in transfer logic. Treat tokens as untrusted unless proven safe.



## 8. Liquidation and Auction Logic Gaps

Liquidation logic often contained issues:
- Auctions could be aborted if the vault health improved.
- Partial liquidation math was wrong.
- Refunds for failed bids weren’t handled properly.

**Lesson:**  
Lock state during auctions. Validate inputs carefully. Make sure every possible auction end state is handled cleanly and fairly.



## 9. Oracle Weaknesses

Oracles using short TWAP windows were manipulated to boost token prices.

**Example:**  
Inverse Finance was exploited this way. The manipulated price was picked up quickly, allowing the attacker to borrow more than allowed.

**Lesson:**  
Avoid short TWAPs. Add bounds or circuit breakers. If using Chainlink, wrap price reads in try/catch and have fallbacks.



## 10. Other Observations

- Rebasing tokens can mess with balance tracking.
- Ignoring decimals across tokens leads to subtle bugs.
- Protocols with shared liquidity pools lacked isolation, letting a hack in one affect all.



## Some prevention techniques 

| Risk Area                  | What to Watch For                                                 |
|---------------------------|--------------------------------------------------------------------|
| Pricing logic              | Avoid balance-based pricing; use oracles with protections         |
| Token assumptions          | Validate interfaces, handle edge-case behaviors                   |
| Liquidation logic          | Ensure math and state transitions are sound                       |
| Auctions                   | Lock state, prevent premature exits, handle refunds               |
| Stablecoins                | Don't assume 1:1 parity or transfer safety                        |
| Oracles                    | Use long TWAPs, deviation checks, fallbacks                       |
| LP tokens                  | Check pool sources, use secure pricing methods                    |
| Re-entrancy                | Never place critical checks after transfers                       |
| Vault health checks        | Apply before/after all state-changing operations                  |