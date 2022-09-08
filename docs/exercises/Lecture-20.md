---
layout: default
title: Lecture 20
classdate: 2020-04-01
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. `$ cp -r ~steve/ex/linenumbers .`

## Task
1. Modify `linenumbers.c` such that it reads a line from `stdin` using
   `fgets(3)` and writes to `stdout` using `fprintf(3)` (you'll see why we're
   using `fprintf(3)` rather than `printf(3)` shortly) except that each line
   should be prefixed with its line number as in this example.
   ```
   $ ./linenumbers < Makefile
   1: CC := clang
   2: CFLAGS := -Wall -std=c11 -g -fsanitize=address,undefined
   3: LDFLAGS := -fsanitize=address,undefined
   4:
   5: .PHONY: all clean
   6:
   7: all: linenumbers
   8: clean:
   9: 	$(RM) linenumbers
   ```
   You may assume a line is no longer than 1024 characters for now.
2. Modify `linenumbers.c` such that if at least one argument is given (so
   `argc > 1`), the first argument is treated as the file to read rather than
   `stdin`. I.e., open the file using `fopen(3)`. Make sure to handle the case
   where `fopen(3)` returns `NULL` by printing out an appropriate message
   using `perror(3)` and exiting with `EXIT_FAILURE`.

   If no arguments are given, then behave as before, namely reading from
   `stdin`. You'll probably want code that looks like
   ```c
   FILE *input = stdin;
   if (argc > 1) {
     input = fopen(/* ... */);
     if (!input) {
       // Handle the error.
     }
   }
   ```

   Here's an example of the error handling.
   ```
   $ ./linenumbers "does not exist"
   does not exist: No such file or directory
   $ echo $?
   1
   ```

   Make sure you `fclose(3)` the input when you're done with it.
3. Modify `linenumbers.c` such that if no arguments are given, it behaves as
   in part 1. If one argument is given, it behaves as in part 2. If two
   arguments are given, the output is written to the file named by the second
   argument rather than to `stdout`. And if more than two arguments are given,
   it prints out a usage message and exits with `EXIT_FAILURE`.

   Be careful that you don't accidentally overwrite a file you care about.

   ```
   $ ./linenumbers one two three
   Usage: ./linenumbers [INPUT [OUTPUT]]
   ```

   Make sure you `fclose(3)` the output when you're done with it.
4. It's traditional that if `-` is passed as a file name for input, then the
   input should come from `stdin` and if `-` is passed as a file name for
   output, then the output should go to `stdout`. Implement this
   functionality, using `strcmp(3)` from `string.h`.

   ```
   $ echo -e "Hello world\nHow are you?" | ./linenumbers - output.txt
   $ cat output.txt
   1: Hello world
   2: How are you?
   ```
5. Modify `linenumbers.c` to remove the restriction on line lengths. You can
   use `strchr(3)` to search for a newline. If it doesn't exist, output what
   you have so far, and then keep reading with `fgets(3)` until you reach the
   end of the line and output it using `fputs(3)`.
6. Modify `linenumbers.c` one final time to make sure that you handle error
   return values (which you really should have been doing all along) from each
   of the standard library functions: `fopen(3)` and `fgets(3)` return `NULL`
   on error; `fputs(3)` and `fclose(3) return `EOF` on error; and `fprintf(3)`
   returns a negative number on error.
