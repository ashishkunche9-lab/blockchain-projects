Proof of Work: Making Tampering Expensive

Why I built this
My first two projects could tell when data was changed. But an
attacker who changes a block can simply fix every hash after it, and
the chain looks valid again.

Real blockchains handle this with proof of work: creating a block has
to cost real effort. So I built a small miner in Python to see how it
makes rewriting history expensive.

How it works
- To add a block, you have to find a number called a nonce, so that
  the block's hash starts with a certain number of zeros.
- There is no shortcut. You just keep trying nonces one by one until
  one works. Checking the answer takes a single hash.
- The number of leading zeros is the difficulty. Every extra zero
  makes the search about 16 times harder, because each hex digit has
  16 possible values.
- Every block stores the hash of the previous block, so editing an old
  block means re-mining that block and every block after it.
- The alarm data is simulated turbine alarms (vibration, bearing
  temperature, lube oil pressure).

What I found
**Difficulty vs effort.** For one alarm, the miner needed:
- difficulty 1: 5 attempts
- difficulty 2: 123 attempts
- difficulty 3: 5,036 attempts
- difficulty 4: 205,803 attempts (0.29 s)
- difficulty 5: 808,547 attempts (1.45 s)

The graph averages 8 different alarms per level, so the curve is
smoother. It climbs from about 10 attempts at difficulty 1 to over a
million at difficulty 5. A single run is lucky or unlucky, so the
jump is not exactly 16x every time, but on average it is close.

**Attack cost.** I mined a chain of 6 blocks at difficulty 5, which
took 14.9 seconds. Then I played the attacker:
- Editing block 1 without re-mining made the chain invalid.
- Re-mining block 1 and every block after it made the chain valid
  again, but it cost 11.9 seconds of work for one small edit.

With only 6 blocks the cost is small. The longer the chain, the more
blocks the attacker has to redo, and the cost keeps growing.

What it can't do
- Real blockchain security comes from many miners. The honest network
  keeps adding blocks while the attacker is still redoing old ones.
  My demo has one machine, so it only shows the cost, not the race.
- The difficulty is fixed. Real networks adjust it automatically to
  keep block times steady.
- Proof of work wastes a lot of energy. That is its biggest
  criticism, and why other approaches exist.
- A fast computer or GPU would make this attack much cheaper than my
  Colab numbers suggest.
- The data is simulated.

Try it
Open `proof_of_work.ipynb` in Google Colab and run the cells in order.
Difficulty 5 takes a few seconds per block. If it feels slow, change
`DIFF = 5` to `4` in the last cell.

## Screenshots
![Mining table](mining_table.png)
![Difficulty graph](difficulty_graph.png)
![Attack cost](attack_cost.png)
