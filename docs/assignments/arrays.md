---

layout: default
permalink: /assignments/hw3
title: hw3

---

# CSCI 241 - Homework 3: Arrays

<em>Due by 11:59.59pm Thursday March 02</em>


## Introduction


In this lab you will get experience dealing with data on the binary level.
You will also have to deal with static local variables and using global
variables.  In addition, you'll be expected to use conditional compilation
via a Makefile, creating header files, etc.


**You may work with a partner on this assignment.**

**It is expected that you work together and equally contribute to the development of your solution.  Also, you are both responsible for understanding how your solution works.  You need only submit one assignment per group, but clearly indicate your partnership in the README and comments for files.  You should play with collaborating on github as you are doing this.**



The URL for the github repository for this account is <a href="https://classroom.github.com/a/tZP6yuY5">https://classroom.github.com/a/tZP6yuY5</a>


There are starter files for this assignment at <a href="starters/hw3.zip">hw3.zip</a>

## Part 1 - frequency analysis


In this part, we will start using arrays to contain frequencies of characters
and print a summary of a text's character distribution.



In C, an array is defined as

<pre>
int arr[LENGTH];
</pre>

where LENGTH must be a constant.  Typically, we will declare something like

<pre>
#define LENGTH 256
</pre>

somewhere in our program before we define the array.  Note, you cannot use a variable for the array length.  It must be something that is evaluated at compile time, not at run-time.



Your assignment is to create a program that has an array of all possible lowercase characters (a-z).  Your program will read from stdin, like the programs last week, and count how many times each letter has occured. 


You will then print out, in tabular form, the letter, the number of times that it has appeared, and the percentage of all letters that this letter represents.  Following this table, print out which is the most frequent and least frequent letter.  If there are multiple letters that are most or least frequent, you should print them all out in sequence.


An example run is as follows, using the file "hamlet.txt" located in ~rhoyle/pub/cs241/hw03/hamlet.txt

	rhoyle:hw2 rhoyle$ ./freq < ~/Downloads/hamlet.
	char        Frequencies         Percentage
	a:               9950             7.6459
	b:               1830             1.4062
	c:               2606             2.0025
	d:               5025             3.8614
	e:              14960            11.4958
	f:               2698             2.0732
	g:               2420             1.8596
	h:               8731             6.7092
	i:               8511             6.5401
	j:                110             0.0845
	k:               1272             0.9774
	l:               5847             4.4930
	m:               4253             3.2681
	n:               8297             6.3757
	o:              11218             8.6203
	p:               2016             1.5492
	q:                220             0.1691
	r:               7777             5.9761
	s:               8379             6.4387
	t:              11863             9.1159
	u:               4343             3.3373
	v:               1222             0.9390
	w:               3132             2.4067
	x:                179             0.1375
	y:               3204             2.4621
	z:                 72             0.0553
	Maximum character(s): E
	Minimum character(s): Z

## Part 2 - Counting Change

In this part, you will be creating a program that will count change for us,
optimally.  Your program should print out the minimum number of coins that 
should be returned for the requested amount of change, with the set of coins specified.


Check out the file change_file in the sample documents.  The file is organized as follows:

* Each line is a separate change that your program should calculate.  
* The first value is the change requested.
* The second value is the number of different coins that are available.
* Subsequent numbers are the different coins.

So, the line: 

	17 4 25 10 5 1

means that you want change for 17 cents, using the 4 coins valued at 25, 10, 5, and 1.  

A simple algorithm that works for this is a greedy algorithm.  You pick the largest coin that you can manage, and then pick the next largest, and so on until you have accumulated enough coins to match the change requested.  So, for 17, with regular US coins, first pick a 10 cent coin, leaving you with 7 cents.  You then pick a 5 cent coin, leaving you with 2 cents.  Then, you do 1 cent, leaving you with 1 cent.  Finally, you pick 1 cent, leaving you with 0 cents.  You are then finished, and you have selected a 10 cent, a 5 cent, and 2 one cent coins, for a total of 4 coins.  That is the optimal solution.

However, that greedy algorithm does not work in all cases.  Imagine a universe in which we had a 20 cent coin, a 15 cent coin, a 7 cent coin, and a 1 cent coin.  The greedy algorithm would select first the 20 cent, and then two 1 cent coins, giving a total of 3 coins.  There is a better solution, however, which is to select a 15 cent coin and then a 7 cent coin, for a total of 2 coins.

So, the coin problem with arbitrary coins is an exponential problem.  We need to try each coin, and then with the change left, recurse, and try each coin, and so forth.  This will take a long time for non-trivial amounts of change.

We can cheat, though.  Using a technique called Dynamic Programming, which you'll explore more in an Algorithms course, you can store values that you have discovered, so you don't need to recalculate information that you have calculated previously.  We will do this with arrays.  If we discover that the optimal solution for 22 cents, in the example above, is 2 coins, then we can store those values, and whenever we need to calculate them again, for example if we wanted to explore 42 cents with a 20 cent coin as our first pick, we would not need to recalculate it again.

For this assignment, I want you to complete the program in coins.c.  You will find several sections marked with "TODO" with instructions on how to complete them.  Your task is to complete those sections, and get the program to work.  Make sure you read through the file, as there are some debugging sections that are turned off that may help you.

My implementation, to not use structs since we haven't seen them yet, uses two arrays.  coins_returned stores how many coins are in the optimal solution for a value.  Whenever we encounter an optimal solution, we insert it there and use it to short-circuit our recursion down branches that have been explored previously.  next_coin keeps track of the change that is needed for the next step in the chain.  So, for example, when calculating change for 17 using coins 25, 10, 5, and 1:


* next_coin[17] = 7
* next_coin[7] = 2
* next_coin[2] = 1
* next_coin[1] = 0


This allows us to reconstruct the coins needed.  17 - 7 is 10.  7 - 2 is 5.  2 - 1 is 1, and 1 - 0 is 1.




### Sample program

I've included my sample solutions in <tt>~rhoyle/pub/cs241/hw3/</tt> with
binaries that should work on the lab machines.



## Extra Credit
Modify freq to take into account capital and lower-case letters, as well as numbers and symbols.
Analyze the differences between doing coins as a dynamic programming solution and a regular recursive solution.

## README

As with the first project, I want you to create a file called <tt>README</tt>

1. Your name (and partner's name) and the date
1. A list of the programs with a short one-line description of each
1. A list of all remaining compilation problems, warnings, or errors.	Note that for full marks, it is expected that you will have corrected all of these things.
1. A statement about any valgrind errors which occur
1. An estimate of the amount of time you spent designing, implementing, and
deubgging these programs
1. The honor code statement:
<q class="honor">I have adhered to the Honor Code in this assignment</q>
		



## Submission 

Now you should <tt>make clean</tt> to get rid of your executables and
commit your folder containing your source files, README, and Makefile through git, as you did in last week's assignment.  For a refresher, refer to those instructions.

## Grading

Here is what I am looking for in this assignment:

* A working Makefile with your programs, all, and clean as targets
* All programs should have comments including your name and date at the
top.  Functions should have a brief description of what they do and an
explanation of their return values.
* All programs should compile using <tt>gccx</tt> on clyde
with no compiler warnings or errors.
* All programs should run using <tt>valgrind</tt> on clyde
with no warnings or errors.


<hr>
<address>Last Modified: September 28, 2022 - <a href="mailto:rhoyle@oberlin.edu">Roberto Hoyle</a></address>

