# Things to watch for in Defi protocols. 

## 1. Incentive logic must match intention  
Saw a case where lenders were meant to get more profit as APR increased, but the logic did the opposite. Tiny condition mistake flipped the outcome. Always double-check if your incentives are actually being enforced by the code.

## 2. Misused token references can break things silently  
One finding was about referencing the wrong token for balance checks. It didn’t crash anything—but it silently made numbers wrong. Always confirm you’re using the correct token contract in every function that interacts with balances.

## 3. Dust is real and needs handling  
Leftover tiny amounts ("dust") can accumulate. Might seem harmless, but if ignored, users can’t fully withdraw, or the protocol might misreport totals. Some kind of dust collection or sweep function helps avoid this.

## 4. `balanceOf` isn’t always reliable  
Lots of protocols just use `balanceOf` without thinking if it applies in that context. For example, tokens could be stuck in a wrapper or a different contract. Make sure you're querying the right address and token.

## 5. Fixes ≠ safe  
A one-line fix might solve the problem—but without test coverage, it could create a new one. Every “fix” should be followed by regression tests, especially if it touches profit distribution or core lending flows.

## 6. Don’t ignore thresholds and edge values  
APR boundaries, minimum debt size, rounding effects—these edge cases break things often. Think in terms of: what happens at 0? What happens at max? What happens just below/above a key threshold?

## 7. Users should never be surprised  
One common thread: bad logic = bad UX. If a user expects 77% of profits and gets 25%, they’ll lose trust even if it’s a code mistake. Make sure behaviors are predictable and explainable.