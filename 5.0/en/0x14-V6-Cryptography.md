# V6 Stored Cryptography

## Control Objective

The objective of V6 is not only to define best practices but also to instill a fundamental understanding of cryptographic principles and inspire a shift toward more resilient and modern approaches by doing the following:

* Implementing robust cryptographic systems that fail securely, adapt to evolving threats, and are future-proof.
* Utilizing cryptographic mechanisms that are both secure and aligned with industry best practices.
* Maintaining a secure cryptographic key management system with appropriate access controls and auditing.
* Regularly evaluating the cryptographic landscape to assess new risks and adapt algorithms accordingly.
* Discovering and managing cryptographic use cases throughout the application's lifecycle to ensure that all cryptographic assets are accounted for and secured.

In addition to outlining general principles and best practices, this document also provides more in-depth technical information about the requirements in [Appendix V](./0x97-Appendix-V_Cryptography.md).

## V1.6 Cryptographic Inventory and Documentation

Applications need to be designed with strong cryptographic architecture to protect data assets as per their classification. Encrypting everything is wasteful, not encrypting anything is legally negligent. A balance must be struck, usually during architectural or high-level design, design sprints or architectural spikes. Designing cryptography as you go or retrofitting it will inevitably cost much more to implement securely than simply building it in from the start.

Architectural requirements are intrinsic to the entire code base, and thus difficult to unit or integration test. Architectural requirements require consideration in coding standards, throughout the coding phase, and should be reviewed during security architecture, peer or code reviews, or retrospectives.

It is also important to ensure that all cryptographic assets, such as algorithms, keys, and certificates, are regularly discovered, inventoried, and assessed.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **1.6.1** | Verify that there is an explicit policy for management of cryptographic keys and that a cryptographic key lifecycle follows a key management standard such as NIST SP 800-57. | | ✓ | ✓ | 320 |
| **1.6.2** | Verify that consumers of cryptographic services protect key material and other secrets by using key vaults or API based alternatives. | | ✓ | ✓ | 320 |
| **1.6.3** | [DELETED, MERGED TO 6.2.4] | | | | |
| **1.6.4** | [GRAMMAR] Verify that the architecture treats client-side secrets (such as symmetric keys, passwords, or API tokens) as insecure and never uses them to protect or access sensitive data. | | ✓ | ✓ | 320 |
| **1.6.5** | [ADDED] Verify that a cryptographic inventory is performed, maintained, regularly updated, and includes all cryptographic keys, algorithms, and certificates used by the application. | | ✓ | ✓ | 311 |
| **1.6.7** | [ADDED] Verify that cryptographic discovery mechanisms are employed to identify all instances of cryptography in the system, including encryption, hashing, and signing operations. | | ✓ | ✓ | 311 |

## V6.1 Data Classification

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.1.1** | [DELETED, MERGED TO 1.8.1] | | | | |
| **6.1.2** | [DELETED, MERGED TO 1.8.1] | | | | |
| **6.1.3** | [DELETED, DUPLICATE OF 1.8.1] | | | | |

## V6.2 Algorithms

Recent advances in cryptography mean that previously safe algorithms and key lengths are no longer safe or sufficient to protect data. Therefore, it should be possible to change algorithms.

Although this section is not easily penetration tested, developers should consider this entire section as mandatory even though L1 is missing from most of the items.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.2.1** | Verify that all cryptographic modules fail securely, and errors are handled in a way that does not enable Padding Oracle attacks. | ✓ | ✓ | ✓ | 310 |
| **6.2.2** | Verify that industry proven or government approved cryptographic algorithms, modes, and libraries are used, instead of custom coded cryptography. | | ✓ | ✓ | 327 |
| **6.2.3** | [DELETED, DUPLICATE OF 6.2.5] | | | | |
| **6.2.4** | Verify that the application is designed with crypto agility such that random number, encryption or hashing algorithms, key lengths, rounds, ciphers or modes can be reconfigured, upgraded, or swapped at any time, to protect against cryptographic breaks. Similarly, it must also be possible to replace keys and passwords and re-encrypt data. This should allow for seamless upgrades to post-quantum cryptography (PQC), once PQC standards are fully established. | | ✓ | ✓ | 320 |
| **6.2.5** | [SPLIT TO 6.5.1, 6.5.2, 6.6.3] | | | | |
| **6.2.6** | [MOVED TO 6.5.3] | | | | |
| **6.2.7** | [MOVED TO 6.5.4] | | | | |
| **6.2.8** | Verify that all cryptographic operations are constant-time, with no 'short-circuit' operations in comparisons, calculations, or returns, to avoid leaking information. | | | ✓ | 385 |
| **6.2.9** | [ADDED] All cryptographic primitives MUST utilize a minimum of 128-bits of security, with exceptions only made for equipment or applications approaching end of life, where the requirement is at least 112-bits of security for all cryptography. | ✓ | ✓ | ✓ | 311 |

## V6.3 Random Values

Cryptographically secure Pseudo-random Number Generation (CSPRNG) is incredibly difficult to get right. Generally, good sources of entropy within a system will be quickly depleted if over-used, but sources with less randomness can lead to predictable keys and secrets.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.3.1** | [GRAMMAR, LEVEL L2 > L1] Verify that all random numbers and strings which are intended to be non-guessable must be generated using a cryptographically-secure pseudo-random number generator (CSPRNG). | ✓ | ✓ | ✓ | 338 |
| **6.3.2** | [MODIFIED] Verify that UUIDs are created with an implementation of the UUID v4 or v7 algorithms which utilizes a cryptographically-secure pseudo-random number generator (CSPRNG). | | ✓ | ✓ | 338 |
| **6.3.3** | [GRAMMAR, LEVEL L3 > L1] Verify that random number generation works properly under heavy system load, or that the system degrades gracefully. | ✓ | ✓ | ✓ | 338 |

## V6.4 Secret Management

Although this section is not easily penetration tested, developers should consider this entire section as mandatory even though L1 is missing from most of the items.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.4.1** | [MODIFIED, MERGED FROM 2.10.4] Verify that a secrets management solution such as a key vault is used to securely create, store, control access to, and destroy back-end secrets, such as passwords, integrations with databases and third-party systems, seeds and internal secrets, and API keys. Secrets must not be included in source code or be received as CI/CD variables. For a L3 application, this should involved a hardware-backed solution such as an HSM. | | ✓ | ✓ | 798 |
| **6.4.2** | [MODIFIED] Verify that key material is not exposed to the application (neither the front-end nor the back-end) but instead uses an isolated security module like a vault for cryptographic operations. | | ✓ | ✓ | 320 |
| **6.4.3** | [ADDED] Verify that key secrets have defined expiration dates and are rotated on a schedule based on the organization’s threat model and business requirements. | | ✓ | ✓ | 320 |
| **6.4.4** | [ADDED] Verify that access to secret assets adheres to the principle of least privilege. | | ✓ | ✓ | 320 |

## V6.5 Cipher Algorithms

Cipher algorithms such as AES and CHACHA20 form the backbone of modern cryptographic practice.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.5.1** | [ADDED, SPLIT FROM 6.2.5] Verify that insecure block modes (e.g., ECB) and weak padding schemes (e.g., PKCS#1 v1.5) are not used. | | ✓ | ✓ | 326 |
| **6.5.2** | [ADDED, SPLIT FROM 6.2.5, LEVEL L2 > L1] Verify that insecure ciphers, including Triple-DES and Blowfish, are not used but secure ciphers and modes** such as AES with GCM are. | ✓ | ✓ | ✓ | 326 |
| **6.5.3** | [MOVED FROM 6.2.6, LEVEL L2 > L3] Verify that nonces, initialization vectors, and other single-use numbers are not used for more than one encryption key/data-element pair. The method of generation must be appropriate for the algorithm being used. | | | ✓ | 326 |
| **6.5.4** | [MOVED FROM 6.2.7] Verify that encrypted data is authenticated via signatures, including unencrypted tokens being used for secure access control, as well as through authenticated cipher modes or HMAC for protection against unauthorized modification. | | | ✓ | 326 |
| **6.5.5** | [ADDED] Verify that any authenticated signatures are operating in encrypt-then-MAC or encrypt-then-hash modes as required. | | | ✓ | 326 |

## V6.6 Hashing and Hash-based Functions

Cryptographic hashes are used in a wide variety of cryptographic protocols, such as digital signatures, HMAC, key derivation functions (KDF), random bit generation, and password storage. The security of the cryptographic system is only as strong as the underlying hash functions used. This section outlines the requirements for using secure hash functions in cryptographic operations.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.6.1** | [ADDED] Verify that only approved hash functions are used for general cryptographic use cases, including digital signatures, HMAC, KDF, and random bit generation. | | ✓ | ✓ | 916 |
| **6.6.2** | [MODIFIED, MOVED FROM 2.4.1, MERGED FROM 2.4.3, 2.4.4] Verify that passwords are stored using an approved, computationally intensive, hashing algorithm with parameter settings configured based on current guidance. The settings should balance security and performance to make brute-force attacks more challenging. | | ✓ | ✓ | 916 |
| **6.6.3** | [ADDED, SPLIT FROM 6.2.5] Verify that cryptographic systems avoid the use of disallowed hash functions, such as MD5, SHA-1, or any other insecure hash functions, for any cryptographic purpose. | ✓ | ✓ | ✓ | 327 |
| **6.6.4** | [ADDED] Verify that hash functions used in digital signatures are collision resistant and have appropriate bit-lengths to avoid attacks, such as collision or pre-image attacks. | ✓ | ✓ | ✓ | 916 |
| **6.6.5** | [ADDED] Verify that hash functions used in HMAC, KDF, and random bit generation are derived from those with proper entropy seeding for random bit generation. | | ✓ | ✓ | 916 |

## V6.7 Key Exchange Mechanisms

There exists a need for approved key exchange mechanisms, such as Diffie-Hellman and Elliptic Curve Diffie-Hellman (ECDH) to ensure that the cryptosystem remains secure against modern threats.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.7.1** | [ADDED] Verify that industry-proven cryptographic algorithms, such as Diffie-Hellman groups, with a focus on ensuring that key exchange mechanisms use secure parameters to prevent man-in-the-middle attacks or cryptographic breaks, are used for key exchanges to prevent attacks on the key establishment process. | | ✓ | ✓ | 798 |

## V6.8 In-Use Data Cryptography

Protecting data while it is being processed is paramount. Techniques such as full memory encryption, encryption of data in transit, and ensuring data is encrypted as quickly as possible after use is recommended.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.8.1** | [ADDED] Verify that full memory encryption is in use that protects sensitive data while it is in use, preventing access by unauthorized users or processes. | | | ✓ | 798 |
| **6.8.2** | [ADDED] Verify that data minimization ensures the minimal amount of data is exposed during processing, and ensure that data is encrypted immediately after use or as soon as feasible. | | ✓ | ✓ | 798 |

## V6.9 Post-Quantum Cryptography (PQC)

The need to future-proof cryptographic systems in preparation for the eventual rise of quantum computing is critical. Post-Quantum Cryptography (PQC) focuses on developing cryptographic systems that are resistant to quantum attacks, which could break current encryption methods such as RSA and ECC.

| # | Description | L1 | L2 | L3 | CWE |
| :---: | :--- | :---: | :---: | :---: | :---: |
| **6.9.1** | [ADDED] Verify, if the application needs to support post-quantum cryptography, that quantum-safe algorithms, or quantum-resistant algorithms, such as lattice-based (ML-KEM), hash-based, code-based, or multivariate cryptographic schemes, are used as replacements for vulnerable classical algorithms like RSA and ECC. | | ✓ | ✓ | 798 |
| **6.9.2** | [ADDED] Verify that advancements in the field of post-quantum cryptography are being monitored in order to ensure that the application is aligned with emerging industry standards, and remains prepared for quantum threats. | | ✓ | ✓ | 798 |

## References

For more information, see also:

* [OWASP Testing Guide 4.0: Testing for Weak Cryptography](https://owasp.org/www-project-web-security-testing-guide/v41/4-Web_Application_Security_Testing/09-Testing_for_Weak_Cryptography/README.html)
* [OWASP Cheat Sheet: Cryptographic Storage](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)
* [FIPS 140-3](https://csrc.nist.gov/pubs/fips/140-3/final)
