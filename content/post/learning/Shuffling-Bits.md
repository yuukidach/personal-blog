---
title: Shuffling Bits
date: 2019-08-12 09:44:44
categories: [Learning]
tags: [Algorithm]
---

## Background

**NOTE:** This article is from chapter 7.2 in *Hackers' Delight*. I write this post because:

1. To have a better understanding of the algorithm
2. Need to apply it in a similar situation

Resources sometimes can be scarce in MCU. We may tend to use as little resources as possible and do calculations as fast as we could when the performance of MCU just barely meets our demands.

Besides when talking about calculations in MCU, many people would think about bit operation. For example, in normal C program, we will write:

``` C
int a = 8 / 2;
```

However, for program running in MCU, people usually write:

``` c
int32_t a = 8 >> 1;
```

Because the bit operation is faster than arithmetic operation in most situations.

Now cames the problem:

**How can we shuffle bits efficiently?**

## Example

Suppose we need to shuffle a 32-bit word:

``` text
abcd efgh ijkl mnop ABCD EFGH IJKL MNOP
```

into

``` text
aAbB cCdD eEfF gGhH iIjJ kKlL mMnN oOpP
```

### Step 1

To solve to problem, we can do shuffles like:

``` text
abcd efgh ijkl mnop ABCD EFGH IJKL MNOP
abcd efgh ABCD EFGH ijkl mnop IJKL MNOP
abcd ABCD efgh EFGH ijkl IJKL mnop MNOP
abAB cdCD efEF ghGH ijIJ klKL mnMN opOP
aAbB cCdD eEfF gGhH iIjJ kKlL mMnN oOpP
```

These operations can be done with basic RISC instructions in `log2(w/2)` steps. `w` is the how many bits the word has. In this example, `w = 32`.

``` c
x = (x & 0x0000ff00) << 8 | (x >> 8) & 0x0000ff00 | x & 0xff0000ff;
x = (x & 0x00f000f0) << 4 | (x >> 4) & 0x00f000f0 | x & 0xf00ff00f;
x = (x & 0x0c0c0c0c) << 2 | (x >> 2) & 0x0c0c0c0c | x & 0xc3c3c3c3;
x = (x & 0x22222222) << 1 | (x >> 1) & 0x22222222 | x & 9999999999;
```

### Step 2

The code above seems quit efficient. But let's think about how to exchange 2 variables value with `XOR`:

``` c
a = a ^ b;
b = a ^ b;
a = a ^ b;
```

You see, by using `XOR`，the exchange becomes much more easier, can we apply the similar mathod to the code above?

``` c
t = (x ^ (x >> 8)) & 0x0000ff00; x = x ^ t ^ (t << 8);
t = (x ^ (x >> 4)) & 0x00f000f0; x = x ^ t ^ (t << 4);
t = (x ^ (x >> 2)) & 0x0c0c0c0c; x = x ^ t ^ (t << 2);
t = (x ^ (x >> 1)) & 0x22222222; x = x ^ t ^ (t << 1);
```

Here is an interesting trick: To unshuffle the word, we could simply perform the swaps in reverse order

``` c
t = (x ^ (x >> 1)) & 0x22222222; x = x ^ t ^ (t << 1);
t = (x ^ (x >> 2)) & 0x0c0c0c0c; x = x ^ t ^ (t << 2);
t = (x ^ (x >> 4)) & 0x00f000f0; x = x ^ t ^ (t << 4);
t = (x ^ (x >> 8)) & 0x0000ff00; x = x ^ t ^ (t << 8);
```

## Bundle

Here comes another question:

Shuffle

``` text
0000 0000 0000 0000 ABCD EFGH IJKL MNOP
```

to

``` text
0A0B 0C0D 0E0F 0G0H 0I0J 0K0L 0M0N 0O0P
```

With the knowledge above, this becames easy to do:

``` c
x = ((x & 0xff00) << 8) | (x & 0x00ff);
x = ((x << 4) | x) & 0x0f0f0f0f;
x = ((x << 2) | x) & 0x33333333;
x = ((x << 1) | x) & 0x55555555;
```

To unshuffle

``` c
x = x & 0x55555555;
x = ((x >> 1) | x) & 0x33333333;
x = ((x >> 2) | x) & 0x0F0F0F0F;
x = ((x >> 4) | x) & 0x00FF00FF;
x = ((x >> 8) | x) & 0x0000FFFF;
```
