# 🔐 Secure Code Signing Using Ring-Tesla Signature  

This repository provides an implementation and experimental study of **Secure Code Signing using Ring-Tesla Signatures** – a **lattice-based, post-quantum digital signature scheme**.  

It is based on the research paper:  
📄 *“Secure Code Signing Using Ring-Tesla Signature” by Shaurya Pratap Singh and Brijesh Kumar Chaurasia*  

---

## 📌 Overview  

Software distribution (firmware, operating systems, mobile apps, and containers) requires **strong protection against tampering and forgery**.  

🔑 **Code Signing** ensures:  
- **Integrity** – Code has not been altered  
- **Authenticity** – Confirms the identity of the signer  

However, **traditional methods (RSA/ECC)** are vulnerable to **quantum computing attacks**.  
To overcome this, we propose **Ring-Tesla Signature based Code Signing** – a **post-quantum secure, efficient, and scalable solution**.  

---

## 🚀 Motivation  

- Cyber threats (malware injection, supply-chain attacks) are on the rise.  
- RSA & ECC will be broken by **quantum computers**.  
- Need a **future-proof cryptographic signature** that is:  
  ✅ Efficient  
  ✅ Scalable  
  ✅ Quantum-resistant  

---

## 🛡️ Contributions  

✔️ **Enhanced Security** – Protects against forgery & tampering  
✔️ **Post-Quantum Resistance** – Lattice-based signatures secure against quantum attacks  
✔️ **Efficiency** – Fast key generation, signing, and verification  
✔️ **Scalability** – Supports IoT, OS, firmware, and enterprise software  
✔️ **Interoperability** – Compatible with existing crypto libraries and standards  

---

## ⚙️ System Architecture  

The architecture of the **Code Signing System (CSS)** includes:  

1. **Code Signing System (CSS)** – Signs submitted code using private keys  
2. **Certification Authority (CA)** – Issues certificates to verify signer’s identity  
3. **Timestamp Authority (TSA)** – Adds trusted timestamp during signing  
4. **Verifier** – Confirms signature validity before execution  
5. **Trust Anchors** – Root public keys securely stored for verification  

## ⚙️ System Architecture  

```mermaid
flowchart TD
    A[Developer] --> B[Code Signing System]
    B --> C[Certification Authority]
    B --> D[Timestamp Authority]
    B --> E[Verifier]
    E --> F[End User]
 ```


 ## Key Gen
<code>

function KeyGeneration(λ, a1, a2):
    s, e1, e2 ← Gaussian Distribution
    if CheckE(e1) = 0 or CheckE(e2) = 0:
        Restart
    t1 = a1*s + e1 (mod q)
    t2 = a2*s + e2 (mod q)
    return (SecretKey = (s,e1,e2), PublicKey = (t1,t2))

</code>


## Sign

<code>
function Sign(message µ, a1, a2, s, e1, e2):
    y ← Random sample from Rq
    v1 = a1*y (mod q)
    v2 = a2*y (mod q)
    c′ = H(µ || v1 || v2)
    c = Encode(c′)
    z = y + s*c
    w1 = v1 - e1*c (mod q)
    w2 = v2 - e2*c (mod q)
    if rejection check fails:
        Restart
    return (z, c′)

</code>


## Verify

<code>

function Verify(message µ, signature (z,c′), a1, a2, t1, t2):
    c = Encode(c′)
    w1 = a1*z - t1*c (mod q)
    w2 = a2*z - t2*c (mod q)
    c′′ = H(µ || w1 || w2)
    if c′ = c′′ and z is valid:
        return ACCEPT
    else:
        return REJECT

</code>



| Message Size | KeyGen (ms) | Sign (ms)   | Verify (ms) | Failures |
| ------------ | ----------- | ----------- | ----------- | -------- |
| 500 KB       | 0.008       | 24.66–39.50 | 6.99–14.33  | 20–29    |
| 1000 KB      | 0.007–0.008 | 23.53–42.53 | 6.96–13.67  | 23–26    |
| 1500 KB      | 0.008–0.009 | 25.85–34.64 | 7.13–13.68  | 19–32    |


