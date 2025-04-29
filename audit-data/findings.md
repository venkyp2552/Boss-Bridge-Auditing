### [H-1] `L1BossBridge::depositTokensToL2` function is sending , Arbitrary from passed to transferFrom (or safeTransferFrom).

**Description:** `L1BossBridge::depositTokensToL2` Passing an arbitrary from address to transferFrom (or safeTransferFrom) can lead to loss of funds, because anyone can transfer tokens from the from address if an approval is made.

```javascript
function depositTokensToL2(address from, address l2Recipient, uint256 amount) external whenNotPaused {
        if (token.balanceOf(address(vault)) + amount > DEPOSIT_LIMIT) {
            revert L1BossBridge__DepositLimitReached();
        }
@>      token.safeTransferFrom(from, address(vault), amount);

        // Our off-chain service picks up this event and mints the corresponding tokens on L2
        emit Deposit(from, l2Recipient, amount);
    }
```
**Impact:** It can lead to loss of funds.

**Proof of Concept:**

**Recommended Mitigation:** 