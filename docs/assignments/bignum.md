---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /assignments/hw7
title: hw7

---

# CSCI 241 - Homework 7:Add it up!

<em>Due by 11:59.59pm Wednesday Nov. 16</em>


The GitHub URL for this assignment is <a href="https://classroom.github.com/a/GtrOOv_Q">https://classroom.github.com/a/GtrOOv_Q</a>


## Introduction

For this assignment, I want you to create a program called <tt>additup</tt>
that will add up a sequence of positive integers.  One catch, the integers
can be any number of digits long. 


## Program behavior

Your program should read in a sequence of integers, one per line using the
following algorithm.


1. Skip leading whitespace -- you can consider multiple blank lines as
this, so you are welcome to just use <tt>isspace()</tt> in a while
loop.
1. Skip leading 0's in the number
1. Read in the digits from most significant to least significant (i.e.,
"123" is one hundred twenty-three)
1. Once you run out of digits, discard the rest of the line until a newline
is reached (or EOF)
1. If there was no valid integer at the start of a line, you should return
as if you had read in the value 0.  If you hit EOF without seeing a
number, you should return NULL.
1. Print out the total so far by printing the string "<tt>Total: </tt>"
with one space and then the total seen so far.
1. Once you hit EOF, your program should stop.  Remember that you can signal
EOF by hitting ctrl-D on your keyboard.

## Sample run
<pre class="boxed">
<b>% <span class="typed">./additup</span></b>
	 <span class="typed">1234</span> 
Total: 1234
<span class="typed">9876</span> 
Total: 11110
<span class="typed">1                       bobo</span>
Total: 11111
<span class="typed">1234567890987654321</span>
Total: 1234567890987665432
<span class="typed">^D</span>
</pre>


There is also a sample binary (Linux) for you to play with in
<tt>~hoyle/pub/cs241/hw07/</tt>



## Programming details

In order to support arbitrarily long integers, you'll need to represent your
data in a linked list format.  I'll leave it up to you to decide if you want
singly or doubly linked lists, but the nodes should look like one of the
following:

<pre class="boxed">
<b><u>Singly Linked</u></b>
struct BigInt {
	int digit;
	struct BigInt *next;
};

<b><u>Doubly Linked</u></b>
struct BigInt {
	int digit;
	struct BigInt *next, *prev;
};
</pre>


Some things to keep in mind:

* You should use an <tt>accumulator</tt> to keep track of the sum.  Set
the value initially to 0 and then keep adding it to the next BigInt.
* Addition takes place from the least significant digit on up.
* You need to be able to handle carrying values from one position to the
next.  No <tt>digit</tt> value should be greater than 9.
* Printing takes place from the most significant digit on downward.  This,
coupled with addition above, means you'll need to have to traverse your
list in both directions (either doubly-linked or just reverse it as
needed).  <em>Note:</em> do not rely upon the recursion to be able to do
the reversing for you.

### Error conditions

Check to see if <tt>malloc()</tt> fails.  If it does, print a message and
immediately exit with a non-zero value.  (e.g., <tt>EXIT_FAILURE</tt> from
<tt>exit(3)</tt> or something from the <tt>sysexits(3)</tt> group if you'd
prefer)


### Output matching

As there is very little output specified, I'm going to require that you
match the sample output exactly.


### Keep your memory clean and tidy

To keep you thinking about your memory management, I want you to clean up
<em>all</em> memory that you dynamically allocated before the end of the
program.  That means that you should be able to run under <tt>valgrind</tt>
with no memory leaks reported.


## Testing

I've provided a program called <tt>gen_nums</tt> that will generate random
positive integer sequences for you.  It is located in
<tt>~rhoyle/pub/cs241/hw07/</tt> as well.  It requires 2 parameters with
an optional third.  They are:


1. A positive integer indicating the max # of digits in each number
1. A positive integer indicating the number of numbers to be generated
1. An optional positive integer to be used as a random number seed.



I'm using the C pseudo-random number generator so that you should get
identical sequences of numbers for the same input parameters.  You can vary
the seed to get different sequences if desired.


<pre class="boxed">
% <span class="typed">./gen_nums 10 3 1</span> 
6753
629127
9
% <span class="typed">./gen_nums 10 4 1</span> 
6753
629127
9
6062
% <span class="typed">./gen_nums 10 4 2</span> 
9
518475729
3794370229
710063
</pre>


## Extra Credit

Handle multiplication as well as addition.


<hr>
## handin
### README

Create a file called <tt>README</tt> that contains

1. Your name and a description of the programs
1. A listing of the files with a short one line description of the contents
1. Any known bugs (including valgrind warnings) or incomplete functions
1. An estimate of the amount of time you spent completing this assignment
1. Any interesting design decisions you'd like to share

Now you should <tt>make clean</tt> to get rid of your executables and
handin your folder containing your source files, Makefile, and README. 


## Grading

Here is what I am looking for in this assignment:

* A working Makefile with your program, all, and clean as targets
* A program that will add up any length of positive integers, ending after
receiving an EOF
* An internal linked-list representation using structs
* Output matching the sample program
* Appropriately modular code
* Good comments
* Runs under valgrind with no errors or warnings
* A README with the information requested above.  The listing of known
bugs is important.

<hr>
<address>Last Modified: Nov 02, 2022 - <a href="mailto:rhoyle@oberlin.edu">Roberto Hoyle</a></address>

