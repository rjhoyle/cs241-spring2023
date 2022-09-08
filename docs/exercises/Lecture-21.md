---
layout: default
title: Lecture 21
classdate: 2020-04-03
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. `$ cp -r ~steve/ex/streams .`

## Task
1. Finish implementing `tobin.c` such that it reads integers (separated by
   spaces) from `stdin` using `scanf(3)` one at a time and then writes them to
   the file whose path is the first argument to `tobin`. (See `tobin.c` for
   more details.)

   We can use `xxd(1)` which prints out each byte in hex to examine our
   result.

   ```
   $ ./tobin output
   1 2 3 4 X
   $ xxd output
   00000000: 0100 0000 0200 0000 0300 0000 0400 0000  ................
   ```

   Notice that it wrote it out in little-endian. I.e., the integer
   `0x00000001` became the four bytes `01 00 00 00`.

   Try giving the input `1819043144 1396908143 170996786 X` to `tobin` and
   take a look at the resulting file.

2. Finish implementing `double.c` that reads the file whose path is the first
   argument to `double`. The goal is to read in all of the integers, from the
   file, double them, and then write them back to the file. (See `double.c`
   for more details.)

   ```
   $ ./double output
   $ xxd output
   00000000: 0100 0000 0200 0000 0300 0000 0400 0000  ................
   ```

3. Finish implementing `frombin.c` which reads integers one at a time from a
   file and prints them to `stdout`, one per line.

   ```
   $ ./frombin output
   2
   4
   6
   8
   ```

4. Modify your implementation of `frombin.c` to first get the file size, then
   allocate an array of integers long enough to hold the whole file, and then
   read everything in with a single call to `fread(3)`.
