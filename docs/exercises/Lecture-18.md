---
layout: default
title: Lecture 18
classdate: 2020-03-13
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. `$ cp -r ~steve/ex/intlist-v2 .`

## Task
1. Run `$ make check` and see that this implementation has some bugs.
2. If we run `$ ./test_initialize`, it appears to succeed so let's check
   `$ ./test_insert`. AddressSanitizer helpfully tells us that we are
   "attempting free on address which was not malloc()-ed" and it gives a
   helpful stack trace showing us which functions were called leading up to the
   crash. Let's debug and see what we can find out.
3. Run `$ gdb test_insert`. At the GDB prompt, enter the following command.
   ```
   (gdb) set environment ASAN_OPTIONS=abort_on_error=1
   ```
   This will cause AddressSanitizer to abort on an error so we can debug it.

   Now use the `run` command to run the program until it crashes. After asan
   prints out the same message, GDB tells us this.
   ```
   Program received signal SIGABRT, Aborted.
   __GI_raise (sig=sig@entry=6) at ../sysdeps/unix/sysv/linux/raise.c:51
   51	../sysdeps/unix/sysv/linux/raise.c: No such file or directory.
   ```
4. That's not really helpful because we didn't write that file. Use the
   `backtrace` or `bt` command to display a stack trace.
   ```
   (gdb) bt
   #0  __GI_raise (sig=sig@entry=6) at ../sysdeps/unix/sysv/linux/raise.c:51
   #1  0x00007ffff6e43801 in __GI_abort () at abort.c:79
   #2  0x000000000050329b in __sanitizer::Abort() ()
   #3  0x00000000005005c8 in __sanitizer::Die() ()
   #4  0x00000000004e03c3 in __asan::ReportFreeNotMalloced(unsigned long, __sanitizer::BufferedStackTrace*) ()
   #5  0x00000000004231e0 in __asan::Allocator::Deallocate(void*, unsigned long, unsigned long, __sanitizer::BufferedStackTrace*, __asan::AllocType) ()
   #6  0x0000000000425224 in __asan::asan_realloc(void*, unsigned long, __sanitizer::BufferedStackTrace*) ()
   #7  0x00000000004da07a in realloc ()
   #8  0x00000000005141d2 in grow_if_needed (list=0x7fffffffdb80, new_capacity=0) at intlist.c:28
   #9  0x00000000005149a1 in intlist_insert (list=0x7fffffffdb80, index=0, x=32) at intlist.c:47
   #10 0x000000000051225a in main () at test_insert.c:10
   ```
5. The first 8 of these aren't interesting to us but `#8` is our code, so use
   the `up` command to move up the stack trace to `#8`.
   ```
   (gdb) up 8
   #8  0x00000000005141d2 in grow_if_needed (list=0x7fffffffdb80, new_capacity=0) at intlist.c:28
   28	  list = realloc(list, new_capacity);
   ```
   This is interesting, we're trying to reallocate `list` but GDB is telling
   us that `list` was allocated on the stack. (If you don't see why that is,
   review the slides from last class.)
6. Using the `print`, `up`, and `list` commands, we see the contents of
   `*list` and that sure enough, in `main`, `list` was allocated on the stack
   but `list->data` is on the heap. We're reallocating the wrong thing!

   Fix that by reallocating `list->data` instead.
7. Rebuild and run `test_insert`. This time, we see a different error, "SEGV
   on unknown address 0x000000000000". "SEGV" is a _signal_. We'll talk about
   signals later. For now, it's enough to know that this is the OS telling us
   we have a "segmentation fault" which really just means we tried to read or
   write to some inaccessible address. In this case, asan tells us we're
   trying to read from address 0. I.e., we've got a NULL pointer somewhere.
8. We're going to be running GDB a lot and it prints a lot of useless
   information at start up. In bash, we can run `$ alias gdb='gdb -q'` to make
   `gdb` start up in quiet mode. You can add that to your `~/.bashrc` file (or
   `~/.bash_aliases` which Ubuntu's default `.bashrc` will source).

   In addition, put `set environment ASAN_OPTIONS=abort_on_error=1` in
   `~/.gdbinit` so that you don't have to enter that command each time you
   start GDB.
9. Run `$ gdb test_insert` and then at the GDB prompt, use `run` to run until
   it crashes. Next, `p *list` shows us that `list->data` is NULL. If we look
   at the stack trace (`bt`) and then print the code listing (`list`), we see
   that the very first call to `intlist_insert` is crashing. So let's set a
   break point, restart the program, and then step through and see if we can
   work out what's going on.
   ```
   (gdb) b intlist_insert
   Breakpoint 1 at 0x514926: file intlist.c, line 46.
   (gdb) run
   ```
10. Print out `*list`. Now step through the code one statement at a time,
    examining variables as needed until you can figure out what has gone
    wrong. Fix it and test your fix works.
11. Repeat the process of building, debugging, and fixing until no bugs
    remain. One thing to keep in mind is the tests aren't complete. If we fix
    one bug, it is important to look at related code to see if the code author
    made the same mistake in multiple places.
