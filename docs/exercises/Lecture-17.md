---
layout: default
title: Lecture 17
classdate: 2020-03-11
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. `cp -r ~steve/ex/intlist .`

## Task
1. Implement `intlist_initialize` by allocating sufficient capacity and
   setting the members appropriately.

   Check that it works by running `make check` and see if `test_initialize`
   fails or not.
2. Implement `intlist_append`. You'll need to finish the implementation of
   `grow_if_needed`.

   Check that it works by running `make check` and see if `test_append` fails
   or not.
3. Implement `intlist_insert` and check its test. You might want to look at
   the man page for `memmove(3)`.
4. Write a new test `test_remove` which tests `intlist_remove` by appending
   some `int`s to a list and then removing some.

   Since `intlist_remove` isn't implemented, `make check` should cause
   `test_remove` to fail.
5. Implement `intlist_remove`.
6. Write tests for and then implement `intlist_append_all` and
   `intlist_insert_all`. You might want to look at the man page for
   `memcpy(3)`.
7. Rewrite your implementations for `intlist_append`, `intlist_insert`, and
   `intlist_append_all` in terms of `intlist_insert_all`. Check that your
   tests still succeed.
