# HashAttack

Brute-force preimage and collision attacks against truncated SHA-1 digests, measured across a range of output bit lengths in Java.

## What this demonstrates

The cost of breaking a hash depends on how many output bits you keep. Truncating SHA-1 to a small number of bits makes both attacks tractable, which lets you watch the birthday bound in action: a preimage attack scales as 2^n, a collision attack as 2^(n/2). This runs both attacks against SHA-1 truncated to a chosen bit length, counting how many hash evaluations each one needs, and repeats 50 times so the counts can be averaged against the theoretical expectation. The included PDF is the write-up analyzing the results.

## Concepts demonstrated

- Preimage attack: search random inputs until one hashes to a fixed target digest (expected ~2^n work)
- Collision attack: hash a growing sequence until two inputs share a digest (expected ~2^(n/2) work by the birthday bound)
- Truncating a SHA-1 digest to an arbitrary bit length, including sub-byte lengths via a computed bit mask
- Bit masking of the final partial byte so comparisons happen at exactly `numBits` resolution
- Empirical measurement: 50 trials per run, counting hash evaluations to compare against the theoretical bound

## What's implemented

- `HashAttack.java` implements `preImageAttack` and `collisionAttack`, the truncation and bit-mask logic (`getBitMask`), random string generation, and SHA-1 hashing via `MessageDigest`.
- `Main.java` parses arguments and runs the chosen attack 50 times.

## Stack

- Java (`java.security.MessageDigest` for SHA-1)

## Usage

Compile and run from `HashAttack/src/`:

```
javac Main.java
java Main <numBits> <-p | -c> [-v]
```

- `numBits` is the truncated digest length in bits
- `-p` runs the preimage attack, `-c` runs the collision attack
- `-v` prints per-iteration target/attack comparisons

Each run repeats the attack 50 times and prints the number of iterations each took. See `Project 2 -- Hash Attack.pdf` for the full analysis.
