---
layout: default
title: Lecture 33
classdate: 2020-05-04
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a new directory.

## Task
1. Write a short program containing a function with the prototype
   ```c
   double varsum(int num, ...)
   ```
   where the `num` parameter controls how many additional `double` parameters
   there are. `varsum` should add up all of the double parameters and return
   the result. If `num <= 0`, return `0.0`.
2. The `printf` family of functions includes (among others) `printf(3)`,
   `fprintf(3)`, `vprintf(3)`, and `vfprintf(3)`. The first three can easily
   be written in terms of the fourth.

   Write four similar functions
   ```c
   void msg(char const *type, char const *fmt, ...);
   void vmsg(char const *type, char const *fmt, va_list ap);
   void fmsg(FILE *fp, char const *type, char const *fmt, ...);
   void vfmsg(FILE *fp, char const *type, char const *fmt, va_list ap);
   ```
   where the intended behavior is
   ```c
   msg("Info", "0x%x %d", 32, 5);
   ```
   prints `[Info] 0x20 5`.

   Implement the first three in terms of the fourth. Feel free to use
   `fprintf(3)`, `vfprintf(3)`, or other standard I/O functions to implement
   `vfmsg`.

   Write a short program that tests them.
3. The `execl(3)` function takes a path and a variable number of arguments.
   The end of the argument list is denoted by an explicit `(char *)0`. 

   Using the same strategy of marking the end of the list with `(char *)0`,
   write a function
   ```c
   char *join(char const *sep, ...)
   ```
   That allocates and returns a string consisting of the variable number of
   strings joined together but separated the string `sep`. For example,
   ```c
   join(" ", "This", "is", "a", "sentence.", (char *)0);`
   ```
   should return the string `"This is a sentence."`;
   ```c
   join("foo", (char *)0);
   ```
   should return the empty string `""`; and
   ```c
   join("+-+" "a", "b", "c", (char *)0);
   ```
   should return the string `"a+-+b+-+c"`.

   In all cases, the string should be allocated via `malloc(3)` or
   `realloc(3)`. Make sure you don't leak memory.

   Write a short program to test your functions. The strings returned from
   `join` should ultimately be passed to `free(3)`.
