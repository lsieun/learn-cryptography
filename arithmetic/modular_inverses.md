# Modular inverses

<!-- TOC -->

- [1. What is an inverse?](#1-what-is-an-inverse)
- [2. What is a modular inverse?](#2-what-is-a-modular-inverse)
- [3. How to find a modular inverse](#3-how-to-find-a-modular-inverse)

<!-- /TOC -->

## 1. What is an inverse?

Recall that a number multiplied by its inverse equals `1`. From basic arithmetic we know that:

- The inverse of a number `A` is `1/A` since `A * 1/A = 1` (e.g. the inverse of `5` is `1/5`)
- All real numbers other than `0` have an inverse
- Multiplying a number by the inverse of `A` is equivalent to dividing by A (e.g. `10/5` is the same as `10 * 1/5`)

## 2. What is a modular inverse?

In **modular arithmetic** we do not have **a division operation**. However, we do have **modular inverses**.

- The **modular inverse** of `A (mod C)` is `A^-1`
- `(A * A^-1) ≡ 1 (mod C)` or equivalently `(A * A^-1) mod C = 1`
- Only the numbers coprime to `C` (numbers that share no prime factors with `C`) have a modular inverse (mod `C`)

## 3. How to find a modular inverse

A naive method of finding **a modular inverse** for `A (mod C)` is:

- step 1. Calculate `A * B mod C` for `B` values `0` through `C-1`
- step 2. The **modular inverse** of `A mod C` is the `B` value that makes `A * B mod C = 1`

Note that the term `B` mod `C` can only have an integer value `0` through `C-1`, so testing larger values for `B` is redundant.
