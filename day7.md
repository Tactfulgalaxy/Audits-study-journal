# 🛡️ Daily Smart Contract Security Tips



## **1. Never trust state you don’t own**
Anything you read from another contract (balances, allowances, prices, etc.) can change instantly.  
- Re-check important values right before you use them.  
- Keep internal records where possible.  
- Don’t assume a read value will still be correct later in the same transaction.  



## **2. Every external call is an attack surface**
Even if you call a “trusted” contract today, it might be upgraded or hacked tomorrow.  
- Always check the result of an external call.  
- Use safe wrappers like `SafeERC20` for token transfers.  
- Whitelist known addresses if the logic allows it.  



## **3. Tokens don’t always behave like you expect**
Not all ERC-20s follow the rules — some take fees, some don’t revert, some rebase supply.  
- Check balances before and after transfers.  
- Don’t assume `transfer()` will send exactly what you requested.  
- Write logic that can handle weird token behavior.  



## **4. Be careful with prices from on-chain sources**
Spot prices from AMM pools can be manipulated in a single block.  
- Use TWAP or an external oracle like Chainlink for critical logic.  
- Avoid making big decisions based on a single on-chain price read.  
- Add delays for sensitive price-based actions if possible.  



## **5. Keep checks and actions close together**
If you check something and then act on it later, that value can change before execution.  
- Follow the Checks-Effects-Interactions pattern.  
- Validate inputs right before using them.  
- Avoid long gaps between checking and acting.