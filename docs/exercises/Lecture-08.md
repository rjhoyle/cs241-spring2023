---
layout: default
title: Lecture 8
classdate: 2020-02-19
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/hello.c` to your directory.

## Task
1. Open `hello.c` in your editor and read it. Try to guess what it will print
   out. Compile the code and run it by entering the following commands in the
   terminal.
   ```
   $ clang -Wall -std=c11 -o hello hello.c
   $ ./hello
   ```
   Did it print what you expected?
2. Run the `hello` program again, this time passing it some names as
   arguments (try to guess what will happen before you run it). You don't need
   to recompile the program, just run it again.
   ```
   $ ./hello Adam Bob Cynthia
   ```
3. Modify `hello.c` such that the "Hello" messages are printed in the reverse order.

   Since you have modified `hello.c`, you need to recompile the program. So
   once again, run
   ```
   $ clang -Wall -std=c11 -o hello hello.c
   $ ./hello Adam Bob Cynthia
   ```
   If you get an error or warning message, read it closely and then try to fix
   the error/warning.

   If your modified program printed out the line
   ```
   Hello (null)!
   ```
   then you have likely tried to print `argv[argc]` which has the special
   value `NULL`. This is an off-by-one error. You should fix this.
4. Lastly, we can use the
   ```c
   char *getenv(char const *environment_variable);
   ```
   function to get the value of environment variables (as a string). In
   particular, `getenv("USER")` returns the user name of the user running the
   program.

   In order to use `getenv`, the function needs to be declared before we can
   use it in `main.c`. Fortunately, `getenv` is part of the C standard library
   and is declared in the `stdlib.h` header.

   Modify `hello.c` as follows. First, add the line
   ```c
   #include <stdlib.h>
   ```
   to the top of the file.

   Next, modify the first `printf()` line so that rather than printing `Hello
   world!`, it prints `Hello ` followed by the user's user name. Look closely
   at the other `printf()` line for inspiration. Rather than printing out
   `argv[idx]`, you'll want to print out `getenv("USER")`.

   Compile and run the program. (After editing a file, it's easiest to press
   up in the terminal until you get to the `clang` line and then you can just
   press enter rather than typing out the whole `clang -Wall -std=c11 ...`
   line each time.)
