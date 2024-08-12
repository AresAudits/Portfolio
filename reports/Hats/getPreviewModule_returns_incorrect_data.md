## Title: getPreviewModule() Returns Incorrect Data

### severity
Medium

### Description
`getPreviewModule()` function in the Helpers.sol returns the 25 modules.However, there is a bug in the external call to safe.getModulesPaginated. In the version of Safe contracts that Palmera is using (version 1.3.0), the getPreviewModules() function returns an incorrect next pointer, resulting in incorrect data being returned. This issue has been fixed in newer versions of Safe contracts, but Palmera still uses version 1.3.0.

```
/// @dev Method to get Preview Module of the Safe
    /// @param safe address of the Safe
    /// @return address of the Preview Module
    function getPreviewModule(address safe) internal view returns (address) {
        // create Instance of the Safe
        ISafe safeInstance = ISafe(safe);
        // get the modules of the Safe
        (address[] memory modules, address nextModule) =
            safeInstance.getModulesPaginated(address(this), 25);

        if ((modules.length == 0) && (nextModule == Constants.SENTINEL_ADDRESS))
        {
            return Constants.SENTINEL_ADDRESS;
        } else {
            for (uint256 i = 1; i < modules.length;) {
                if (modules[i] == address(this)) {
                    return modules[i - 1];
                }
                unchecked {
                    ++i;
                }
            }
        }
    }
```

#### Intro

#### vulnerability details
the issue is present in the version 1.3.0 of getModulesPaginated() function. here the `getmodulesPaginated()` function returns incorrect next module.can u please check the below test case

https://github.com/safe-global/safe-smart-account/commit/743af7f46728bb7018907a151ce649ebe4ffd142

#### Impact

### Proof Of Concept