---
layout: default
title: Lecture 15
classdate: 2020-03-06
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/ptr.c` into the directory.

## Task
1. Implement
   ```c
   size_t simple_strlen(char const *str);
   ```
   to return the length of the string argument `str`, not counting the final
   `0` byte.

   You will want to loop over each character in the string and check if it is
   `0` or not (note that `0` and `'0'` are very different). You can do this by
   checking if `str[length]` is `0` and if so, return `length`. If it isn't
   `0`, increment `length` by one.

   Alternatively, you can use pointer arithmetic and loop while `*str` is not
   `0`. In the body of the loop, increment both `length` and `str`.
2. The environment variables are available in a variable in `unistd.h`.
   ```c
   extern char **environ;
   ```
   (Remember that `extern` just means it's defined somewhere else.)

   `environ` is a pointer to an array of strings and it ends with a `NULL`
   pointer. That is, `environ[0]` is the 0th environment variable,
   `environ[1]` is the 1st, and so on. If there are _n_ environment variables,
   then `environ[n]` is `NULL`. The number of environment variables isn't
   stored for you.

   Print out how many environment variables.

   Your final output should be something like this (but you may have a
   different number of environment variables).
   ```
   $ ./ptr foo 'bar baz'
   Arg 0 [./ptr] has length 5
   Arg 1 [foo] has length 3
   Arg 2 [bar baz] has length 7
   There are 53 environment variables
   ```
