---
layout: default
title: Lecture 29
classdate: 2020-04-22
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Copy `~steve/ex/staticlib` to your home directory.

## Task
1. First, look through all of the `.c` files.
2. Compile each of the `.c` files to `.o` file. You can either do this by hand
   ```
   $ clang -c -o a.o a.c
   ```
   or you can use `make(1)`'s built-in rule.
   ```
   $ make CC=clang a.o b.o foo.o bar.o
   ```
3. Use `nm(1)` to print out the symbols for each of the `.o` files.
   ```
   $ nm *.o
   ```
   This shows you the symbol name and, if it's defined, the value. Undefined
   symbols show up as `U`. Defined global function symbols show up as `T` (for
   text). There are other possibilities listed in the man page for `nm(1)`.
4. Create an archive `libex.a` containing `a.o` and `b.o` using `ar(1)`.
   Either check the man page or look at the slides.

   `nm(1)` works with archives too. Try running it on `libex.a`. It should
   list the symbols for both `a.o` and `b.o`.

   You might notice that one symbol in `a.o` and one in `b.o` have value 0.
   That's because the archive just contains the two object files (and a symbol
   table assuming you used the `s` option to `ar(1)` _or_ ran `$ ranlib
   libex.a`). Each object file is independent and the 0 is an offset into the
   `.text` segment of the corresponding file.
   
5. You can use `$ readelf -SW` to print out the list of sections in object
   files (including those in archives) (or any other sort of ELF file). 

   You can also get more information about the symbols by using `$ readelf
   -s`.
6. Create an archive `libbar.a` containing `bar.o`.
7. Now run
   ```
   $ clang -o prog1 foo.o libbar.a libex.a
   ```

   Look at the included symbol using
   ```
   $ nm prog1|grep ' T '
   ```
   (`nm(1)` has `-g` and `--defined-only` options that together are close to
   what you get from the `grep`, but they also include other data).

   Can you tell which object files got included? If not, try running `$
   ./prog1`.
8. Run
   ```
   $ clang -o prog2 foo.o libex.a libbar.a
   ```
   and look at its defined symbols. Can you tell which object files got
   included? Are they they same ones from step 7?

   Run `$ ./prog2` and compare the output with that of `$ ./prog1`.
9. Create a `Makefile` with targets `all`, `clean`, `prog1`, `prog2`,
   `libex.a`, and `libbar.a` that build `prog1` and `prog2` as specified
   above.

   Make sure `all` and `clean` are `.PHONY` targets and that all of the object
   files depend on `ex.h`.

   Don't forget to `$(RM) $@` before building an archive.
