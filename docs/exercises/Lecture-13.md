---
layout: default
title: Lecture 13
classdate: 2020-03-02
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.

## Task
1. Implement a function in C that returns the value of the most significant
   bit of an `unsigned int` using `CHAR_BIT` (defined in `limits.h`) and
   `sizeof(unsigned int)` to figure out how many bits are in an `unsigned
   int`.
   ```c
   unsigned int msb(unsigned int x) {
     /* ... */
   }
   ```

   I recommend creating a constant `NUM_BITS` defined in terms of `CHAR_BIT`
   and `sizeof(unsigned int)` that is the number of bits in an `unsigned int`.
   ```c
   #define NUM_BITS (/* ... */)
   ```
2. Test your implementation by printing out the result of `msb(0)`, `msb(1)`,
   `msb(-1)`. [Fun fact: Because clyde, like all modern machines, uses two's
   complement integers, converting a signed integer like `-1` to an unsigned
   integer does not change the bit pattern, instead negative integers become
   large positive integers.]
3. Implement a function that extracts the least significant `n` bits of an
   `unsigned int x`.
   ```c
   unsigned int extract_lsbs(unsigned int x, unsigned int n) {
     assert(n < NUM_BITS);
     /* ... */
   }
   ```

   `assert(3)` is defined in `assert.h`.
4. Test `extract_lsbs` by printing out the result of extracting a few
   different numbers of bits (printing hex using `printf`'s `%x` specifier is
   by far the easiest way to see this). E.g., `extract_lsbs(0x378, 5)` should
   return `0x18`.
5. Implement a function that returns a bit field---a contiguous sequence of
   bits---from `x` that starts at `start` and is `n` bits long.
   ```c
   unsigned int extract_field(unsigned int x,
                              unsigned int start,
                              unsigned int n) {
     assert(start < NUM_BITS);
     assert(n < NUM_BITS - start);
     /* ... */
   }
   ```
6. Test `extract_field`. E.g., `extract_field(0x11223344, 17, 9)` should
   return `0x91`.
7. That second assert in task 5 is unusual looking, but correct. The more
   natural looking assertion, `assert(n + start < NUM_BITS)` is _incorrect_.

   Find unsigned integers `start` and `n` such that calling the following
   function with them causes the program to abort, but only on the marked
   line.
   ```c
   void crash_me(unsigned int start, unsigned int n) {
     assert(start < NUM_BITS);
     assert(n + start < NUM_BITS);
     assert(n < NUM_BITS - start); // Abort on this line.
   }
   ```

   When it crashes, it should look like this (although file names and line
   numbers may differ).
   ```
   a.out: bit_fields.c:27: void crash_me(unsigned int, unsigned int): Assertion `n < NUM_BITS - start' failed.
   Aborted (core dumped)
   ```
