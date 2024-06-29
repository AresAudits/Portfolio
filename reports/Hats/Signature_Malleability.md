## Title: Signature Malleability in WebAuthn.sol

### severity
 High

### Description

The WebAuthn.sol library is designed to verify valid signatures and prevent signature malleability. However, in the verifySignature function, there is an incorrect function call. Instead of calling the verifySignature function in P256.sol, which checks if s > _N_DIV_2, it calls the verifySignatureAllowMalleability function. This oversight allows attackers to slightly modify the signature without invalidating it, potentially bypassing signature validity measures


#### Impact

Attacker can able to craft a valid signature without knowing the corresponding private key

### Proof Of Concept

An attacker can exploit this vulnerability by creating a different but valid signature for the same message. This can lead to replay attacks and other security issues, as the system would accept the modified signature as valid, bypassing the intended security checks.

working on it using Foundry to demonstrate this vulnerability and the proposed fix. will post the PoC in the comments shortly.


#### Revised Code File (Fix)

```
function verifySignature(
    bytes32 challenge,
    Signature calldata signature,
    AuthenticatorFlags authenticatorFlags,
    uint256 x,
    uint256 y,
    P256.Verifiers verifiers
) internal view returns (bool success) {
    // The order of operations here is slightly counter-intuitive (in particular, you do not
    // need to encode the signing message if the expected authenticator flags are missing).
    // However, ordering things this way helps the Solidity compiler generate meaningfully more
    // optimal code for the "happy path" when Yul optimizations are turned on.
    bytes memory message = encodeSigningMessage(challenge, signature.authenticatorData, signature.clientDataFields);
    if (checkAuthenticatorFlags(signature.authenticatorData, authenticatorFlags)) {
        // Call the correct function to ensure signature malleability is checked
        success = verifiers.verifySignature(_sha256(message), signature.r, signature.s, x, y);
    }
}

```

To fix this vulnerability, update the verifySignature function in WebAuthn.sol to call the verifySignature function instead of verifySignatureAllowMalleability. This ensures that the signature malleability check is performed, enhancing the security of the signature verification process.