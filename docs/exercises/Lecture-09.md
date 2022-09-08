---
layout: default
title: Lecture 9
classdate: 2020-02-21
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/size.c` to your directory.

## Task
1. Open `size.c` in your editor and read it. Try to guess what it will print
   out. Compile the code and run it.
   ```
   $ clang -Wall -std=c11 -o size size.c
   $ ./size
   ```
   Did it print what you expected?
2. Modify `size.c` and add additional `printf` lines to print the sizes of
   `short`, `int`, `long`, `long long`, `float`, `double`, and `size_t`.
   Compile your program and run it.
3. Modify `size.c` one more time. This time, print the size of `bool`. Try to
   compile the code. Clang gives two errors, the second is nonsensical, but
   the first is instructive. Try to compile with `gcc` instead of `clang`.
   You'll get a very similar error.

   Look closely at the error messages, they tell you a lot. They start by
   giving the name of the file, the line of the error, and the column of the
   error: `size.c:line:column:`. This is followed by the error message and
   then the line of code with a marker indicating the place the compiler
   thinks you've made an error.

   (You can have Vim show you line numbers by adding `set number` to your
   `~/.vimrc` or by running `:set number` while Vim is running. You can
   have Emacs [display line
   numbers](https://www.emacswiki.org/emacs/LineNumbers) too.)

   This particular error is because C calls its Boolean type `_Bool`. This is
   terrible. Fortunately, there's a better way.
4. Add the line `#include <stdbool.h>` at the top of `size.c`. This makes
   `bool` an alias (technically a `typedef`) of `_Bool` and defines `true` as
   1 and `false` as 0. Compile and run it.

   (If you'd like, you can read `/usr/include/clang/6.0/include/stdbool.h` to
   see how that happens, but note we haven't actually talked about most of
   what's in there. Nevertheless, it should be fairly easy to follow what's
   going on.)

5. Run
   ```
   $ clang-format -i size.c
   ```
   to make sure your code is consistently formatted.
6. Create a new file, `fib.c`. Write a function `long long
   fib(int n)` that computes and returns the nth [Fibonacci
   number](https://en.wikipedia.org/wiki/Fibonacci_number#Sequence_properties).
   (Feel free to use either recursion or a loop. Make sure that any temporary
   variables you use have type `long long`.)

   Below it, create a main function that calls `fib` with the argument of your
   choice and prints out the result, e.g.,
   ```c
   int n = 19;
   printf("F_%d = %lld\n", n, fib(n));
   ```

   Format your code with `clang-format`, compile and run it. Make sure you get
   the right answer!
7. Read the man page for `atoi`. This function takes a string and returns an
   integer. Modify `fib.c` to iterate over all of the command line parameters
   (excluding `argv[0]` which is the name of the program itself), call
   `atoi(argv[idx])` for each 1 ≤ `idx` < `argc`, and print out the result.

   Pay attention to which header file you need to include in order to use
   `atoi` and make sure to `#include` it.

   Format, compile, and test.
   ```
   $ ./fib 0 19 12
   F_0 = 0
   F_19 = 4181
   F_12 = 144
   ```
8. Modify `fib.c` such that if any of the arguments are negative, it prints
   out an error message rather than the value of the Fibonacci number. If any
   of the arguments are negative, make `main` return 1 to indicate error.

   Recall that error messages should go to `stderr` rather than `stdout`. We can
   use `fprintf` to print to the `stderr` file.
   ```c
   fprintf(stderr, "%s: Error: negative number: %s\n", argv[0], argv[idx]);
   ```

   Example output.
   ```
   $ ./fib 2 -3 6
   F_2 = 1
   ./fib: Error: negative number: -3
   F_6 = 8
   $ echo $?
   1
   ```
