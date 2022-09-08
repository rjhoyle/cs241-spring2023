---
layout: default
title: Lecture 6
classdate: 2020-02-14
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/guess` to the current directory. (Remember, bash will
   expand `~steve` to be Prof. Checkoway's home directory.)

## Task
1.  Modify `guess` to make it interactive by letting the user make multiple
    guesses until the answer is correct using either a `while` or `until`
    loop.
    
    To make it "fun," you probably shouldn't print out the correct
    answer on a wrong guess. Mine looks like this
    ```
    steve@clyde:~/blah$ ./guess
    Guess a number between 1 and 10: 1
    Incorrect. Guess again: 3
    Incorrect. Guess again: 8
    Good guess!
    ```
2.  Write a new script `high-low` that picks a random number between 1 and
    100 and then asks the user to guess the value. After each guess, if the
    answer is incorrect, tell the user if the correct value is higher or
    lower. After the user guesses correctly, tell them how many guesses it
    took.

    Remember, `<` and `>` are for comparing strings. You need to use `-lt` and
    `-gt` to compare integers. If you wish, you can use the `break` command to
    break out of loops.
    
    Mine looks like this.
    ```
    steve@clyde:~/blah$ ./high-low
    Guess a number between 1 and 100: 50
    50 is too low. Guess again: 75
    75 is too high. Guess again: 63
    63 is too high. Guess again: 57
    57 is too high. Guess again: 53
    53 is too low. Guess again: 55
    55 is too low. Guess again: 56
    You got it in 7 guesses.
    ```
3.  Write a new script `scripts` that outputs the name of all files in
    `/usr/bin` that are shell scripts. You can identify if a file is a shell
    script by running the `file` command on the file. This will print out a
    one line description about the file. For example,
    ```
    steve@clyde:~/blah$ file /usr/bin/zipgrep
    /usr/bin/zipgrep: POSIX shell script, ASCII text executable
    steve@clyde:~/blah$ file /usr/bin/awk
    /usr/bin/awk: symbolic link to /etc/alternatives/awk
    steve@clyde:~/blah$ file /usr/bin/host
    /usr/bin/host: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/l, for GNU/Linux 3.2.0, BuildID[sha1]=c3ab218fd658e9fd02c6636bfc5a5d9b4a068737, stripped
    ```
    Check out the `-q` option in the man page for `grep` and remember that the
    "then" arm of an `if` statement runs when the `cmd` in `if cmd; then`
    returns 0.
4.  Run `shellcheck` on `guess`, `high-low`, and scripts. Fix any errors or
    warnings. E.g., "SC2086: Double quote to prevent globbing and word
    splitting." means you should add some double quotes around your use of
    variables.
