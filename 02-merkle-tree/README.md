Merkle Tree: Checking One Record Without Checking Them All

Why I built this
My first project was a hash chain for turbine alarms. It worked, but
there was a catch. To check even one record, you have to walk through
the whole chain. With thousands of alarms, that gets slow.

I read that real blockchains use something called a Merkle tree for
this, so I built a small one in Python to understand it myself.

How it works
- Each record gets hashed. Those hashes are the bottom of the tree.
- The hashes are joined in pairs and hashed again, level after level,
  until only one hash is left. That one is the Merkle root.
- If any record changes, its hash changes, and the change travels all
  the way up. So if the root still matches, nothing was touched.
- To prove one record belongs to the set, you don't need all the
  data. You just need a short proof, which is the neighbouring
  hashes on the way up. For 8 records, that is only 3 hashes.
- If a level has an odd number of hashes, the last one is paired with
  itself.
- The records are simulated turbine alarms: vibration, bearing
  temperature and lube oil pressure.

What I tested
I ran 5 checks in the notebook, and all of them passed:

- A real record with its correct proof was accepted.
- The same record after I edited its value was rejected.
- A proof taken from a different record was rejected.
- A fake root hash was rejected.
- All 8 records verified correctly, one by one.

The notebook prints PASS for each check. For the three "rejected" cases,
PASS means the tree correctly refused the bad input.

Checking one record out of 8 needed only 3 hashes, not all 8.

What it can't do
- The root must be stored somewhere safe. If an attacker can change
  the root too, the tree proves nothing.
- Pairing the last hash with itself is the simple way. Real systems
  handle odd levels more carefully.
- It runs on one machine and the data is simulated.
- It tells you that something changed, not who changed it. Digital
## Try it
Open `merkle_tree.ipynb` in Google Colab and run the cells in order.

Screenshots
![Valid records](green_table.png)
![Tampered record](red_table.png)
![Proof tests](proof_tests.png)
