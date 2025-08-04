# What I Learnt While Studying Uniswap V3 and  Security Lessons for AMMs

While digging into Uniswap V3, I picked up a bunch of security details that you wouldn’t easily catch unless you’ve spent time really studying how the protocol works. Some of these might seem small, but they can break a system fast if overlooked.



##  ETH Doesn’t Auto-Refund

One thing I noticed — when users swap with ETH, any extra ETH they send doesn’t just bounce back automatically. It stays in the router. Unless `refundETH()` is called, the ETH is just stuck there.

Only the original sender can call `refundETH()`, but if you forget to call it (especially if you don’t wrap it in a multicall), you’re basically leaving money behind.

**Takeaway**: If your AMM supports ETH, always wrap swap + refund in one call. Never assume leftover ETH is handled for you.



##  TWAP Matters, Spot Price Isn’t Enough.

Pulling prices directly from `slot0` works, but it’s risky. An attacker can push prices around with a few swaps, especially in low-liquidity pools.

**Takeaway**: If you're building on top of Uniswap or any AMM, use a TWAP. Add deviation checks. Don’t trust spot prices — ever.



##  Be Careful With `unchecked`

Uniswap uses `unchecked` in math-heavy areas to save gas and avoid revert spam in older Solidity versions. But if you mess this up, especially after 0.8.x, you're asking for trouble.

**Takeaway**: Know exactly why you're using `unchecked`. Don’t copy-paste it blindly. Bugs here are silent and brutal.



##  Path Encoding Has To Be Exact

Swaps in V3 use a `path` parameter (tokenA|fee|tokenB), and if you're decoding it differently on your end, the whole thing breaks.

**Takeaway**: Stick to Uniswap's path structure — no shortcuts. If you’re not decoding it properly, your contract will just choke mid-swap.



##  Use Slippage Bounds  Or Get Sandwiched

Uniswap lets you set slippage bounds via `sqrtPriceLimitX96`. Most users don’t even know it exists, but if it’s not set, you're wide open to sandwich attacks.

**Takeaway**: If you care about your users, force them to set max slippage. If you don’t, someone else will, and they’ll profit.



##  Small Pools Are a Big Risk

Uniswap allows multiple pools for the same token pair with different fee tiers. A lot of those pools are empty or barely used. If you price from those, you’re playing yourself.

**Takeaway**: Whitelist pools with deep liquidity. Don’t grab prices from just any pool address — make sure it's battle-tested.



##  Token Order Isn’t Just Cosmetic

Uniswap always sets `token0` and `token1` based on address sorting. That changes how prices are interpreted. Cross-chain, this can flip without warning.

**Takeaway**: Don’t hardcode assumptions about which token is `token0`. Always recalculate based on addresses, especially when porting across chains.



##  Callback Abuse Is Real

Swaps and flash loans call back into your contract. If you’re not ready for that, attackers will hit you with reentrancy, fake tokens, or whatever they can.

**Takeaway**: Always validate what’s coming into your callback. Add reentrancy guards. Treat callbacks like a door you didn’t lock.



##  Fee Tiers Should Be Verified

Don’t just let anyone create a pool with any fee tier. Some people abuse rare fee tiers to spin up shallow pools and game your price logic.

**Takeaway**: Stick to known fee tiers (like 0.05%, 0.3%, 1%). If you're accepting LP tokens, verify their origin and fee tier before trusting the price.



##  Inconsistent Position Tracking Can Backfire

Liquidity positions are NFTs. If you don’t track them properly or allow unsafe updates, someone could overwrite, reuse, or front-run you.

**Takeaway**: Treat LP NFTs like live state. Always verify position ownership and uniqueness. Don’t allow repeat mints on the same `tokenId`.



##  Final Thoughts

Uniswap V3 is powerful, but also sharp-edged. A lot of the vulnerabilities don’t come from the core contracts — they come from how people build around them. If you’re integrating AMM logic, **test every edge**. And if you're auditing one, dig deep into assumptions - pricing, token behavior, math, reentrancy, etc.

You can't just secure swaps. You have to secure the logic *around* them.

GM