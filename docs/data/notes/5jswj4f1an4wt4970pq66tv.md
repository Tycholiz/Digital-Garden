
In actuality, RNGs implemented using computers are Pseudo Random Number Generators, since they are deterministic (ie. the output is known ahead of time)
- this sequence is completely determined by an initial value (the **seed**)
    - Given the same seed, the algorithm will produce the same sequence of numbers every time.
    - If the same random seed is deliberately shared, it becomes a secret key, so two or more systems using matching pseudorandom number algorithms and matching seeds can generate matching sequences of non-repeating numbers which can be used to synchronize remote systems, such as GPS satellites and receivers.

### Analogy: Pi
Imagine we created a simple PRNG algorithm that just took each decimal digit of Pi (3.14159...). So the first time, we got 1, then 4, and so on. In this example, we used a seed of `1`, meaning we start at the first decimal place. To introduce some element of randomness, we might then use a seed of `2` on a subsequent execution of the PRNG, giving us a sequence of `4, 1, 5, 9...`
