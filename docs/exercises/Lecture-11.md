---
layout: default
title: Lecture 11
classdate: 2020-02-26
katex: true
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Create a directory and `cd` into it.
3. Copy `~steve/ex/mc_pi.c`, `~steve/ex/logger.h`, `~steve/ex/logger.c`,
   `~steve/ex/rand.h` and `~steve/ex/rand.c` into the directory.

## Task
1. Based on the example Makefile in the
   [slides](../slides/Lecture-11-Make-and-Compiling.pdf), create a `Makefile`
   to compile the program `mc_pi`. Make sure you look through each `.c` file
   to see which `.h` files it includes so you can set up the prerequisites
   correctly.

   If you have put `-std=c11` in the `CFLAGS` variable (and you should),
   you'll get an error and a warning like this.
   ```
   mc_pi.c:49:18: warning: implicit declaration of function 'getopt' is invalid in C99 [-Wimplicit-function-declaration]
     while ((flag = getopt(argc, argv, "hn:v")) != -1) {
                    ^
   mc_pi.c:52:21: error: use of undeclared identifier 'optarg'
         trials = atoi(optarg);
                       ^
   1 warning and 1 error generated.
   ```
   To fix that, add `-D_POSIX_C_SOURCE=200809L` to `CFLAGS`.

   When you run `$ make`, it should build three object files (those ending in
   `.o`) and the program `mc_pi`.
2. Run `$ ./mc_pi` a few times to see what it does. Run `$ ./mc_pi -h` to get
   the help. Run `mc_pi` again but change the number of trials. Try small
   values and large values. Try nonsensical values (or no value at all).
3. The `touch` command can be used to change the time stamp on a file without
   changing any of its contents. We can use this to test the `Makefile`.

   First, run `$ make` again. If you haven't changed any files, `make` should
   say it has nothing to do. Next, run `$ touch logger.c` and then rerun
   `make`. If everything worked correctly with your `Makefile`, `logger.o` and
   `mc_pi` should be remade but nothing else.
4. Run `touch` on `rand.h` and rerun `make`. Did it rebuild the right files?
5. If you didn't do so in step 1, add phony `all` and `clean` targets. `all`
   should depend on `mc_pi` and have no recipe. `clean` shouldn't depend on
   anything but should remove `mc_pi` and the object files. Make sure you
   don't delete your source files!

   Remember, `all` should be the first target in the `Makefile`. Running `$
   make` and `$ make all` should have the same effect. Test that `$ make
   clean` performs the correct action.

   Recall that to make a phony target, you add it as a prerequisite to the
   `.PHONY` so you should have the line
   ```make
   .PHONY: all clean
   ```
6. Remove `clean` from the `.PHONY: all clean` line of the `Makefile`. Run `$
   touch clean`. This will create an empty file named `clean`. Run `$ make
   clean` and note what happens.
7. Readd `clean` to `.PHONY:` and run `$ make clean` again. It should perform
   the clean action even though the `clean` file is present. Remove the
   `clean` file.
8. Add logging to `rand.c` to log (with `LOG_LEVEL_INFO`) each random value
   that is produced. `log_message` takes a variable number of arguments where
   the first one is the log level, the second is a printf-style format string,
   and the remaining are the arguments to print according to the format
   string's conversion specifiers. You can use `%f` to print out the double.

   Make sure you include the appropriate header _and_ update the dependencies
   in your `Makefile`.
9. Run `mc_pi` with the `-v` argument. You might want to have just a few
   trials to avoid spamming your terminal with a few hundred random values.
   You can also redirect the messages to a file using `$ ./mc_pi -v -n 1000
   2>generated_files`.
