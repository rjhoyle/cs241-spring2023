---
layout: default
title: Lecture 2
classdate: 2022-09-08
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
Log on to clyde: `ssh user@clyde.cs.oberlin.edu`.

## Task
1. Create a directory named `books` using `mkdir`.
2. `cd` into the directory.
3. `wget` to download a copy of Bram Stoker's
   _Dracula_ from `https://www.gutenberg.org/cache/epub/345/pg345.txt`. (Try
   running either `$ wget --help` or `$ man wget` to see how to use `wget` to
   download files.
4. Using `mv` rename the file from `pg345.txt` to `Stoker, Bram - Dracula.txt`.
   Since this file name has spaces, you'll need to specify it as
   `'Stoker, Bram - Dracula.txt'` or `Stoker,\ Bram\ -\ Dracula.txt`.
5. The `grep` command is used for searching for text files for a "pattern" and
   printing out each line that matches the pattern. For example, `$ grep
   vampire 'Stoker, Bram - Dracula.txt'` prints out each line containing the
   word `vampire` (in lower case).

   Read `grep`'s man page to figure out how to perform a case-insensitive
   search and run the command to print out all lines matching `vampire`, case
   insensitively. Hint: typing `/case` (and then hit enter) while viewing a
   man page will search for `case` in the manual. While searching, you can
   press `n`/`N` to go to the next/previous instance.
6. Use `grep` to print out a _count_ of the lines matching `vampire` case
   insensitively. (Search the man page again.)
7. You're probably tired of typing the same command and file name over and
   over, try using the up/down arrows to move back and forth through the
   history of your commands and then editing the commands to make new ones.
   You can also use _tab-completion_ to get Bash to fill in the rest of the
   name for you: start typing the file name and then hit tab.

   Open the man page for `grep` one final time and figure out how to get
   `grep` to print the line numbers (and the lines themselves) that match
   `Transylvania` and then do that.
8. Use `wget` again to download James Joyce's _Dubliners_ from
   `https://www.gutenberg.org/files/2814/2814-0.txt`. Rename it
   `Joyce, James - Dubliners.txt`.
9. Find and use the command to print out a count of every word in both books.
   Hint, the `-a` (or `--and`) option to `apropos` lets you search for
   commands that involve all of the key words so `$ apropos -a apple sauce`
   will match commands whose descriptions contain both the words apple and
   sauce. So use `appropos -a` with appropriate keywords to find a command
   that produces a _word count_.

   Run that command on all `.txt` files in the current directory using
   ```
   $ cmd *.txt
   ```
   where `cmd` is the command you found with `apropos`.

   This is called a glob and we'll talk about it next time.
10. Read the man page for the command you found in step 9 and find and use the
    option to print _only_ the word counts.
11. Go to the parent directory (`cd ..`) and delete the `books` directory.
    Note that neither `$ rm books` nor `$ rmdir books` will work.

    ```
    steve@clyde:~$ rm books
    rm: cannot remove 'books': Is a directory
    steve@clyde:~$ rmdir books
    rmdir: failed to remove 'books': Directory not empty
    ```

    Read the man page for `rm` to figure out how to _recursively_ delete
    directories.
