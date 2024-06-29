## Title: Inadequate Verification of safe Address in _executeModuleTransaction Function Leading to Potential Execution of Malicious Contracts

### Description

The function _executeModuleTransaction in Helpers.sol accepts an address safe and calls execTransactionFromModule on it. If a malicious user passes an address of a contract that mimics the ISafe interface, they can potentially execute arbitrary code within the context of the Helpers contract. This is because the contract does not verify that the safe address is indeed a legitimate ISafe contract.


#### Impact
Execution of Malicious Contracts

### Proof Of Concept

Attack Scenario

1. Malicious Contract Creation: An attacker creates a contract that implements the ISafe interface, specifically the execTransactionFromModule function.
2. Passing Malicious Contract: The attacker passes the address of this malicious contract to the _executeModuleTransaction function.
3. Execution of Malicious Code: When _executeModuleTransaction calls execTransactionFromModule on the malicious contract, the attacker's code is executed, potentially leading to unauthorized transactions or other malicious actions.

```
/// @dev refactoring of execution of Tx with the privilege of the Module Palmera Labs, and avoid repeat code
    /// @param safe Safe Address to execute Tx
    /// @param data Data to execute Tx
    function _executeModuleTransaction(address safe, bytes memory data)
        internal
        nonReentrant
    {
        ISafe targetSafe = ISafe(safe);
        bool result = targetSafe.execTransactionFromModule(//@audit potential bypass
            safe, uint256(0), data, Enum.Operation.Call
        );
        if (!result) revert Errors.TxExecutionModuleFailed();
    }
```
