---
theme: seriph
# Enable presenter mode for notes
presenter: true
# Custom fonts for creative look
fonts:
  sans: 'Inter'
  serif: 'Merriweather'
  mono: 'Fira Code'
# Background for all slides
# background: linear-gradient(135deg, #1a1a40 0%, #2a4066 100%)
# Slide transitions for dynamic feel
transition: slide-up
layout: cover
background: ./assets/privacy-bg.jpg
---

# Unlocking Privacy on Solana
**Token2022 Confidential Transfer Extension**

<div class="text-center mt-8">
  <img src="./assets/solanaLogo.png" class="w-32 mx-auto animate-pulse" />
  <p class="text-xl text-neutral-300 mt-4">Privacy for Transactions, Compliance for All</p>
  <p class="text-sm text-neutral-400">Presented by pupplecat | Solana Enthusiast | May 5, 2025</p>
</div>

---
layout: default
transition: slide-up
---

# Agenda

```markdown
1. Solana & Tokens

2. Why Privacy?

3. Token 2022

4. Confidential Transfers

5. Cryptography

6. Use cases

7. Live Demo

8. Q & A
```

---
layout: default
transition: slide-up
---
# SPL Token

```text
                                                     ┌───────────┐
 SPL Token                                           │   Token   │                    ◄─────   Program Account
                                                     └─────┬─────┘
  - mint                                                   │
  - transfer                                        own    │    own
                                               ┌───────────┴───────────┐
                                               │                       │
                                         ┌─────▼──────┐          ┌─────▼──────┐
                                         │    Mint    │          │  Account   │       ◄─────   State Account
                                         └────────────┘          └────────────┘

                                          - supply                 - mint
                                          - authority              - owner            ◄-----   Logical Owner
                                          - decimals               - amount
                                                                   - delegate

                                   Program ID: TokenkegQfeZyiNwAJbNbGKPFXCWuBvf9Ss623VQ5DA
```

---
layout: image-right
image: ./assets/blockchain-privacy.png
transition: slide-up
---

# Why Privacy in Blockchain?

- **Public Ledgers**: All amounts visible
- **Need for Confidentiality**:
  - Protect sensitive payments
  - Enable institutional adoption
  - Meet regulatory compliance
- **Confidential Transfer**: Hides amounts, not identities

<div class="text-sm italic mt-4 animate-pulse">
  “Privacy for payments, transparency for regulators.”
</div>

---
layout: default
transition: slide-up
---

# Token2022: Beyond SPL Token

```text

                                                     ┌────────────┐
 SPL Token 2022                                      │ Token 2022 │                   ◄─────   Program Account
                                                     └──────┬─────┘
  - confidential transfer                                   │
  - fees                                             own    │    own
  - hooks                                       ┌───────────┴───────────┐
  - metadata                                    │                       │
  - etc.                                  ┌─────▼──────┐          ┌─────▼──────┐
                                          │    Mint    │          │  Account   │      ◄─────   State Account
                                          └────────────┘          └────────────┘
                                     ─    *   Ext #1   *          *   Ext #1   *      ─
                                     │    **************          **************      │
                                     │    *    ...     *          *    ...     *      │
                     not extendable  │    **************          **************      │   extendable
                                     │    *   Ext #n   *          *   Ext #n   *      │
                                     ─    **************          **************      ─


                                   Program ID: TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb
```

---
layout: default
transition: slide-up
---

# Confidential Transfer Extension

```text

 Confidential Transfer


   What It Does :
        - Hides transfer amounts                                                          Mint
        - Keeps sender/receiver public                                             ┌─────────────────┐
        - Optional auditor key                                Confidential mint    │   Account Data  │
                                                                                   ├─────────────────┤
   Workflow :                                               - confidential_mint    │     CT Ext.     │
        1. Create mint with extension                       - confidential_burn    └─────────────────┘
        2. Configure accounts
        3. Deposit to encrypted balance
        4. Transfer confidentially
        5. Apply/withdraw balance


                                Account A         1. deposit          4. withdraw      Account B
                            ┌─────────────────┐                                    ┌─────────────────┐
                Public Data │   Account Data  │ ───┐                          ┌──► │   Account Data  │
                            ├─────────────────┤    │                          │    ├─────────────────┤ 3. apply_pending
             Encrypted Data │     CT Ext.     │ ◄──┘      2. ct_transfer      └─── │     CT Ext.     │    _balance
                            └─────────────────┘ ─────────────────────────────────► └─────────────────┘

```


---
layout: center
transition: slide-up
---

# Cryptography Behind It

<div class="grid grid-cols-2 gap-6">
  <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg">
    <h3 class="font-bold text-lg">Zero-Knowledge Proofs in CT</h3>
    <ul class="text-sm">
      <li><strong>CiphertextValidityProof</strong>: Validates encrypted transfer</li>
      <li><strong>EqualityProof</strong>: Matches amounts via commitments (not direct compare)</li>
      <li><strong>RangeProof</strong>: Ensures new balance is 0 to 2⁶⁴</li>
    </ul>
  </div>
  <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg">
    <h3 class="font-bold text-lg">Role of ElGamal Key</h3>
    <ul class="text-sm">
      <li><strong>Encryption</strong>: Hides balances (asymmetric)</li>
      <li><strong>Homomorphic</strong>: Enables encrypted additions</li>
      <li><strong>Stored</strong>: Public key in token account</li>
    </ul>
  </div>
  <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg">
    <h3 class="font-bold text-lg">Role of AE Key</h3>
    <ul class="text-sm">
      <li><strong>Owner Access</strong>: Encrypts decryptable balance</li>
      <li><strong>Updates</strong>: Refreshed on key operations</li>
      <li><strong>Privacy</strong>: Only owner decrypts; auditors cannot</li>
    </ul>
  </div>
  <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg">
    <h3 class="font-bold text-lg">Auditor Role</h3>
    <ul class="text-sm">
      <li><strong>Decrypt Amount</strong>: Views transfer amount</li>
      <li><strong>Validate Proofs</strong>: Checks ZK proof integrity</li>
      <li><strong>No Balance Access</strong>: Cannot see full balances</li>
    </ul>
  </div>
</div>


---
layout: center
transition: slide-up
---

# CT Transfer Step-by-Step (Unencrypted View)

```text

 Zero Knowledge Proof in Confidential Transfer


    Account A has 100 USDC and is transfering 20 USDC to Account B


                Account A                                                             Account B

before :    - available_balance = 100    ===>   transfer_amount = 20    ===>       - available_balance = 0
                                                                                   - pending_balance = 0
                                           ............. ...................
                                           . Zero-knowledge proof          .
                                           .   - Ciphertext validity proof .
                                           .   - Equality proof            . ◄─── Verified by zk elgamal program
                                           .   - Range proof               .
                                           .................................

 after :    - available_balance = 80                                               - available_balance = 0
                                                                                   - pending_balance = 20  ✅


 ZK ElGamal Proof Program ID: ZkE1Gama1Proof11111111111111111111111111111
```

---
layout: image-left
image: ./assets/use-case.png
transition: slide-up
---

# Use Cases & Benefits

- **Use Cases**:
  - Stablecoins (Paxos USDP)
  - Payroll (hidden salaries)
  - B2B settlements
  - Compliance (auditor keys)
- **Benefits**:
  - Native integration
  - Scalable on Solana
  - Compliance-friendly
- **Challenges**:
  - Limited wallet support
  - Extension conflicts


---
layout: center
transition: slide-up
---

# Live Demo: Hiding the Amount

- **Objective**: Create a token and transfer it confidentially, showing the amount is encrypted.
- **Tools**: Solana CLI, local validator
- **Github**: [pupplecat/token-2022-confidential-transfer-example](https://github.com/pupplecat/token-2022-confidential-transfer-example)
- **Steps**:
  1. Create confidential mint
  2. Set up accounts
  3. Deposit to encrypted balance
  4. Transfer (amount hidden)
  5. Check Solana Explorer

<div class="text-sm mt-4 animate-bounce">
  Watch the amount disappear!
</div>

<img src="./assets/live-demo.png" class="w-48 mx-auto mt-4 animate-pulse" />

---
layout: center
transition: slide-up
---

# Key Takeaways

```
- Privacy:    Hides amounts with Confidential Transfer
- Secure:     ElGamal encryption, Bulletproofs
- Practical:  Stablecoins, payroll, compliance
- Accessible: Easy with Solana CLI
```



---
layout: center
transition: slide-up
---

# Q&A and Resources

<div class="grid grid-cols-2 gap-6">
  <div>
    <h3 class="font-bold">Ask Away!</h3>
    <p class="text-sm">What’s on your mind? Use the chat!</p>
  </div>
  <div>
    <h3 class="font-bold">Resources</h3>
    <ul class="text-sm">
      <li><a href="https://solana.com/docs/tokens/extensions/confidential-transfer">Solana Docs</a></li>
      <li><a href="https://github.com/solana-program/token-2022">GitHub</a></li>
      <li><a href="https://spl.solana.com/confidential-token/quickstart">CLI Guide</a></li>
    </ul>
  </div>
</div>

<img src="./assets/qa.png" class="w-24 mx-auto mt-8 animate-pulse" />
<PoweredBySlidev class="mt-10 text-xs" />


---
<style>
.animate-pulse {
  animation: pulse 2s infinite;
}
.animate-bounce-in {
  animation: bounceIn 1s ease-in-out;
}
.animate-slide-in-right {
  animation: slideInRight 1s ease-in-out;
}
.animate-spin-slow {
  animation: spin 10s linear infinite;
}
@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
@keyframes bounceIn {
  0% { transform: scale(0.3); opacity: 0; }
  50% { transform: scale(1.05); opacity: 1; }
  100% { transform: scale(1); }
}
@keyframes slideInRight {
  0% { transform: translateX(100%); opacity: 0; }
  100% { transform: translateX(0); opacity: 1; }
}
@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
</style>