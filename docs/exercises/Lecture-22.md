---
layout: default
title: Lecture 22
classdate: 2020-04-06
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. `$ cp -r ~steve/ex/mda .`

## Task
1. Implement the function `read_array` in `array.c`. It should read in a
   rectangular (2D) array of integers from the specified file into an array
   and return the array along with the number of rows and columns. See
   `array.h` and `array.c` for details.

   The first line of the file contains two integers: the number of rows and
   the number of columns. Following this are number-of-rows lines each of
   which contains number-of-columns integers. For example,
   ```
   5 3
   1 2 3
   4 5 6
   7 8 9
   10 11 12
   13 14 15
   ```
   represents a 2D array with 5 rows and 3 columns (this file is in
   `small.txt`)
   $$\begin{pmatrix}1&2&3\\4&5&6\\7&8&9\\10&11&12\\13&14&15\end{pmatrix}.$$
2. Implement the function `write_array` in `array.c`. It should first write
   out the number of rows and then columns on the line followed by the data of
   the array to the file.
3. Test that your code works by making `copyarray` (`$ make copyarray`) and
   then trying to copy the array from `small.txt` into `copy.txt`.
   ```
   $ ./copyarray small.txt copy.txt
   ```

   Check that the output matches what you expect.
4. Implement the missing functionality in `transpose.c`. It should take in an
   array from one file, compute the transpose, and write it out to the second
   file.

   The transpose of the small $5\times 3$ array above is the $3\times 5$ array
   $$\begin{pmatrix}1&4&7&10&13\\2&5&8&11&14\\3&6&9&12&15\end{pmatrix}.$$
5. Create a new program `matrixmult` (and add it to the `Makefile` by adding
   `matrixmult` to the `bins` variable) with a corresponding `matrixmult.c`
   This program should take three arguments. The first two should be the paths
   to files containing arrays and the third is the output file. Compute the
   [matrix
   multiplication](https://en.wikipedia.org/wiki/Matrix_multiplication) of the
   two input arrays and write the result to the output file.

   Note that if you have a $k\times m$ matrix and a $m\times n$ matrix, then
   the result of multiplication is a $k\times n$ matrix. Otherwise, the
   multiplication isn't defined.
