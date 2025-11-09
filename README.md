# 🔢 ECDSA Ephemeral Key (`k`) Recovery — Demonstration

This Python snippet demonstrates how to **recompute ephemeral nonces (`k1`, `k2`)** used during the generation of two ECDSA signatures, when the **private key `d`** and all signature components `(r, s, z)` are known.

It’s a simplified, educational example that shows how `k` values are derived inside the ECDSA algorithm — useful for cryptography students, auditors, and researchers investigating nonce reuse vulnerabilities.

---

## 🧩 Overview

In ECDSA, every signature uses a unique, random ephemeral key `k`.  
Each signature satisfies the equation:

\[
s = k^{-1} (z + d \cdot r) \mod n
\]

Where:

- `d` — private key,  
- `z` — hash of the signed message,  
- `r`, `s` — signature components,  
- `n` — order of the elliptic curve group (for secp256k1).

If we **know the private key `d`**, we can reverse the formula and compute:

\[
k = (z + d \cdot r) \cdot s^{-1} \mod n
\]

This script does exactly that — for two different signatures (`r1, s1, z1`) and (`r2, s2, z2`).

---

## ⚙️ How It Works

```python
# Private key (known)
d = 21045754775091014956941825806178930320453004810019895872537059011271830355595

# First signature
r1 = 46159134511846639653039227807867168677952429760806101162575716914492122120852
s1 = 7519772703183545940918986660617875086369147038649256132503899290067419860069
z1 = 96305888925087028226280700902788330707257073607110099029890896029884121755055

# Second signature
r2 = 111616838599096250300489315075857406212435899769031134709979742002100806022869
s2 = 16473844652988003574805773187527026768208893032028674194682143648834372476120
z2 = 82526933124808898216141238576469063794369340677613970807733221005881288311205

# Order of secp256k1 curve
n_secp256k1 = 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFEBAAEDCE6AF48A03BBFD25E8CD0364141

def calculate_k(z1, z2, r1, r2, s1, s2, d, n):
    k1 = (z1 + d * r1) * pow(s1, -1, n) % n
    k2 = (z2 + d * r2) * pow(s2, -1, n) % n
    return k1, k2

k1, k2 = calculate_k(z1, z2, r1, r2, s1, s2, d, n_secp256k1)
print(f"k1 = {k1}")
print(f"k2 = {k2}")
🧮 What It Shows

✅ How to compute the ephemeral key k used in an ECDSA signature
✅ The mathematical link between r, s, z, d, and k
✅ How to verify consistency between two signatures made with the same private key
✅ A foundational step for nonce reuse or nonce difference vulnerability analysis

🧠 When It Works

This script will produce correct k1 and k2 values only when:

The private key d used to create both signatures is known.

The signature values (r1, s1, z1) and (r2, s2, z2) are correct and valid for that key.

The curve order n corresponds to the elliptic curve actually used (e.g., secp256k1 for Bitcoin).

If the data comes from real ECDSA signatures on that curve, the computed k1 and k2 will match the true ephemeral nonces used during signing.

⚠️ Security Notice

⚠️ This code is for educational and testing purposes only.
It illustrates how ephemeral keys can be reconstructed when the private key is already known — or as part of research into ECDSA nonce vulnerabilities.
Do not use it to analyze signatures or keys that you do not own or have permission to test.
Always experiment with testnet or synthetic data.

BTC donation address: bc1q4nyq7kr4nwq6zw35pg0zl0k9jmdmtmadlfvqhr
