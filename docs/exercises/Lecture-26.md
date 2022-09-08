---
layout: default
title: Lecture 26
classdate: 2020-04-15
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Copy `~steve/ex/checksum` to your home directory

## Task
1. Write a function in C to compute a 1-byte checksum of a file. Look in
   `checksum.c`. You'll need to modify the function to
   1. Open the file with
      `open(2)`;
   2. In a loop, read the file one byte at a time using `read(2)` into the
      variable `byte` and add that value to `cs`;
   3. Close the file descriptor using `close(2)`; and
   4. Return `256 - cs`.

   If step 1 fails, return `-1`. If step 2 fails, save `errno` into a local
   variable, call `close(2)` on the file descriptor, set `errno` back to the
   value it had before calling `close(2)`, and return -1. (If `close(2)`
   fails, there's nothing we can do anyway, so don't bother check its return
   value. But after class, you should read
   [this](https://stackoverflow.com/a/24479705) about a case where failing to
   check the return of `close(2)` can actually lead to data corruption!)

   Note that the return value of `read(2)` is a `ssize_t` which is like a
   `size_t` but is signed. So `read(2)` returns `-1` on error and `0` on
   `EOF`. 

   You should be able to run `checksum` on various files to get a simple
   1-byte checksum.
   ```
   $ ./checksum checksum checksum.c Makefile
   47  checksum
   f5  checksum.c
   f8  Makefile
   ```
   (You will almost certainly get different values for `checksum` and
   `checksum.c`, but the value for `Makefile` should be the same, unless you
   have modified it.)
2. In `checksum2.c`, you're going to write the same program except you're
   going to use `fopen(3)`, `fread(3)`, and `fclose(3)` instead of `open(2)`,
   `read(2)`, and `close(2)`.

   One thing to notice is the file descriptor is the first argument to
   `read(2)` whereas the file pointer (`FILE *`) is the last argument to
   `fread(3)`.
   
   In addition, `fread(3)` returns an unsigned `size_t` rather than a signed
   `ssize_t`. On failure, `fread(3)` returns `0`, just as it does for `EOF`.
   You should use `ferror(3)` to check if the `fread(3)` failed. If it did
   fail, save `errno`, call `fclose(3)`, restore `errno`, and return `-1`.

   ```
   $ ./checksum checksum checksum.c checksum2 checksum2.c Makefile
   47  checksum
   f5  checksum.c
   bb  checksum2
   a3  checksum2.c
   f8  Makefile
   ```

   Again, your code and binaries will probably be different from mine, but you
   should get the same results with either `checksum` or `checksum2`.
3. You probably noticed that `checksum` took several seconds where as
   `checksum2` finished almost immediately.

   Think about why that is and try to come up with an explanation. We'll
   figure out why this happened next class.
