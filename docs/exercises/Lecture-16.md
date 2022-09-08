---
layout: default
title: Lecture 16
classdate: 2020-03-09
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/arr.c` into the directory.

## Task
1. Look at the code for `arr.c`. It currently doesn't do much more than look
   at its command line parameters. Compile the program and run it with
   a positive number, a negative number, 0, and something that isn't a number
   but starts with a number like `35foo`.
2. Inside `main`, create a variable-length array, `seq`, of `int`s of size `n`.
   Initialize this array using a `for` loop such `seq[idx]` has the value
   `idx*idx` if `idx` is even and `-idx*idx` if `idx` is odd.
3. Write a function
   ```c
   int sum(size_t len, int arr[len]);
   ```
   that computes and returns the sum of the elements of `arr`.

   Inside `main`, print out the result of `sum(n, seq)`.

   Compile and run the code with a few small values of the command line
   argument `NUM`.
   Make sure you compile with
   ```
   $ clang -Wall -std=c11 -o arr arr.c
   ```
   and fix any warnings that appear.
4. Change the code to print out `sum(n+10, seq)` instead. Before you compile
   and run this, make a prediction as to what will happen? Now compile and
   run. Did it match your expectation?
5. In task 4, the code was changed so that the `sum` function would read
   beyond the end of the array. The compiler didn't complain about this. At
   runtime, you got strange results, but there was no error or warning.

   **This is bad. Nearly every C program in existence has a similar problem.**

   Compile your program with
   ```
   $ clang -Wall -std=c11 -fsanitize=address,undefined -o arr arr.c
   ```
   and run it again. This time it should crash with a colorful error message.
   This additional option tells the compiler (and the linker) to insert
   additional code in the program to detect these sorts of undefined behavior.
6. You can get a nice backtrace (a list of the functions that were called
   leading to the error) with a couple of more steps.

   Compile with
   ```
   $ clang -Wall -std=c11 -fsanitize=address,undefined -g -o arr arr.c
   ```
   where the additional `-g` option adds debugging information to the binary.
   Run the program with a particular environment variable set.
   ```
   $ ASAN_SYMBOLIZER_PATH=/usr/lib/llvm-6.0/bin/llvm-symbolizer ./arr 8
   ```

   When I do that, I get the output
   ```
   ==7410==ERROR: AddressSanitizer: dynamic-stack-buffer-overflow on address 0x7ffd4d3aee80 at pc 0x00000051222d bp 0x7ffd4d3aedd0 sp 0x7ffd4d3aedc8
READ of size 4 at 0x7ffd4d3aee80 thread T0
    #0 0x51222c in sum /usr/users/noquota/faculty/steve/inclass/arr.c:8:14
    #1 0x5128f8 in main /usr/users/noquota/faculty/steve/inclass/arr.c:25:18
    #2 0x7f7ab6782b96 in __libc_start_main /build/glibc-OTsEL5/glibc-2.27/csu/../csu/libc-start.c:310
    #3 0x419d59 in _start (/net/storage/zfs/faculty/steve/inclass/arr+0x419d59)
   ```
   where this is telling me that the error happened in `arr.c` in the `sum`
   function on line 8, column 14. It also shows where `sum` was called from,
   namely `main` at line 25, column 18.
7. Write a program to concatenate all command line arguments into a single
   string separated by spaces. I.e., you want something like
   ```
   int main(int argc, char *argv[argc]) {
     char msg[] = "You ran the following command:";
     char command_line[50];
     /* copy argv[0] to command_line */
     for (int i = 1; i < argc; ++i) {
       /* append a space to command_line */
       /* append argv[i] to command_line */
     }
     puts(msg);
     puts(command_line);
     return 0;
   }
   ```
   Use `strcpy(3)` and `strcat(3)` to perform the copies.

   Run the program with slightly more than 50 characters of command line
   arguments. What happens? Recompile with `-fsanitize=address,undefined` and
   run the program again.
8. Modify the program to use `strlcpy(3)` and `strlcat(3)` instead. Recompile
   with `-fsanitize=address,undefined` and see what happens.
