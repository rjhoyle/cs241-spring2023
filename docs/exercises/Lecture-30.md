---
layout: default
title: Lecture 30
classdate: 2020-04-24
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.
2. Copy `~steve/ex/dynamiclib` to your home directory.

## Task
1. We're going to build a dynamic library `libgreeting` and an application to
   link to it (and also do some bug fixes).

   Start by reading the library source which consists of just a single `.c`
   file in `libgreeting` and a header file in `libgreeting/include/greeting`.
   (We'll fix the typo later.)
2. Compile `hello.c`. To do this, we're going to need to tell the compiler to
   compile it as position independent code and we also need to tell the
   compiler where the header files are (it's common to separate out the header
   files which will be installed from source files).
   ```
   $ clang -Wall -std=c11 -fPIC -Iinclude -c -o hello.o hello.c
   ```
3. Next, we need to build our library. Since this is the first version of our
   library, we're going to use an ABI version of 1.
   ```
   $ clang -fPIC -shared -Wl,-soname=libgreeting.so.1 -o libgreeting.so.1.0.0 hello.o
   ```
4. Step 3 should have created the file `libgreeting.so.1.0.0`. If you run `$
   file libgreeting.so.1.0.0` it should tell you that it's a shared object
   (along with some other interesting things). If you run `$ nm
   libgreeting.so.1.0.0 | grep hello` you can see where our `hello()` function
   is defined in the library.
5. Next, we're going to "install" the library and its header files for our
   program to use.

   Copy `libgreeting/libgreeting.so.1.0.0` into the `lib` directory.

   Copy `libgreeting/include/greeting` into the `include` directory.

   Inside `lib`, we need to make two symlinks.
   ```
   $ ln -s libgreeting.so.1.0.0 libgreeting.so
   $ ln -s libgreeting.so.1.0.0 libgreeting.so.1
   ```
6. Our library is installed, so it's time to build our application which is in
   the `src` directory.

   The first step is to compile the source files to object files (this could
   be omitted, especially since it's just a single file for the program, but
   we're going to do it here).

   We need to tell `clang` where to find the header files.
   ```
   $ clang -Wall -std=c11 -I../include -c -o main.o main.c
   ```
7. Now we want to link our application together. We need to inform `clang`
   which library we want (namely `libgreeting.so`) and in which directory to
   look for it.
   ```
   $ clang -L../lib -o prog main.o -lgreeting
   ```
8. Great, now let's run the program: `$./prog`.
9. Well that didn't work. The problem is the dynamic linker doesn't know to
   look in that directory. We can tell it where to look by using an
   environment variable.
   ```
   $ LD_LIBRARY_PATH=../lib ./prog
   ```
10. Using `LD_LIBRARY_PATH` isn't great (you can search the Internet for the
    downsides). Instead, let's tell the linker to include a runtime path in
    the binary. This will instruct the linker to check that path for
    libraries.
    ```
    $ clang -L../lib -Wl,-rpath='$ORIGIN/../lib' -o prog main.o -lgreeting
    ```
11. Now when we run the program normally, it will do the right thing as long
    as the path to the library from the directory of the binary is `../lib`.
    Let's "install" the binary into the `bin` directory. (Just move or copy it
    there.)
12. Let's fix the typo in the library. Edit the library source and fix
    "lirbary."
13. Build a new version of the library. Make sure you rebuild the object file!

    Since this version is compatible with
    the existing version, use the same soname of `libgreeting.so.1` but change
    the output file to `libgreeting.so.1.0.1`.
14. "Install" `libgreeting.so.1.0.1` in the `lib` directory and change the
    symlinks to point to the new version. You can leave the old version in
    place or remove it as you desire.
15. Run the program which should still be in `bin`: `$ bin/prog`.

    If all has gone well, then without needing any changes to the binary, it
    should be using the new version of the dynamic library.
16. Rerun the command from step 10 to build the program, but this time run it
    under `strace`.
    ```
    $ strace -o syscalls -f -e openat clang -L../lib -Wl,-rpath='$ORIGIN/../lib' -o prog main.o -lgreeting
    ```
    This will write the trace to the `syscalls` file, trace the children of
    `clang` (because `clang` will be invoking the linker), and only trace the
    `openat` system call.

    Open `syscalls` up in an editor and search for `greeting`. Which file did
    it open?
17. Run `prog` under `strace`.
    ```
    $ strace -e openat ./prog
    ```
    Which of the `libgreeting.so` files did it open? How did it know to open
    that one when the one opened by the linker in the previous step was
    different?
