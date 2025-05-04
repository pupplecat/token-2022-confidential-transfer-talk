```
             │                                                                                                                      │
             │                                                                                                                      │
             │                                                                                                                      │
─────────────                                                                                                                        ────────────
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








             │                                                                                                                      │
             │                                                                                                                      │
             │                                                                                                                      │
─────────────                                                                                                                        ────────────
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
                                                 ─     *   Ext #1   *          *   Ext #1   *      ─
                                                 │     **************          **************      │
                                                 │     *    ...     *          *    ...     *      │
                                 not extendable  │     **************          **************      │   extendable
                                                 │     *   Ext #n   *          *   Ext #n   *      │
                                                 ─     **************          **************      ─


                                                Program ID: TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb








             │                                                                                                                      │
             │                                                                                                                      │
             │                                                                                                                      │
─────────────                                                                                                                        ────────────
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