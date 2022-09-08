---
layout: default
title: Lecture 12
classdate: 2020-02-28
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/parse_int.c` into the directory.

## Task
1. Read through `parse_int.c` and try to understand what it is doing. Looking
   at the man page for `getchar(3)` and `isdigit(3)` is going to be helpful.
2. Compile `parse_int` and run it. It's expecting input from `stdin`, so
   you can either type into the terminal while it's running. It'll receive
   input one line at a time so be sure to hit return/enter and if you want to
   signal the end of the input, press control-d (on an otherwise blank line).
   You can also run `$ echo 12345 | ./parse_int`.
3. Edit `parse_int.c` and change `num_value` so that it returns `0` on input
   `'0'`, `1` on input `'1'`, and so on up to `9` on input '`9`'. You can
   assume the input is in ASCII (take a look at the `ascii(7)` man page for
   handy tables of values) and in particular, '0'--'9' are consecutive values.
   Thus `ch - '0'` is probably useful to you.
4. Modify the behavior of `parse_int.c` so that it reads a positive integer
   (in decimal) one character at a time (until a non-digit is read). Print out
   the value of the integer times 7.
   ```
   $ echo 0 | ./parse_int
   0 * 7 = 0
   $ echo 1 | ./parse_int
   1 * 7 = 7
   $ echo 12345 | ./parse_int
   12345 * 7 = 86415
   ```

   Recall, $539$ means
   $$ 5\cdot10^2 + 3\cdot10^1 + 9\cdot10^0 = (5\cdot10^1 + 3\cdot10^0)\cdot10 + 9.$$
   But notice that the number in the parentheses is just $53$. Therefore, if
   `val` is our current value, then after reading in another digit, we need to
   multiply `val` by 10 and add the value of the digit.
5. Modify the behavior of `parse_int.c` again so that instead of reading just
   one integer, it reads two integers, separated by a `+`, adds them together,
   and prints out the result.
   ```
   $ echo 32527+1256289 | ./parse_int
   32527 + 1256289 = 1288816
   ```
6. Modify `parse_int.c` one more time so that if the two numbers are separated
   by `+`, it adds them; by `-`, it subtracts the second from the first; by `*`,
   it multiples them; and by `/`, it divides the first by the second. What
   happens if you try to divide by 0? Handle that gracefully.
