---
date: 2026-03-13
updated: 2026-03-13T19:30:00
tags:
  - projects/writeup
---
Digital identity systems have recently received significant attention as governments explore implementing national digital ID schemes. However, these proposals have often faced substantial public backlash, largely due to concerns about privacy, security, and potential misuse. Many citizens are uneasy about the idea of the government maintaining a centralised database containing sensitive identity information, and there is widespread scepticism about whether such systems could be protected against breaches or used to monitor individuals.

On the other hand, the current system of uploading photos of physical IDs and selfies to unaccountable foreign verification companies who frequently fall victim to hacking, and many of whom have at best a dubious reputation, is arguably worse than a government-controlled digital ID.

Sharing some of these concerns, I set out to research whether a digital identity system could be designed in a way that addresses these risks while still functioning as a reliable method of identity verification.
# Design Principles

For such a system to gain public trust while remaining secure and functional, several principles would need to be upheld:

- **Only the government can issue valid identities**
- **Verification of identities can be performed by anyone without contacting the government**
- **When verification is required, only the minimum necessary information is disclosed rather than the entire identity record**
- **There is no cryptographic correlation between the information provided to a verifier and the user**

## Attribute-Based Credentials

One approach that can potentially satisfy these requirements is an **Attribute-Based Credential (ABC)** system supporting **selective disclosure** and **zero-knowledge proofs**.

In this model, identity credentials consist of a collection of attributes (for example, name, date of birth, or nationality) that can be selectively revealed when needed. Instead of presenting the entire credential, the holder can generate a cryptographic proof demonstrating only the information required for a specific interaction.

## BBS Signatures

A practical implementation of this idea can be built using **BBS signatures**, named after Dan Boneh, Xavier Boyen, and Hovav Shacham.

In this scheme:
- A **Signer** (the government) signs multiple messages corresponding to the different attributes contained in an identity credential. These messages might represent fields such as name, date of birth, gender, or nationality.
- The signing process produces a **single fixed-size cryptographic signature** covering all attributes.

Later, the **Prover** (owner of the ID) can generate a proof derived from this signature. During this process, the prover chooses which attributes to disclose while keeping the remaining attributes hidden.

The resulting proof demonstrates that:
- The disclosed attributes originate from a valid credential.
- The credential was signed by the government.
- No information about the hidden attributes is revealed.
## Unlinkable Proofs

BBS signatures also support **unlinkable proofs**.

Each proof generated from a credential is cryptographically independent, meaning that repeated uses of the same credential cannot be linked together. Importantly, even the original signer—the government—cannot trace a proof back to the specific credential instance from which it was derived.

This property prevents large-scale tracking of credential usage and helps preserve the privacy of credential holders.

## Public-Key Cryptography

The system relies on **public-key cryptography**.
- The government, acting as the **Signer**, maintains a secret **private key** used to issue credentials.
- A corresponding **public key** is derived and published.

When a digital ID is issued, its attributes are signed using the government's private key. When a verifier checks a proof generated from that ID, the verification is performed using the publicly available public key.

Because verification only requires the public key, it can be performed by anyone without contacting the government or accessing a central database.

As long as the private key remains secure, the system maintains its integrity: only the government can issue valid credentials, while anyone can independently verify proofs.

```
(1) sign                                      (3) ProofGen
   +-----                                         +-----
   |    |                                         |    |
   |    |                                         |    |
   |   \ /                                        |   \ /
+----------+                                   +-----------+
|          |                                   |           |
|          |                                   |           |
|          |                                   |           |
|  Signer  |---(2)* Send signature + msgs----->|  Holder/  |
|          |                                   |  Prover   |
|          |                                   |           |
|          |                                   |           |
+----------+                                   +-----------+
                                                     |
                                                     |
                                                     |
                                    (4)* Send proof + disclosed msgs
                                                     |
                                                     |
                                                    \ /
                                               +-----------+
                                               |           |
                                               |           |
                                               |           |
                                               | Verifier  |
                                               |           |
                                               |           |
                                               |           |
                                               +-----------+
                                                  |   / \
                                                  |    |
                                                  |    |
                                                  +-----
                                             (5) ProofVerify
```

# Demo

To demonstrate that this system is not purely theoretical, I built a small prototype. The goal was to simulate a simple real-world use case: **age verification for a website**.

The prototype consists of three minimal web applications representing the three roles in the credential system:

- **Issuer** — the government authority that creates and signs credentials  
- **Wallet** — the user application that stores credentials and generates proofs  
- **Verifier** — the website requesting proof of a specific attribute  
## Issuer

The **issuer** interface creates and signs credential attributes.

Each attribute (such as name, date of birth, or age) is treated as a separate message. These messages are signed together using a BBS signature, producing a credential package.

![[Pasted image 20260313184313.png]]

The resulting **credential package** contains the signed attributes and can be transferred to the user.

## Wallet

The credential package is then copied into the **wallet** application. This represents a user-controlled identity wallet where credentials are stored.

![[Pasted image 20260313184450.png]]

From the wallet, the user can generate a proof for a verifier. During this process, the user selects **which attribute to disclose** while keeping the remaining attributes hidden.

In this example, the user chooses to reveal only the **Age** attribute (index 2).

Upon sending the proof, the Verifier responds, confirming that the proof was valid.
## Verifier

The selected attribute and its cryptographic proof are sent to the **verifier**, which represents the website requesting age verification.

![[Pasted image 20260313184651.png]]
The verifier checks the proof using the issuer’s public key. If verification succeeds, the verifier can be confident that:
- the attribute originated from a valid credential
- the credential was issued by the government

Importantly, the verifier receives **only the requested attribute**—in this case, the user's age.
## Public Key Distribution

In this demo, the verifier retrieves the issuer’s public key once during startup from a public endpoint: `issuer.example.com/public-key`
![[Pasted image 20260313184847.png]]  

Because the verifier can validate proofs locally using this public key, no communication with the issuer is required during verification.  
  
As a result, the issuer (the government) has **no visibility into when or where credentials are used**, preserving the privacy of the credential holder.

# Real-World Implementation

In a real deployment, the **wallet** would likely be implemented as a mobile application, a browser extension, or a built-in operating system feature. The wallet would securely store issued credentials and handle proof generation when interacting with verifiers.

The process of receiving a credential would also differ from the simplified demo. Instead of copying a credential package manually, the issuer could deliver credentials through a secure channel such as a **QR code**, a secure download link, or a direct device-to-device transfer. The wallet would import the credential and store it locally under the user's control.

When a verifier requests proof of an attribute, the wallet would generate the proof automatically and transmit it to the verifier through the relevant interface (for example, a browser interaction when visiting a website).

For physical retail, an NFC system such as the one used for contactless payment could also allow verification of age for restricted items.
# Limitations

This prototype here is intentionally minimal and therefore does not address several important challenges that a real-world deployment would need to consider.
### Granularity of Disclosed Information

In the demo, the **age** attribute is disclosed directly. In practice, systems can be designed to reveal even less information.

For example, instead of storing age in the credential, the credential could contain the **date of birth**. When generating a proof, the wallet could perform a cryptographic comparison such as: `date_today - date_of_birth > 18 years`. This is known as a **Predicate Proof**.

The resulting proof would reveal only the boolean result (e.g., *over 18: true*) rather than the user’s exact age or date of birth.

Such a system would also allow for the credential to be issued once, rather than repeatedly each year.

This type of computation is possible using advanced zero-knowledge proof techniques, allowing verifiers to learn only the specific fact they require.

### Credential Sharing

Another challenge is **credential sharing**. A user could potentially transfer their credential package to another person, allowing that person to generate proofs and impersonate the original holder.

One possible mitigation is binding credentials to the device or to the user through **biometric authentication**. For example, a credential could require a fingerprint or other biometric factor when generating proofs.

However, this approach introduces its own trade-offs. If biometric data is collected or centrally managed, it may raise additional privacy concerns and could reduce the trust benefits the system is intended to provide.

Another approach to combat this is **Hardware-Bound Keys**. These are limited to the specific device they were assigned for, and are stored and processed within **Trusted Execution Environments (TEEs)**, rendering unauthorised sharing infeasible .

This approach would however require requesting a credential for each new device

### Credential Revocation

The demo system also lacks a mechanism for **credential revocation**.

In practice, governments must be able to invalidate credentials in cases such as:

- lost or stolen identities  
- fraudulent issuance  
- expired credentials  
- legal changes affecting eligibility  

Designing revocation systems that preserve privacy while allowing verifiers to check credential validity is a non-trivial problem. Potential approaches include revocation lists, short-lived credentials, or cryptographic accumulators, but each introduces additional complexity and trade-offs.

Addressing these issues would be necessary for any production-grade deployment of such a system.
# Conclusion

A privacy-preserving digital ID is technically feasible. While the simple demo presented here does not address all real-world challenges, more advanced systems could overcome these limitations.  

Compared to the current approach of uploading physical ID card photos and selfies to third-party companies, a privacy-preserving digital ID would offer far stronger protection for users. Centralised third-party storage exposes sensitive credentials to risks of hacking, acquisition, or financial failure, potentially resulting in large-scale leaks or misuse.

A well-designed digital ID system which keeps the concerns of citizens as its top priority, such as the one outlined here, would make large-scale tracking by governments or corporations practically impossible, while remaining more robust than physical IDs. Additionally, creating fraudulent digital IDs would be infeasible, unlike physical IDs, which are frequently forged.