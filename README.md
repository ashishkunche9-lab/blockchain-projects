# blockchain-projects

I'm learning blockchain by building small projects in Python, one at a
time. Each project has its own folder with the notebook, a README and
screenshots. All of them run in Google Colab, and the data is
simulated turbine alarms and a few toy accounts.

## Projects

1. [Turbine alarm hash chain](01-turbine-alarm-hashchain): a
   tamper-evident alarm log. If anyone edits or deletes an entry, the
   chain catches it.
2. [Merkle tree](02-merkle-tree): check one record out of many using
   a short proof, without going through everything.
3. [Proof of work](03-proof-of-work): a small miner that shows why
   rewriting a blockchain's history is expensive.
4. [Digital signatures](04-digital-signatures): sign each alarm with
   the operator's key, so you can prove who made an entry.
5. [Mini wallet](05-mini-wallet): signed payments that stop forged
   payments, replays and double spends.

## What I learned
Each project fixes a gap in the one before it. The hash chain shows
that something changed, the Merkle tree makes checking faster, proof
of work makes rewriting costly, signatures show who did it, and the
wallet puts it together to stop double spending.

## Tools
Python, hashlib, cryptography (Ed25519), pandas, matplotlib, Google
Colab.
