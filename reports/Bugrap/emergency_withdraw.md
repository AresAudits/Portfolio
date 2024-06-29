## Title: WAVAX rewards or native tokens or eth will be permanently locked in YieldSource Contract

### Description

#### Intro

in `YieldSource.sol` i found that there is no logic implementation of withdraw() or transfer() function,which leads to loss of WAVAX rewards or tokens as there is no logic to withdraw or transfer them.

#### vulnerability details

[YieldSource](https://github.com/struct-defi/struct-contracts/blob/main/contracts/protocol/yield-sources/YieldSource.sol) has two functions i.e `_tokenToShares()` and `_sharesToTokens()` functions with the `receive() external payable` function.this contract receives rewards/tokens/eth as the `receive() external payable` is implemented,but i found that there is no `withdraw` or `transfer` function implementation ,due to this assets will be permanently locked

#### Impact

any asset transfer to yieldSource contract will be permanently locked.

### Proof Of Concept

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.11;

/// Internal imports
import {CustomReentrancyGuard} from "../../utils/CustomReentrancyGuard.sol";
import {GACManaged} from "../common/GACManaged.sol";
import {WadMath} from "../../utils/WadMath.sol";
import {Helpers} from "../libraries/helpers/Helpers.sol";

/**
 * @title  Struct YieldSource contract
 * @author Struct Finance Team
 * @notice Used to supply, recompound rewards and withdraw tokens to and from a yield source.  Exposes interest to Product/Vault contract.
 *             Vaults accrue yield from this contract. Each vault's shares will be calculated via within this contract.
 *             Must be inherited by the platform specific yieldsource contracts, such as AutoPool YieldSource, etc.,
 */
contract YieldSource is GACManaged, CustomReentrancyGuard {
    using WadMath for uint256;

    /// @notice This will be the shares allocated to the first product
    /// @dev Required to prevent share manipulation.
    uint256 public constant INITIAL_SHARES = 1e8;

    /**
     * @notice Calculates the share amount for the given token amount
     * @param _tokenAmount Amount of tokens to be converted to shares
     * @param _currentTotalShares Current total share value within the yield source contract
     * @param _currentTotalExternalShares The total share value of external tokens (specific to underlying YieldSource)
     * @return _equivalentShares Equivalent amount of shares for the given amount of tokens
     */
    function _tokenToShares(uint256 _tokenAmount, uint256 _currentTotalShares, uint256 _currentTotalExternalShares)
        internal
        pure
        returns (uint256 _equivalentShares)
    {
        if (_currentTotalShares == 0 && _tokenAmount > 0) {
            if (_tokenAmount < INITIAL_SHARES) {
                _equivalentShares = INITIAL_SHARES;
            } else {
                _equivalentShares = _tokenAmount;
            }
        } else {
            _equivalentShares = _tokenAmount.mulDiv(_currentTotalShares, _currentTotalExternalShares);
        }
    }

    /**
     * @notice Calculates the token amount for the given share value
     * @param _shares Amount of shares to be converted to tokens
     * @param _currentTotalShares Current total share value within the yield source contract
     * @param _currentTotalExternalShares The total share value of external tokens (specific to underlying YieldSource)
     * @return _equivalentTokens Equivalent amount of tokens for the given share value
     */

    function _sharesToTokens(uint256 _shares, uint256 _currentTotalShares, uint256 _currentTotalExternalShares)
        internal
        pure
        returns (uint256 _equivalentTokens)
    {
        if (_currentTotalShares == 0) {
            _equivalentTokens = 0;
        } else {
            _equivalentTokens = _shares.mulDiv(_currentTotalExternalShares, _currentTotalShares);
        }
    }

    /// Fallback function to receive WAVAX rewards when rewards are recompounded.
    receive() external payable {}//@audit-no withdraw function in this contract can lead permanent loss of assets
}

```

as we can see there is no way to withdraw assets from the contact.

#### Solution

implementing the logic to withdraw the assets will help the contract from assets being locked permanently

```
function withdraw(address _token,address _to) external onlyRole(GOVERNANCE) {

    if(address(_token)==address(0)){
    uint256 balance = address(this).balance;
    require(balance > 0, "Contract has no funds to withdraw");
    _to.transfer(balance);
   }else{
       uint256 balance = _token.balanceOf(address(this));
       require(balance > 0, "Contract has no funds to withdraw");
       _token.safeTransfer(_to,balance);
  }
}
```