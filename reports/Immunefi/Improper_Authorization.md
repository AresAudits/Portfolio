## Title: Improper Authorization in `claimReward()` Function

### severity
 Med

### Issue Description

**Bug Description:**

The `VaultFeeReward` contract manages rewards for liquidity providers, featuring two critical functions: `claimReward()` and `reinvest()`. The `claimReward()` function is designed to transfer claimable rewards to users, while `reinvest()` allows users to reinvest or stake these rewards back into the system. However, there is a critical oversight in the `claimReward()` function where improper authorization is present. This allows any caller to invoke `claimReward()` on behalf of any user address, thereby preventing legitimate users from reinvesting their rewards if they choose to do so.

**Impact:**

This vulnerability allows unauthorized parties to claim rewards on behalf of other users, effectively locking out legitimate users from reinvesting their rewards as intended.

**Risk Breakdown:**

- **Difficulty to Exploit:** Very Easy
  - Exploiting this issue requires minimal effort as there are no restrictions on invoking `claimReward()` on behalf of any user, making it straightforward to prevent reinvestment of rewards.

**Recommendation:**

Implement proper authorization in the `claimReward()` function to ensure that only the authenticated user can execute this function for themselves. Below is a recommended approach:

```solidity
function claimReward() public returns (uint256 rewardToSend) {
    updateReward(msg.sender);
    rewardToSend = claimableReward[msg.sender];
    claimableReward[msg.sender] = 0;

    if (rewardToSend > 0) {
        _transferOut(msg.sender, rewardToSend);
        emit ClaimedReward(
            msg.sender,
            rewardToken,
            rewardToSend
        );
    }
}
```

## Proof of Concept

Vulnerable Function:
```
function claimReward(address user) public returns (uint256 rewardToSend) {
    updateReward(user);
    rewardToSend = claimableReward[user];
    claimableReward[user] = 0;

    if (rewardToSend > 0) {
        _transferOut(user, rewardToSend);
        emit ClaimedReward(
            user,
            rewardToken,
            rewardToSend
        );
    }
}

```

### Exploit Scenario:

Call claimReward() with Arbitrary User Address:
Any external entity can call claimReward() with a user's address (user), triggering the transfer of rewards to that user.
Impact:
The rewards are transferred to the user, but the user cannot reinvest them because the transaction was unauthorized by the user themselves.
This scenario forces the user to start over in their investment process, potentially resulting in missed profits or disruption of their investment strategy.
By implementing proper authorization checks and ensuring that only the authenticated user can claim their rewards, the contract can safeguard against unauthorized claims and allow users full control over their reinvestment decisions.