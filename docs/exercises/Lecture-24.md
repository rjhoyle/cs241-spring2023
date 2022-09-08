---
layout: default
title: Lecture 24
classdate: 2020-04-10
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Download [data.txt](data.txt) using `wget(1)` or `curl(1)`.

## Task
1. `data.txt` consists of a table of information regarding government offices
   including phone numbers of the administrators in charge of the office. Each
   line in the file consists of a number of tab-delimited fields. The first
   line of the file gives the name of each field.

   Write a single regular expression for use with `grep -E` to output all of
   the lines in `data.txt` containing phone numbers in the 202 and 314 area
   codes.

   If you use `$ grep -E --color regex data.txt` it will highlight the matches
   in the line so you can be sure it's matching the correct things. There
   should be 42 matching lines.

2. Write a `sed(1)` substitution command to print out the `Symbol`
   corresponding to each organization in the `Archives I` building.

   You're going to want to use the `-n` option to `sed(1)` to suppress
   printing lines by default and use the `p` flag to the substitution command
   to print lines on which a substitution was performed. In other words, you
   want to use `sed -E -n` and the substitution should be
   `s/regex/replacement/p`.

   Hint:
   ```
   $ sed -E -n '/\tArchives I\t/p' data.txt
   ```
   uses the `p` (print) command to print all lines matching `Archive I`
   surrounded by tabs. Replace the `p` with a suitable `s///p` to extract just the
   symbol from those lines, assuming it's not blank.

   There are 16 such symbols. The first three lines of output should be
   ```
   N
   NCON
   NHPRC
   ```

3. Write a `sed(1)` substitution command that matches lines with phone numbers
   in the 202 and 314 area codes _and_ has a nonblank person in charge. The
   substitution command should replace each matching line with
   ```
   [Symbol] In Charge: Phone
   ```
   where `Symbol`, `In Charge`, and `Phone` correspond to the fields in the
   line with those names.

   The first two lines of output should be
   ```
   [N] David S Ferriero: 202-357-5900
   [ND] Debra Steidel Wall: 202-357-5900
   ```
   and there should be 39 lines in total.

   Note that some of the lines with telephone numbers in the appropriate area
   codes do not have a person in charge and thus should _not_ be printed. The
   line with the Symbol `AFN-MR` is an example of one that should not be
   printed.

   This one is complicated, try building it up your regular expression piece
   by piece. The fields are separated by tabs and you can use `\t` to match a
   tab.
4. Repeat 2 and 3, but use `awk(1)` instead of `sed(1)`.
