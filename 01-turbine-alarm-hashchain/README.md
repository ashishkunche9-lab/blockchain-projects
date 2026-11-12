Why I built this
In a power plant, alarm logs matter a lot. If vibration goes high or
bearing temperature crosses its limit, that record is what people look
at later to understand what happened. But if someone quietly edits or
deletes an entry, nobody can trust the log anymore.

I wanted to see if I could make a log where any change gets noticed
immediately. So I built a small hash chain in Python and tested it
against different attacks.

How it works
- Every alarm is saved as a block with its time, alarm name, value and
  the hash of the block before it.
- Each block's hash is made with HMAC-SHA256 and a secret key, so it
  depends on the block's own data and on the previous block. Change one
  block and every block after it stops matching.
- A verify function recomputes everything and tells you exactly which
  block was changed or where the link broke.
- I also keep the last hash separately as an "anchor". This is what
  catches someone who deletes the final block.
- The sensor data is simulated: 20 readings each for vibration, bearing
  temperature and lube oil pressure, with alarms raised when a limit is
  crossed.

What I tested
I tried 7 cases: no change, editing a value, editing an alarm name,
deleting a block, swapping two blocks, deleting the last block, and a
re-hash attack where the attacker rebuilds all the hashes without
knowing the key. All the attacks were caught.

What it can't do
- Deleting only the last block still leaves a valid chain. Only the
  anchor catches it.
- If the secret key leaks, the chain can be forged. In a real plant the
  key would sit in a vault or hardware module, not in the code.
- It runs on one machine, so it is not a real distributed blockchain.
- The sensor values are simulated, not from real equipment.

Try it
Open `alarm_log.ipynb` in Google Colab and run the cells in order.

Screenshots
![Valid chain](green_table.png)
![Tampered chain](red_table.png)
![7 tests](tests_output.png)
![Sensor graph](sensor_graph.png
