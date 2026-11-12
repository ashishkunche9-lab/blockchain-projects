Digital Signatures: Knowing Who Made the Entry

Why I built this
My first projects could tell me that a record was changed. But none
of them could tell me who made a record in the first place. In a
plant, that matters. If an alarm is acknowledged, you want proof of
which operator did it, and you want that person to be unable to
deny it later.

Digital signatures are how real blockchains solve this, so I built a
small version in Python to understand it.

How it works
- Each operator (Ravi, Sita, Kiran) has two keys. The private key is
  secret and is used to sign. The public key is shared with everyone
  and is used to check.
- When an operator logs an alarm, the record (id, operator, alarm,
  value, time) is signed with their private key using Ed25519.
- Anyone with the operator's public key can verify the signature.
  If even one character of the record changed, the check fails.
- A signature made with a different key fails too, so nobody can
  pretend to be someone else.
- The alarms are simulated turbine alarms: vibration, bearing
  temperature and lube oil pressure.

What I tested
I tried 8 cases on one signed record. Here is what came out:

- **No change:** VALID.
- **Editing the value:** INVALID.
- **Editing the alarm name:** INVALID.
- **Putting a different operator's name on the record:** INVALID, so
  you cannot blame someone else.
- **Forging a record as Ravi with the attacker's own key:** INVALID.
- **Using an operator that is not registered:** UNKNOWN OPERATOR.
- **Copying the signature from another record:** INVALID, because a
  signature only fits the exact record it was made for.
- **Signing with Ravi's stolen private key:** VALID.

That last one is not a bug. It is the main limitation, and I
explain it below.

What it can't do
- If a private key is stolen, the attacker can sign anything as that
  operator and the system accepts it. The key has to be kept safe,
  for example in a hardware device.
- A signature protects one record, but nothing links records
  together. Someone could delete a signed record and nobody would
  notice. That is why this works best combined with the hash chain
  from Project 1.
- A whole signed record can be copied and added again. The signature
  would still be valid. The id and time are signed, so duplicates
  could be detected, but I did not build that check.
- The list of public keys has to be trusted. If an attacker swaps in
  their own public key for Ravi, they can pass as Ravi.
- It runs on one machine, the keys are generated fresh each time, and
  the data is simulated.

Try it
Open `digital_signatures.ipynb` in Google Colab and run the cells in
order. The `cryptography` library is already installed in Colab.

Screenshots
![Valid records](green_table.png)
![One record tampered](red_table.png)
![Test results](tests_output.png)
