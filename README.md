# AresAudits - Portfolio

Welcome to my GitHub portfolio! I am a dedicated Smart Contract Security Researcher with a strong focus on ensuring the safety and reliability of decentralized applications. This repository showcases my work, contributions, and research in the field of smart contract security.

## 🛡️ About Me

I am passionate about blockchain technology, cryptography, and secure coding practices. My expertise includes conducting comprehensive security audits, developing security tools, and responding to security incidents within the blockchain ecosystem.

## 🚀 Findings


|Platform       | Title                     | Severity   | Link
--------------  | ------------------------- | ---------- | -------------------------------------
| [CodeHawks](https://codehawks.cyfrin.io/) | Allowance issue in transfer function | High | [Link](https://codehawks.cyfrin.io/c/2024-08-tadle/s/1552)
|  | AuthorityRate can be bypassed | High | [Link](https://codehawks.cyfrin.io/c/2024-08-tadle/s/1716)
|  | Reentrancy in withdraw() function | High | [Link](https://codehawks.cyfrin.io/c/2024-08-tadle/s/1672)
|  | Platform Fees Can Be Bypassed for Low Decimal Standard ERC20 Tokens | Medium | [Link](https://codehawks.cyfrin.io/c/2024-08-tadle/s/1706)
|  | EIP712MetaTransaction.executeMetaTransaction() failed txs are open to replay attacks | Medium | [Link](https://codehawks.cyfrin.io/c/2024-11-one-world/s/836)
|[BugRap](https://bugrap.io/)   | by staking very small amount of tokens,attacker steals the user funds and here user gets 0 shares in return         | High       | [Link](/reports/Bugrap/donation_attack.md)
|                               | [WAVAX rewards or native tokens or eth will be permanently locked in YieldSource Contract]  | Medium | [link](/reports/Bugrap/emergency_withdraw.md)
|               | precision loss in _calculateBoxPrice() function leads to loss of funds | Medium | [Link](/reports/Bugrap/precision_loss.md)
|[HackenProof](https://hackenproof.com/) | Incorrect Utilization Rate Calculation Leads to Inaccurate Borrow Rate in getBorrowRate() Function | High     |[Link](/reports/HackenProof/Incorrect_UR.md)
|[Hats Finance](https://hats.finance/security-researchers) | Bypass of isSafe Validation Allows Malicious Contract Registrations and Spam the Palmera Module contracts with System-Wide Exploitation | High   | [Link](/reports/Hats/Bypass.md)
|               | Potential Vulnerability in execTransactionOnBehalf Function Allowing Destruction of targetSafe contract | Medium | [Link](/reports/Hats/Destruction_of_targetSafe.md)
|  | Inadequate Verification of safe Address in _executeModuleTransaction Function Leading to Potential Execution of Malicious Contracts | High | [Link](/reports/Hats/Inadequate_Verification.md)
|  | Signature Malleability in WebAuthn.sol | Medium | [Link](/reports/Hats/Signature_Malleability.md)
|  | user tokens will be fully vested before the duration,incorrect logic implementation | Medium | [Link](/reports/Hats/incorrect_logic.md)
|  | processAndSortSignatures Function Fails to Handle Multiple Valid Signatures for Owners, Leading to Reversions in checkNSignatures Function if Threshold is Greater Than 1 | Medium | [Link](/reports/Hats/palmera.md)
|[Sherlock](https://www.sherlock.xyz/)   | Improper Validation in depositEth Function Allows Bypassing of Pool Type Checks which allow Direct Manipulation of Dai Holdings         | Medium     | [Link](/reports/Sherlock/Improper_Validation.md)
|  | Incorrect Handling of Fee-on-Transfer Tokens in ManagedBudget | Medium | [Link](https://github.com/sherlock-audit/2024-06-boost-aa-wallet-judging/issues/394)
|  | Approval Race Condition for USDT Token | Medium | [Link](https://github.com/sherlock-audit/2024-08-sentiment-v2-judging/issues/593)



## 🌐 Connect with Me
Feel free to reach out to me through:
- **[Twitter](https://x.com/_AresAudits)**
- **[Email](aresaudits@gmail.com)**


---
