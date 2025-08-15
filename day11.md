## Audit lessons for the day


1. **Always enforce whitelist and role-based restrictions on-chain for sensitive actions like burns or redemptions, even if off-chain processes are expected to handle eligibility off-chain assumptions can fail due to race conditions or unexpected actor behavior.**

2. **Always ensure withdrawal logic can access and redeem staked or invested assets when on-hand liquidity is insufficient—especially during special operational phases like yield farming so users can always retrieve their entitled funds.**

3. **When building vault or yield systems, always make sure the withdrawal function can pull money back from investments if there’s not enough cash on hand—otherwise users can get stuck.**

4. **Enforce critical validation checks at every entry point where state changes can happen, not just through the main contract otherwise, bypassing a gateway contract can create stuck funds or broken invariants.**

5. **When designing helper functions, ensure return values match the intended meaning in all edge cases especially when the return value is later used in arithmetic that affects user balances.**

6. **When updating shared state (like total shares or balances), always adjust it by the specific user’s amount rather than overwriting or removing the entire value—otherwise, one action can unintentionally or maliciously wipe out all users’ data.**

7. **When updating a user’s reward timestamp or stake state, always settle any pending rewards first before overwriting values—otherwise, legitimate earnings can be permanently lost, and malicious actors can exploit the reset.**

8. **Before transferring tokens to another contract, verify that the recipient contract supports the token standard and has the necessary logic to handle those tokens—otherwise, you risk sending funds into an unrecoverable black hole.*"

9. **When chaining multiple token transfer or burn calls across contracts, always track who actually holds the tokens at each stage—otherwise you risk charging users twice or failing transactions due to missing funds.**

10. **If your protocol uses `permit()` with user-submitted signatures, you must handle the case where that signature is already used (due to front-running)—otherwise attackers can break your user’s transactions at will.**