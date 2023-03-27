---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /assignments/hw6
title: hw6

---

# CSCI 241 - Homework 6: Sorting it all out<br>Creating a  Unix tool

**Due by 11:59.59pm Wednesday, April 12th**


The Github URL for this assignment is <a href="https://classroom.github.com/a/a3Kgxt2x">https://classroom.github.com/a/a3Kgxt2x</a> As there are a number of components to this assignment, I'd
encourage you to get started before the last minute.


## Introduction


You will be
creating a copy of a commonly used Unix tool; a tool that allows you to sort text in various ways.



You'll also learn about the C99 replacement for <tt>atoi()</tt> and get 
some additional experience working with strings.



As with the previous assignment, I'd like you to read through the project
specification and then formulate an estimate of the length of time
that you expect it will take to complete the project.
Record your estimate in the README before you begin coding.



<hr>

## sort

The tool you'll be creating is <tt>sort</tt>, modeled after the Unix
tool of the same name.

This program reads in lines of text and then sorts them based on specified
criteria.  By default, it does so using the ordering of <tt>strcmp()</tt>.


### Example

<pre class="boxed">
<b>INPUT:</b>
ALICE
bob
BOB
CAROL
alice
carol
</pre>

<pre class="boxed">
<b>OUTPUT:</b>
ALICE
BOB
CAROL
alice
bob
carol
</pre>

### Folding case

Notice how all the capital letters come before the lower case ones?  That is
not a mistake.  The numeric value of 'A' is 32 less than the value of 'a' so
it is natural for them to come first. 
(See notes below if odd behavior occurs.)



However, sometimes you don't want this to take place.  Therefore you should
support the <tt>-f</tt> flag which <u>folds</u> lowercase letters into
uppercase ones (i.e., case insensitive comparisons).


<pre class="boxed">
<b>OUTPUT:</b>
ALICE
alice
BOB
bob
CAROL
carol
</pre>


Ordering of otherwise identical upper vs. lower case lines is up to the
sort, not something you need to handle.


### Numeric sorting

Sometimes, you want to sort lines based on a leading number.  To do that
you'll use the <tt>-n</tt> flag.  Note that this should ignore leading
whitespace, treat the first thing as a number, then sort based on the rest
of the line as before if multiple lines have the same leading number.




<pre class="boxed">
<b>INPUT:</b>
0    Alice
99   Bob
20   Carol
172  Eve
76   Mallory
9    Trent
59   Plod
14   Steve
</pre>

<pre class="boxed">
<b>OUTPUT:</b>
0    Alice
9    Trent
14   Steve
20   Carol
59   Plod
76   Mallory
99   Bob
172  Eve
</pre>


I'd like you to do this by implementing <tt>long mystrtol(char *start, char
**rest)</tt> based on the <tt>strtol()</tt> function added in to C99 to
replace <tt>atoi()</tt>. 



This function should take a pointer to the start of a string, skip leading
whitespace, and attempt to convert the next characters into a <tt>long
integer</tt>.  If it isn't a number, you should return 0. 



The second parameter is where you can pass in the address to a character
pointer.  If this value is not <tt>NULL</tt>, you should store the address
of the first character **after** the number you interpreted.  If no
number was interpreted, you should just set it to the start of the line,
whitespace included. 



<b>NOTE:</b> The real version of <tt>strtol()</tt> takes a third parameter
that lets you specify the base of the number being interpreted, but we
aren't implementing that.


<pre class="prettyprint lang-c">
/* Example of mystrtol */

char line[] = " -56   Cards in a negative deck";
long value;
char *rest;

value = mystrtol(line, &amp;rest);

// After the call, the following should be true
assert(value == -56);
assert(rest == &amp;line[4]);
</pre>

### Reversing the sort

You sometimes want things in descending order instead of ascending
order.  To do this, you will support the <tt>-r</tt> flag to reverse the
sort, regardless of the other options specified.  So you can do reverse
numeric, folded, or normal sorts.



### Programming notes

Some guidelines you should follow when working on your solution:

<ul class="padded">
<li>Options are always given as separate fields.  You don't need to handle
		things like <tt>-js</tt> for this.</li>

<li>See the <tt>strcmp()</tt> man page for pointers to other comparison
functions, like those that ignore case.</li>

<li>If <tt>strcmp()</tt> seems to already be ignoring case, check the value
of the <tt>locale</tt> command. Do things change if you do something like
"LC_COLLATE=C sort" when you run your command?</li>

<li>Read all of your input in from <tt>stdin</tt>.  You can do this using
<tt>getchar()</tt> -- just don't forget to include a null byte at the end of
each line.</li>

<li>The maximum line length you should read is 1024 bytes.  If an input line
		is longer than this, only store the first 1024 bytes (but you need to
		consume the rest of the line).</li>

<li>Use an array of character pointers to store your pointers to lines.</li>

<li>Use a constant to set the size of the array of char pointers at
		1,048,576 lines (1024 * 1024).  You will need to dynamically allocate
		the array (i.e., use <tt>malloc</tt>).</li>

<li>Use <tt>malloc()</tt> to allocate storage for each line you read.</li>

<li>Use <tt>free()</tt> at the end of your program to deallocate
		everything.</li>

<li>Use <tt>valgrind</tt> to verify that you did deallocate it all.</li>

<li>Use the built-in <tt>qsort()</tt> function to sort your lines.</li>
</ul>

<hr>
## More on command line arguments

In addition to the flags listed above, I'd like you to implement a
<tt>-h</tt> and <tt>-?</tt> flag that prints out a brief usage message and
then exits the program with a non-zero value.  You should do the same
behavior if an unknown flag is passed to your program.



You should have your program's main function return 0 upon successful
completion of the assigned task.


<hr>
## handin
### README

Create a file called <tt>README</tt> that contains

<ol>
<li>Your name and a description of the program</li>
<li>A listing of the files with a short one line description of the contents</li>
<li>Any known bugs or incomplete functions</li>
<li>Your estimated time from the start and the actual time taken, feel free to speculate as to the reason for any differences</li>
<li>Your affirmation as to the honor code if you followed it</li>
</ol>

Now you should <tt>make clean</tt> to get rid of your executables and
handin your folder containing your source files, Makefile, and README. 


<pre class="boxed">
% <tt>git add</tt>
% <tt>git commit</tt>
% <tt>git push</tt>
</pre>

## Extra Credit
For Extra Credit, implement QuickSort or MergeSort instead of using the built-in function.  Compare how fast your implementation is to the library one.

## Grading

Here is what I am looking for in this assignment:

<ul class="padded">
<li>A working Makefile with your program, all, and clean as targets</li>

<li>Appropriately modular code</li>

<li>Good comments</li>

<li>A working <tt>mystrtol()</tt> implementation</li>

<li>All flags working as described above</li>

<li>Programs which run under <tt>valgrind</tt> without errors</li>

<li>A statement about any valgrind errors which occur</li>

<li>A README with the information above.  The listing of known bugs is
important.</li>

</ul>

<hr>
<address>Last Modified: Oct 29, 2015 - <a href="mailto:roberto.hoyle@cs.oberlin.edu">Roberto Hoyle</a></address>

</body>
</html>

<!-- vim: set fdm=marker tw=80: -->
