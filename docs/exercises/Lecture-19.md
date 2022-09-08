---
layout: default
title: Lecture 19
classdate: 2020-03-30
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. `$ cp -r ~steve/ex/linkedlist .`

## Task
1. Look at `list.h` to get a sense of the functions and data types. Run
   `$ make check`.
2. Implement `list_prepend` and check that `test_prepend` passes.
3. Implement `list_insert` and check that `test_insert` passes.
4. Implement `list_remove_first` and check that `test_remove_first` passes.
5. Implement `list_remove_last` and check that `test_remove_last` passes.
6. Write a new test program, `test_remove`, to test `list_remove` (you'll
   probably want to based it on the `test_insert` program).
7. Implement `list_remove` and check that your test passes.
8. Write a new test program, `test_reverse`, to test `list_reverse`.
9. Implement `list_reverse` and check that your test passes.
