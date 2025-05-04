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
background: linear-gradient(135deg, #1a1a40 0%, #2a4066 100%)
# Slide transitions for dynamic feel
transition: slide-up
addons:
  # - naive
naive:
  # see https://naiveui.com/en-US/os-theme/docs/customize-theme
  common:
    # primaryColor: "#0000FF"
  Button:
    # textColor: "#FF0000"
  lightThemeOverrides:
  darkThemeOverrides:
---

# Unlocking Privacy on Solana
**Token2022 Confidential Transfer Extension**

<div class="text-center mt-8">
  <img src="./assets/solanaLogo.svg" class="w-32 mx-auto animate-pulse" />
  <p class="text-xl text-gray-300 mt-4">Privacy for Transactions, Compliance for All</p>
  <p class="text-sm text-gray-400">Presented by pupplecat | May 3, 2025</p>
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

8. Q&A
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
        - Optional auditor key                      Confidential mint is optional  │   Account Data  │
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
transition: zoom
---

# Cryptography Behind It

<div class="grid grid-cols-2 gap-6">
  <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg">
    <h3 class="font-bold text-lg">Zero-Knowledge Proofs in CT</h3>
    <ul class="text-sm">
      <li><strong>Bulletproofs</strong>: Prove validity without revealing amounts</li>
      <li><strong>Range Proof</strong>: Ensures balance ≥ 0 and < 2⁶⁴</li>
      <li><strong>Equality Proof</strong>: Confirms sender amount = receiver amount</li>
    </ul>
  </div>
  <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg">
    <h3 class="font-bold text-lg">Role of ElGamal Key</h3>
    <ul class="text-sm">
      <li><strong>Encryption</strong>: Hides balances and transfer amounts</li>
      <li><strong>Homomorphic</strong>: Allows encrypted arithmetic (e.g., additions)</li>
      <li><strong>Public Key</strong>: Used by sender/receiver for secure data</li>
    </ul>
  </div>
  <div class="bg-white bg-opacity-10 p-4 rounded-lg shadow-lg">
    <h3 class="font-bold text-lg">Role of AE Key</h3>
    <ul class="text-sm">
      <li><strong>Access Encryption</strong>: Secures transfer proposals</li>
      <li><strong>Decryption</strong>: Allows receiver to unlock encrypted amounts</li>
      <li><strong>Confidentiality</strong>: Prevents unauthorized viewing</li>
    </ul>
  </div>
</div>



---
layout: image-left
image: https://via.placeholder.com/400x600.png?text=Use+Cases
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
transition: fade
---

# Live Demo: Hiding the Amount

- **Objective**: Create a token and transfer it confidentially, showing the amount is encrypted.
- **Tools**: Solana CLI, local validator
- **Steps**:
  1. Create confidential mint
  2. Set up accounts
  3. Deposit to encrypted balance
  4. Transfer (amount hidden)
  5. Check Solana Explorer

<div class="text-sm mt-4 animate-bounce">
  Watch the amount disappear!
</div>

<img src="https://via.placeholder.com/300x200.png?text=Encrypted+Transfer" class="w-48 mx-auto mt-4 animate-pulse" />


---
layout: center
class: bg-purple-900 text-white
transition: zoom
---

# Key Takeaways

<div class="text-left max-w-3xl mx-auto">
  - **Privacy**: Hides amounts with Confidential Transfer
  - **Secure**: ElGamal encryption, Bulletproofs
  - **Practical**: Stablecoins, payroll, compliance
  - **Accessible**: Easy with Solana CLI
</div>

<img src="https://solana.com/branding/logomark.png" class="w-24 mx-auto mt-8 animate-spin-slow" />

---
layout: center
class: bg-blue-900 text-white
transition: fade
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
      <li><a href="https://spl.solana.com/token-2022">Solana Docs</a></li>
      <li><a href="https://github.com/solana-labs/solana-program-library">GitHub</a></li>
      <li><a href="https://spl.solana.com/confidential-token">CLI Guide</a></li>
    </ul>
  </div>
</div>

<img src="https://via.placeholder.com/100x100.png?text=QR+Code" class="w-24 mx-auto mt-8 animate-pulse" />


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