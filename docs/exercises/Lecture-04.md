---
layout: default
title: Lecture 4
classdate: 2020-09-10
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log on to clyde: `ssh user@clyde.cs.oberlin.edu`.
2. Create a directory with whatever name you like and `cd` into it.

## Task
1. The command `$ ls /bin /usr/bin` will print out a list of all the files
   in `/bin` and `/usr/bin`. Each of these is a program you can run, but there
   are likely too many to display in your terminal. So run the command again,
   but redirect the output to a file called `binaries`.
2. Run `$ wc -l binaries` to see how many lines are in the file.
3. Open the file with your favorite command line editor (e.g., `nvim`, `vim`,
   `emacs`, `nano`) and take a look at the output. Is the number you printed
   out in step 2 the actual number of binaries? Hint: Look closely at the
   first line.
4. One thing to notice is the output from `ls` is different when directed to a
   file than it is when it goes to the terminal. It turns out that programs
   can detect if their `stdout` is connected to a terminal or not. Some programs
   use this ability to change their output, some do not. Run `$ man isatty` and
   take a look at the manual page for the `isatty()` function. This is a C
   function and we'll get to programming in C soon enough. For now, it's
   enough to know that `stdout` is file descriptor `1` so the function call
   `isatty(1)` tells the program if `stdout` is a terminal or not.
5. Let's figure out how many binaries start with the letter `z` by using
   `grep`. Recall `grep` reads files (or `stdin`) and prints lines matching a
   pattern. Run `$ grep z binaries`. Notice that it prints out all lines
   containing a `z`. That's not what we want, but we're close.
6. Grep interprets the `^` character to mean "match the beginning of the line"
   so `$ grep ^z binaries` should do what we want. Run that and look at the
   output. 
7. Let's figure out how many of them there are. Rerun that `grep` command and
   redirect `stdout` to a file `zfiles`. Run the appropriate `wc` command to
   figure out how many there are.

   To recap what we've done so far, we created a file containing a list of all
   of the binaries in `/bin` and `/usr/bin`. We used `grep` to extract just
   those that start with `z` and stored that in a file. Finally, we ran `wc`
   on that to figure out how many there are. Pretty simple but we had to
   create two files for it that we don't really care about if our goal was
   just to figure out that final number.
8. We can skip the creation of the first file by _piping_ the output of `ls`
   to `grep` and storing the result to a file. Explicitly,

   ``` sh
   $ ls /bin /usr/bin | grep '^z' >zfiles1
   ```

   Notice that we don't specific a file name for `grep`. When we omit it, it
   reads from `stdin`.
9. Let's check that the two files we just created are identical by using the
   `diff` program to print out the differences between two files. Run

   ``` sh
   $ diff -u zfiles zfiles1
   ```

   Nothing should be printed when you do that because the files should be the
   same.
10. Append a line of text (anything you want) to `zfiles1` using `echo` with
    an append redirection (`>>`).
11. Rerun the `diff` command from step 9. You should see some output. (Diffing
    files is extremely useful. We'll see more of this later and you'll see a
    lot of it as you program more.)
12. Enough `diff` for now. In step 8, we skipped the creation of one file by
    using a pipe but we still created a second file. Construct a new command
    piping the output of `ls` to `grep` and piping that output to `wc`. You
    should get exactly the output you got in step 7.
13. We actually did more work than we needed to. Remember that the shell will
    expand wildcards that match file names. For example, `/bin/z*` will be
    expanded to the paths of all of the files that match that pattern. Run `$
    echo /bin/z*` to see the result.
14. Using `ls` rather than `echo` (because `echo` prints a single line and we
    want multiple lines) and the appropriate patterns, construct a pipeline to
    count all of the binaries starting with `z` again. That is, fill in the
    missing bits in
    
    ``` sh
    $ ls [...] | wc -l
    ```
15. That's enough for today. You can delete the directory you created.
