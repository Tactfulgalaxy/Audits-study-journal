### Some security tips for Defi  protocols 

1 Always validate incoming payments with a strict require(msg.value == expectedAmount) or at least require(msg.value <= maxAllowed).
Failing to do so risks accidentally trapping funds in your contract  which is like tipping the router an unlimited amount by mistake.

2 When signing or verifying structured data, always encode it in a way that preserves boundaries.... use ABI encoding, delimiters, or length prefixes for each section. Concatenating without separators is like writing “STOPHITAXI”.  it could mean “STOP HIT AXI” or “STOP HI TAXI”, and that ambiguity is dangerous in signature verification.

3 When designing interest rate models in lending protocols, ensure there’s a mechanism to account for and recover surplus funds created by calculation differences. Without a “rescue” or “skim” function, small mismatches in accrual formulas can lead to significant, permanently locked liquidity  even if it’s not immediately felt by users.

4 Always enforce proper access control for functions that can affect core protocol functionality. External functions that manipulate critical state (like starting/canceling raffles) must be restricted to authorized roles (e.g., admin).

5 When handling ERC20 token transfers, avoid redundant balance checks and exact equality comparisons, especially for tokens with transfer fees, deflationary mechanisms, or unusual rounding behaviors. Use established libraries like OpenZeppelin’s SafeERC20 to ensure safe, standardized transfers.
