---
layout: default
title: Lecture 4
classdate: 2020-02-10
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

This exercise is designed to give you hands-on experience with Bash's
expansion.

## Setup
1. Log on to clyde: `ssh user@clyde.cs.oberlin.edu`.
2. Create a directory with whatever name you like and `cd` into it.
3. Run `$ cp ~steve/ex/args .` (note the trailing dot!) to copy the `args`
   program to your directory.

## Task
1. Either run `$ cat args` to print the contents of `args` or open `args` in
   an editor (e.g., `$ nvim args` or `$ emacs args`). `args` is a Bash script
   that will print out the command line arguments that you pass it.

   The `#` character starts a comment, just like Python. Later in this course,
   we'll learn what all of this is, but for now, you can probably see the
   structure of the program. There's a `num` variable that is incremented each
   time through the `for` loop. The `echo` line produces the output and you
   can see `${num}` and `${arg}` which will expand to the command line
   argument number (its position in the list) and the command line argument
   itself.

   The most mysterious part is the `"$@"` which tells the `for` loop to loop
   over the command line arguments, setting `arg` to each one in turn.
2. Splitting input into words. Run the `args` program several times with
   different command line arguments such as
   ```
   $ ./args
   $ ./args aa bb    cc
   $ ./args -a --version foo bar
   ```
   Try adding more or less space characters between each argument. Does it
   have any impact on what arguments the `args` script receives?
3. Try to construct a command line argument that contains spaces. (Hint: the
   `args` program itself shows how to do that.) Test that you're correct by
   running `args` with the appropriate command line argument.
4. There are 3 different ways we can get spaces in the command line arguments:
   single quotes, double quotes, and a backslash before each space. Run the
   following and examine the output (you'll probably want to copy and paste
   rather than type this in).
   ```
   $ ./args '  foo  bar  ' "aaa bbb" this\ is\ one\ arg
   ```
   There should be exactly three command line arguments printed out.
5. Now try putting backslashes inside single or double quotes.
   ```
   $ ./args 'foo\\bar' "foo\\bar" "foo\bar" foo\\bar foo\bar
   ```
   If you look carefully at that, you'll see that inside single quotes a
   backslash is treated literally (indeed _everything_ inside single quotes is
   treated literally). Inside double quotes (or outside quotes), the backslash
   acts as an escape so `"\\"` produces just a single slash. Since `\b`
   doesn't mean anything to Bash, the backslash doesn't act as an escape.

   As a final step of input processing, Bash removes unquoted (and unescaped)
   quotation marks and backslashes. This explains why we don't see any
   quotation marks in the arguments and why the backslash disappeared from
   `foo\bar`.
6. Brace expansion. Try the following examples.
   ```
   $ ./args {foo,bar,baz}.mp3
   $ ./args before{X,Y,,Z}after # the ,, isn't a typo
   $ ./args {a,b{c,d},e}{f,g}
   ```
   Notice how Bash (recursively) expands each comma-separated string inside
   the braces into its own argument, prepending the stuff before it and
   appending the stuff after it.
7. Try adding quotes (single and double) outside some of the braces. Try
   inside some of the braces. Try adding a space to one of the comma-separated
   items

   These are instructive.
   ```
   $ ./args before{X,Y, ,Z}after
   $ ./args before{X,Y,' ',Z}after
   ```
8. Tilde expansion. `~` expands to your home directory, `~foo` expands to user
   `foo`'s home directory. Try it out using `~steve` to get my home directory.
   (Notice how we used this in the setup to copy `args` from my home
   directory.)

   Try adding quotes or a backslash in front of the `~`. Try putting the `~`
   in the middle of the word like `foo~bar`. What happens?
9. Parameter (variable) expansion. This is the most useful one of all! Define
   a new Bash variable `class` by entering the following.
   ```
   $ class='CS 241'
   ```
   Note carefully that the `=` must not have spaces on either side of it.

   First, let's echo (i.e., print out) the value of the `class` variable.
   ```
   $ echo ${class}
   ```
   That should print out `CS 241`.
10. Now try passing the `class` variable to the `args` program.
    ```
    $ ./args ${class}
    $ ./args '${class}'
    $ ./args "${class}"
    ```

    Note carefully the difference between these: `"${class}"` expanded to a
    single argument whereas `${class}` expanded to multiple arguments. What
    happens is after doing parameter expansion (and several other expansions),
    Bash splits the result into multiple words by splitting (by default) at
    whitespace.

    **You will never want this behavior.** You can avoid it by putting double
    quotes around any use of variables.
11. Read the slides discussing Bash expansion, in particular command
    substitution and quote removal. Play around with some of the examples on
    the slides. For example, compare
    ```
    $ ./args $(echo hi there)
    $ ./args "$(echo hi there)"
    ```
