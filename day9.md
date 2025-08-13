Security Lessons for the day. 

1. **Enforce Role & Permission Checks Consistently**
Any lapse in permission checks . Even in one function, creates an entry point for privilege abuse.


2. **Validate All Recipients Before Transfers**
Distribution logic should reject ineligible recipients before tokens move, preventing irreversible mistakes.


3. **Design for Safe Role Changes**
Access changes shouldn’t strand funds; build recovery paths into permission systems.


4. **Handle Special Addresses Explicitly**
Burn, zero, or reserved addresses must be treated intentionally to avoid silent exploits.


5. **Check Both Sender and Receiver Rules**
Restrictions fail if only one side of a transfer is validated,verify both ends every time.


6. **Test for Math & Boundary Edge Cases**
Off-by-one, rounding, or equality conditions can bypass rules you thought were watertight.


7. **Defend Against Market Manipulation**
Use averaged or delayed pricing so attackers can’t game your logic in a single block.


8. **Control Randomness to Avoid Certainty**
Extreme inputs should never make “random” outcomes predictable or guaranteed.


9. **Require Initialization Before User Actions**
Critical variables left unset can nullify your core security assumptions.


10. **Lock Resources at the Moment of Commitment**
Without early reservation, the same funds can be promised to multiple users, leading to DoS or theft.