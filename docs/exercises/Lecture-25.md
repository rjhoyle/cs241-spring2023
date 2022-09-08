---
layout: default
title: Lecture 25
classdate: 2020-04-13
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. `$ cp -r ~steve/ex/funptr .`

## Task
1. Look at `fold.c`. The `foldl()` function at the top of the file is an
   instance of a
   [fold](https://en.wikipedia.org/wiki/Fold_%28higher-order_function%29). It
   takes an array of integers (and its size), an initial value, and a pointer
   to a function of two `int`s that returns an `int`. It iterates through the
   array, applying the function to `initial` and the next element in the list,
   updating `initial` each time.

   Write a function `add` such that `foldl(num, arr, 0, add)` computes the sum
   of the `num` integers in `arr`. Have `main` print out the result of that.
2. Write a function `mult` that you can use with `foldl` to produce the
   project of all the integers in the array. Print that result out too.
3. Do the same thing but with `min` and `max` to compute the minimum and
   maximum. You may want to use `INT_MIN` and `INT_MAX`.
4. Write a function `print` that you can use with `foldl` to print out (via
   `printf(3)`) each of the elements in the array.

   When you run your code, you should get something like this.
   ```
   $ ./fold
   array: -8 2 5 19 20 -72
   sum: -34
   product: 2188800
   min: -72
   max: 20
   ```
5. Look at `courses.c`. Add your other courses to the array. Implement the
   appropriate comparison functions to be used with `qsort(3)` to sort the
   list of courses by (1) start time, (2) department and number, and (3)
   department and number but have CSCI courses first. Print out the course
   list after sorting by each of these using the provided `print_courses()`.
