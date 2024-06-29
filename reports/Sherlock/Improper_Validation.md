## Title: Improper Validation in depositEth Function Allows Bypassing of Pool Type Checks which allow Direct Manipulation of Dai Holdings

### Description

#### Intro

The depositEth() function in the SophonFarming contract does not properly validate the pool type, allowing a malicious user to bypass checks and force the contract to convert DAI to sDAI. This can lead to unintended behavior and potential loss of funds and Dai holdings.

#### vulnerability details

The `depositEth` function allows users to deposit ETH and convert it to either `wstETH` or `weETH` based on the specified PredefinedPool. However, there is no validation to ensure that only these two pool types are allowed. A malicious user can exploit this by calling the function with `PredefinedPool.sDAI`, which is not intended for ETH deposits.

Here is an example of how the vulnerability can be exploited:

1. A malicious user calls the depositEth function with 1 ETH and PredefinedPool.sDAI.
2. The function does not validate the pool type and proceeds to call _depositPredefinedAsset.
3. Inside _depositPredefinedAsset, the _daiTOsDai function is triggered, which converts DAI in the contract to sDAI.
4. This allows the user to force the contract to convert its DAI holdings to sDAI, bypassing all intended checks.

```
function depositEth(uint256 _boostAmount, PredefinedPool _predefinedPool) public payable {
    if (msg.value == 0) {
        revert NoEthSent();
    }

    uint256 _finalAmount = msg.value;
    if (_predefinedPool == PredefinedPool.wstETH) {
        _finalAmount = _ethTOstEth(_finalAmount);
    } else if (_predefinedPool == PredefinedPool.weETH) {
        _finalAmount = _ethTOeEth(_finalAmount);
    }

    _depositPredefinedAsset(_finalAmount, msg.value, _boostAmount, _predefinedPool);
}
```

#### Impact

This vulnerability allows a malicious user to manipulate the contract's DAI holdings, potentially leading to a loss of funds or disruption of the intended functionality of the contract. It can also result in the contract holding an unintended amount of sDAI, which may affect the overall farming strategy and rewards distribution.


