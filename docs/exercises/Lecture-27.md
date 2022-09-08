---
layout: default
title: Lecture 27
classdate: 2020-04-17
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.

## Task
1. [Last time](Lecture-26.html), you wrote two versions of a program to
   compute a 1-byte checksum. If you completed them, you may use those
   programs. Otherwise, you can copy `~steve/ex/checksum-v2` to your home
   directory and use my solutions.

   If you use your own, edit the `Makefile` to change
   `-fsanitize=address,undefined` to `-fsanitize=undefined` in both the `CFLAGS`
   and `LDFLAGS`. Run `$ make clean` and then `$ make` to rebuild without the
   Address Sanitizer.
2. You can time how long a Bash command takes to run but prefixing the command
   with `time`. Time how long `checksum` and `checksum2` take to run on
   themselves:
   ```
   $ time ./checksum checksum checksum2
   $ time ./checksum2 checksum checksum2
   ```
   Time will print out three numbers, the total (`real`) time it took
   to run the command and how much of that was spent in your code (`user`)
   versus the kernel (`sys`).

   You probably saw that `checksum` spent significantly more time in the
   kernel than `checksum2`.

   Let's figure out why that is.
3. You're going to use `strace(1)` to trace the system calls made by a
   program. We're interested in file operations since that's what the
   difference between the two programs are. We can use the `-e` option to
   limit the set of system calls `strace(1)` prints out. To make things
   shorter, let's run the checksum programs on the `Makefile`.
   ```
   $ strace -e trace=open,openat,close,read,write ./checksum Makefile
   $ strace -e trace=open,openat,close,read,write ./checksum2 Makefile
   ```

   In both cases, it will open, read from, and close files you don't care
   about. You can ignore those lines of output and just focus on the ones
   where it's opening and reading from the `Makefile`. I.e., focus on the
   lines starting with
   ```
   openat(AT_FDCWD, "Makefile", O_RDONLY)  = 3
   ```
   (`openat(2)` is a system call like `open(2)` except it takes the path to be
   relative to a directory. You can read about its operation in its man page,
   if you like.)

   Why did `checksum` take so much more time than `checksum2`? Next time you
   find yourself writing code to read and write from files, think carefully
   about whether you want to use a `FILE *` or a file descriptor.
4. In Bash, we can redirect output of an arbitrary command to a file like this
   `$ cmd >output.txt`.

   Let's figure out how Bash does that using `strace(1)`. Bash's `-c` option
   takes a string to run as input. E.g., if we run
   ```
   $ bash -c '/bin/echo hello >output.txt'
   ```
   in Bash, then this will start a new instance of Bash which will run the
   command in quotes and then exit.

   We can use strace's `-f` option to trace children processes (remember, Bash
   will first `fork(2)` and then use `execve(2)` to transform the child
   process into `/bin/echo`.

   Run this command.
   ```
   $ strace -o syscalls.txt -f bash -c '/bin/echo hello >output.txt'
   ```
   Passing `-o file` to `strace(1)` causes it to write its output to `file`
   rather than `stderr`.
   
5. Open `syscalls.txt` in an editor (Vim will highlight the `strace(1)` output
   by default. There's an Emacs mode that will do it too, but it doesn't
   appear to be installed on Clyde.) There's a lot of output here. The number
   preceding each line is the process id (PID) of the process that made the
   system call.

   Let's find the `fork(2)` in the output. Interestingly, Linux doesn't use a
   `fork` system call. Instead, it uses `clone` which is more general. When I
   did this, I got the line
   ```
   8038  clone(child_stack=NULL, flags=CLONE_CHILD_CLEARTID|CLONE_CHILD_SETTID|SIGCHLD, child_tidptr=0x7f76f75c2a10) = 8039
   ```
   which tells me that the Bash with PID 8038 forked and the child had
   PID 8039. The next few lines of `rt_sig...` system calls are changing
   signal handling masks, we'll talk about this next time.

   There's also a `getpid(2)` and `wait4(2)`. The latter is just like
   `wait(2)` but with more options.

   Finally, there's an `openat(2)` which opens `output.txt` for writing,
   creating it if it doesn't already exist, and truncating the file if it
   does.

   Starting with the `openat(2)` and going up to the system call just before
   the `execve("/bin/echo", ...)` is the sequence of system calls that Bash
   made to perform the redirection. There's only one new one there. Read its
   corresponding man page.

6. Copy `~steve/ex/redir` to your directory. Modify `redir.c` such that when
   you run `$ ./redir -o file command arguments` it runs `command arguments`
   with `stdout` redirected to `file`. You only need to add some lines inside
   the `if` statement to open the file and perform the redirection. You can
   make a similar set of system calls that Bash did to perform the
   redirection.
