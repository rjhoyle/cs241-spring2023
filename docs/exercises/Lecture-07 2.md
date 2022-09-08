---
layout: default
title: Lecture 7
classdate: 2020-02-17
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/args` into the directory.

## Task
1. Copy `args` to a new file `args2`. Modify `args2` so that your code is
   inside a function named `main`. After the function definition, call `main`
   as
   ```bash
   main $*
   ```
   and examine what happens when you run the script. In particular, compare to
   `args`. Replace `$*` with each of `$@`, `"$*"`, and `"$@"` and run the
   script after each modification. Make sure you understand the differences
   between the four. (Unquoted `$*` and `$@` should behave the same, but the
   others are different.)
2. Create a new script `architecture`. This script should iterate over each of
   its parameters and print out a friendly name for each computer
   architecture. If the architecture is not known, print it out verbatim.
   ```
   $ ./architecture i386 i486 i686 x86_64 m68k ppc armv6l armv6b \
     armv7l armv7b armv8l armv8b microblaze
   Intel x86
   Intel x86
   Intel x86
   AMD x86-64
   Motorolla 68000
   PowerPC
   32-bit, Little-endian ARM
   32-bit, Big-endian ARM
   32-bit, Little-endian ARM
   32-bit, Big-endian ARM
   64-bit, Little-endian ARM
   64-bit, Big-endian ARM
   microblaze
   ```
   (Here, `microblaze` isn't known to the script so it printed it verbatim.)
   Use a
   [case](https://www.gnu.org/software/bash/manual/html_node/Conditional-Constructs.html#index-case)
   statement rather than a series of `if`/`else` statements. Use patterns with
   wild cards to minimize the number of cases. In particular, the 32-bit Intel
   x86 and the 32-bit ARMs can be simplified.
3. The `uname` command will print a bunch of information about the current
   machine when given the appropriate options. In particular, `uname -m` will
   print out the current machine's architecture. Try running
   ```
   $ ./architecture "$(uname -m)"
   ```
4. Run `shellcheck` on each of the scripts you created. Fix any warnings or
   errors.
5. Bash's parameter expansion can actually do a lot more than expand variables
   to their values. Peruse [the manual][manual] and try out several of these
   in the shell.

   In particular, `${var:-word}` and `${var-word}` will expand to either the
   value of the `var` variable or the expansion of `word` depending on whether
   `var` has been set (and is not empty, depending on using the colon or not).

   For example, imagine printing a message using a variable `name` like this.
   ```
   $ echo "Hello ${name}"
   ```
   Since `name` isn't defined, it prints `Hello ` (try it!). We can add a
   default value.
   ```
   $ echo "Hello ${name-${USER}}"
   ```

   If `name` _is_ defined, then it will print out `name` instead.
   ```
   $ name='Fatima'
   $ echo "Hello ${name-${USER}}"
   ```
6. Substrings can be returned by using `${var:offset}` or
   `${var:offset:length}`. Set some variables and then print out their
   substrings.

   Other useful expansions include `${var#word}`, `${var##word}`,
   `${var%word}`, `${var%%word}`, and `${var/pattern/string}`.

   In new enough versions of Bash (like that on Clyde, but not on macOS by
   default), parameter expansion can change the case of characters. Look in
   the [manual][manual] to figure out how to print out the `USER` variable with just the
   first character changed to uppercase. Now, print out `USER` but with all
   characters changed to uppercase.

[manual]: https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html
