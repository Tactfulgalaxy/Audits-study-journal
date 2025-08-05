# Here are some security tips for security researchers. Applicable to all protocols. 

- Don’t rely on `tx.origin`. It breaks with contract-based wallets or multisigs.

- Some ERC-20s don’t revert or return false on failure. Use `SafeERC20`.

- Storage layout changes in upgradable contracts can silently break everything. Always check before deploying.

- Avoid putting logic in fallback functions unless absolutely necessary.

- Never make external calls before updating contract state. That’s how reentrancy slips in.

- Don’t assume transfers always work. Tokens with fees, blacklists, or paused states will break your logic.

- Infinite loops in on-chain code are dangerous. Avoid looping over dynamic arrays unless you’re capping them.

- Avoid hardcoding trust into price feeds or token behavior. Everything external can break or lie.

- Commit-reveal is still one of the cleanest ways to avoid front-running in sensitive operations.

- Logging ≠ storage. Events are not a reliable source of truth.