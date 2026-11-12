# Mini Wallet: Payments and Double-Spend Protection

## Why I built this
My first four projects were about protecting records: spotting edits,
proving who wrote them, and making rewrites expensive. But a
blockchain is mostly used for money, and money has a problem that
plain records don't. Someone can try to spend the same coins twice.

So I built a small wallet system in Python to see how a ledger stops
that, and other tricks like forged payments and replays.

## How it works
- There are three accounts: Ravi, Sita and Kiran. Each has a key pair
  and starts with 100 coins.
- A payment says who sends, who receives, how much, and a counter
  called a nonce. The sender signs it with their private key
  (Ed25519, same as Project 4).
- Every sender has a nonce that starts at 0 and goes up by one after
  each accepted payment. A payment is only accepted if it carries the
  next expected nonce. This is what stops replays and double spends.
- Before accepting a payment, the ledger checks in order: the
  accounts exist, the signature is valid, the amount is positive,
  the nonce is correct, and the sender has enough coins.
- Accepted payments are saved in a ledger. An audit function can
  replay the whole ledger from scratch and check that every
  signature, nonce and balance still adds up.
- The coins are just numbers in a simulation. They have no real value.

## What I tested
I tried 9 payments in a row. 3 were accepted and 6 were rejected:

- Ravi sends 30 to Sita: accepted.
- Sita sends 50 to Kiran: accepted.
- Ravi sends 500 with only 70 coins: rejected, insufficient balance.
- Kiran forges a payment from Ravi using his own key: rejected, bad
  signature.
- Someone replays Ravi's first payment: rejected, nonce already used.
- Sita double-spends. She signs two payments of 70, one to Kiran and
  one to Ravi, using the same coins. The first is accepted. The
  second is rejected because that nonce was already used.
- Ravi sends -10 to Sita, trying to steal coins: rejected, invalid
  amount.
- Ravi sends 10 to an account that does not exist: rejected, unknown
  account.

Final balances were Ravi 70, Sita 10 and Kiran 220. The total is still
300, so no coins were created or lost, and nobody went negative.

I also audited the ledger from scratch. It passed. Then I edited one
transaction and the audit failed with a bad signature. When I deleted
one transaction, the audit failed because the later balances no longer
added up.

## What it can't do
- There is one ledger on one machine, and you have to trust whoever
  runs it. Real blockchains have many nodes that must agree on the
  order of payments. Proof of work from Project 3 is one way they do
  that.
- Double-spend protection here works because one program sees every
  payment in order. In a network, two payments can reach different
  nodes at the same time, and that is the hard part I did not build.
- There are no blocks, no fees, and no network.
- Private keys are generated fresh in the notebook. A real wallet has
  to store keys safely, and a stolen key means stolen coins (see
  Project 4).
- The accounts and coins are simulated.

## Try it
Open `mini_wallet.ipynb` in Google Colab and run the cells in order.
Run the payment cell only once. If you run it again, the balances
change, so start again from the first cell.

## Screenshots
![Payments table](tx_table.png)
![Audit and balances](audit_checks.png)
