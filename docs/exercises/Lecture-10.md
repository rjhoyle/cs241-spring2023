---
layout: default
title: Lecture 10
classdate: 2020-02-24
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/rand.h` and `~steve/ex/rand.c` into the directory.

## Task
1. Take a look at `rand.h` and look at the function declared there. What
   arguments does it take? What does it return?
2. Write a file `printfloat.c`. Include `rand.h` and `stdio.h`. Your `main`
   function should call `random_value()` one time, storing the result in a
   variable of type `double`. Print that value out three times using `printf`
   with the `%e`, `%f`, and `%g` format specifiers.

   Compile the program using
   ```
   $ clang -std=c11 -Wall -o printfloat printfloat.c rand.c
   ```
   and then run it a few times. Do the printed values match what you expected?
   If not, check out the man page for `printf(3)` (you'll need to run `$ man 3
   printf` because `printf(1)` is the manual page for the shell command
   `printf` and we want the library function).
3. Modify `printfloat.c` to change the precision used to print the floats.
   E.g., try `%.1e`, `%.2e`, `%.3e`. Recompile and run the program a few times
   each.
4. Each time we recompile, we're forcing the compiler to recompile `rand.c`
   even though it hasn't changed. This isn't a big deal with such a small
   program, but imagine a program with 100 files. This could take a long time
   for each small change.

   Compile each of `printfloat.c` and `rand.c` separately and then link them
   together.
   ```
   $ clang -std=c11 -Wall -c -o printfloat.o printfloat.c
   $ clang -std=c11 -Wall -c -o rand.o rand.c
   $ clang -o printfloat printfloat.o rand.o
   ```
5. Now modify `printfloat.c` in some way (any way you want), recompile just
   `printfloat.o` and then do the final linking step.
   ```
   $ clang -std=c11 -Wall -c -o printfloat.o printfloat.c
   $ clang -o printfloat printfloat.o rand.o
   ```
6. What do you think the average distance between two uniformly random numbers
   in the range [0, 1] is? There are two ways we can figure this out. The hard
   way: we could compute the integral
   $$ \int_0^1\int_0^1|x-y|\,dx\,dy. $$

   But this isn't calculus class, so let's do it the easy way: Monte--Carlo
   simulation. That's just a fancy way of saying, let's run a bunch of random
   trials and see what we get on average.

   Write a new file `mc.c`. Include `rand.h` and `stdio.h` as before. Use
   `#define` to define a constant `NUM_TRIALS`. E.g., something like
   ```c
   #define NUM_TRIALS 10
   ```

   In the `main` function, define a variable `double total = 0.0;`. Next, loop
   `NUM_TRIALS` times (a `for` loop works well here) and use `random_value()`
   to get two random numbers `x` and `y`. Use the `fabs` function (read its
   man page to see what header you need and how to call it) to compute the
   absolute value of `x-y`. Add the result to `total`.

   After the loop, print out the average value of $|x-y|$ by dividing
   `total` by `NUM_TRIALS`.

   Compile your program by compiling `mc.c` to `mc.o` and then linking `mc.o`
   and `rand.o` as a program called `mc`. Run `./mc` a few times.
7. Increase the value of `NUM_TRIALS` and recompile and run. Try the values
   100, 1000, and 10000.

   Does the answer match what you'd expect? (I find it kind of surprising!)
   Let's double check our results by asking a _much_ more complicated computer
   program than the one we just wrote to work a lot harder and solve the
   double integral above. Check out the result on
   [Wolfram|Alpha](https://www.wolframalpha.com/input/?i=int+int+%7Cx+-+y%7C+dx+dy+x%3D0..1+y%3D0..1).
