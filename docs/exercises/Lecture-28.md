---
layout: default
title: Lecture 28
classdate: 2020-04-20
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Copy `~steve/ex/sig` to your home directory.

## Task
1. `cd` into `sig` and look at `block.c`. It installs a signal handler for
   `SIGINT`, masks `SIGINT`, sleeps for three seconds, and then unmasks
   `SIGINT`.

   Run `$ make` to compile `block`. Run `$ ./block` and let it sit until it
   exits. Nothing should be printed.

   Run it again, but this time, press `ctrl-C` a few times while it is
   sleeping. See how many times the message printed from the signal handler
   happens. (Note that although we cannot use `printf(3)` from a signal
   handler, we _can_ use `write(2)`.)
2. Now, take a look at `sig.c`. Modify the code such that when `ctrl-C` is
   pressed, `interrupt_count` is incremented and when `ctrl-\` is pressed
   (which sends `SIGQUIT`), `done` is set to `1` to exit the loop. Make sure
   to uncomment the loop.

   To do this, you'll need to use `sigaction(2)` twice to install a signal
   handler for `SIGINT` and `SIGQUIT`. You can use the same function or two
   different functions. I suggest copying the relevant code from `block.c`.
3. Build and run `./sig`. Try pressing `ctrl-C` a few times and then press
   `ctrl-\`. It should print the number of times you pressed `ctrl-C`.
4. Run `./block` in a short loop
   ```
   $ for i in {1..3}; do ./block; done
   ```
   and press `ctrl-C` while it is running.

   You'll notice that after `SIGINT` is unmasked and the program exits, the
   `for` loop keeps running and `block` will be run again. This is because the
   signal handler caught and handled the signal so Bash didn't know that it
   had exited due to a signal.
5. Change the `sa_flags` to be `SA_RESTART | SA_RESETHAND`. This will reset
   the signal handler to the default behavior as soon as the signal is
   delivered to the handler.

   Add `raise(sig)` at the end of the handler to raise the signal again.

   Rerun the loop from part 4 above and press `ctrl-C`. This time, it should
   exit immediately.
