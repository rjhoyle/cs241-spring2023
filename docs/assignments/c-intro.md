---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /assignments/hw2
title: hw2

---

# CSCI 241 - Homework 2 - Introduction to C
Due by 11:59.59pm Tuesday, February 14


For this assignment you will be:

<ul class="padded">
    <li>Creating several small C programs</li>
    <li>Writing your own Makefile</li>
    <li>Learning about shell redirection</li>
    <li>Learning to use <tt>diff</tt> to compare outputs</li>
    <li>Learning how to use github for submission</li>
</ul>

## Part 1 - "Hello, World!"

We're going to start things off nice and simple.  I'll recommend you create a
<tt>cs241</tt> directory in your CS account and a <tt>hw3</tt> directory inside
that.


First off, go to [https://classroom.github.com/a/MrPne5G8](https://classroom.github.com/a/MrPne5G8) and create a repository in the CS 241 organization.

Click on the link and head towards the created repository.  It will be called <b>hw02-username</b>


I put some starter files in <a href="starters/hw2.zip">hw2.zip</a>.  Feel free to use them or not, but your files should be named the same as they are in that .zip file.  There's also a grader.sh script there.  More on that later.


Go back to your CS account to the cs241 directory above, and clone the repository into this account.  Refer to the github tutorials for how to do this step.



Now that you've got a place for files, go ahead and re-create the "Hello,
World!" program from class.  Try to compile it using <tt>gccx</tt>.
Remember that this is just a shorthand for

<pre>
   <tt>gcc -g -pedantic -std=c99 -Wall -Wextra</tt> 
</pre>

So if you are playing along from home, you need to be sure that you get no
compilation errors or warnings from compiling using these options.  Feel
free to use <tt>clang</tt> in place of gcc.


<h2>Part 2 - Create a Makefile</h2>

Now that you've written a C program, you need to write a <tt>Makefile</tt> to
help you compile and revise it.

<ol class="padded">
    <li>Create a file called <tt>Makefile</tt></li>

    <li>Create a target called <tt>hello</tt> that compiles the code from Part
    1 into an executable called <tt>hello</tt>. Don't forget the source code
      dependency.  Remember that the rules line needs to have a <em>tab</em>
      character at the start.
    
    Be sure you have this working.  If <tt>make</tt> keeps telling you
    everything is up to date, you can just re-save your source file or
    <tt>touch</tt> it to get make to run again.
    
    <pre>
    <tt>touch hello.c</tt></pre>
    </li>
        
    <li>Once this is working, add in a target at the top called <tt>all</tt>
    that depends on <tt>hello</tt>.  Test to see if this works.</li>

    <li>Now add in a rule at the bottom called <tt>clean</tt> with no
    dependencies.  Have it delete <tt>hello</tt> but <em>not</em>
    <tt>hello.c</tt>.</li>
</ol>


If you are using emacs or vim, you can invoke <tt>make</tt> from within
your editor window.  Both editors can monitor the output and let you jump
directly to the error reported.  


### Personalizing hello world

Once you have this working, I'd like you to make a few changes in your
first program.

<ol class="padded">
        <li>Add a comment section at the top with your name and a description
        of the program</li>

        <li>Personalize the message that the program prints out.  Add in at
        least 5 more <tt>printf()</tt> commands with the following conditions:
        <ul>
            <li>At least one decimal number printed with <tt>%d</tt></li>
            <li>At least one decimal number printed with <tt>%Xd</tt> where <tt>X</tt> is an integer number</li>
            <li>At least one floating point number printed with <tt>%f</tt></li>
            <li>At least one floating point number printed with <tt>%X.Yf</tt> where both <tt>X</tt> and <tt>Y</tt> are integers</li>
            <li>At least one floating point number printed with <tt>%.Yf</tt> where <tt>Y</tt> is an integer</li>
        </ul>
        Kudos if you make it somehow interesting to read (poem, story, etc.). 
    </li>
</ol>

<h2>Part 3 - rot128</h2>

Next you will create a program called <tt>rot128</tt> that will "encrypt"
files by using a rot-128 encryption algorithm.  This algorithm is based on
the classic text encryption scheme rot-13 which shifts each letter 13
positions in the alphabet (i.e., 'a' becomes 'n', 'b' becomes 'o', 'z'
becomes 'm', etc.).  Two applications of rot-13 returns you to the original.
<small>(Sadly, this has been used as actual protective encryption in
commercial settings.)</small>

<div id="message">
<pre class="boxed" id="crypt">
    Guvf vf zl irel frperg zrffntr rapbqrq va ebg13!
</pre>
<pre class="boxed" id="clear">
    This is my very secret message encoded in rot13!
</pre>
</div>

Instead of just rotating 13 positions, I want you to rotate by half the
allowed range of a <tt>char</tt>, that way we can encrypt and decrypt any
file on the system.  Normally, a char is 8-bits, but since we can't be sure
of that, you should have the program calculate using the proper constant.
(You will need to <code style="prettyprint lang-c">#include &lt;limits.h&gt;</code> to use this value.)

<pre><tt>
    (UCHAR_MAX + 1) / 2
</tt></pre>


Add in lines to the Makefile that compile the target <tt>rot128</tt> and
add it into both the dependencies for <tt>all</tt> and as part of the
files removed by <tt>clean</tt> (being careful not to delete your source
code file).



When you write your program, you should just add the above value to all
characters you read in, and immediately write them out.
You can use <tt>getchar()</tt> and <tt>putchar()</tt>
to handle the input and output.  
Be aware that <tt>getchar()</tt> returns an int and you will need to do your
processing on a char to have things loop around correctly.
(Note that the resulting output won't be comprehensible, see the next section about how to store it.)



Be careful to not write out the EOF signal.



### Redirecting files in and out of a program

Your "rot128" program reads in from the user typing and writes out to
the console.  You can <em>redirect</em> the input from a file and also
redirect the output to a file.  Use the "<tt>&lt;</tt>" character to
redirect an input file, and the "<tt>&gt;</tt>" to redirect output as
follows:

<pre>
    <tt>./rot128 <em>&lt;</em> inputfile <em>&gt;</em> outputfile</tt>
</pre>

### Using diff

There is a tool called <tt>diff</tt> on Unix systems that will report the
differences between files.  You can use this to check to see if your
program is indeed working.


<pre>
    % vi mytext                         # create a text file
    % ./rot128 < mytext > mytext.enc    # create an encrypted file

    % diff -q mytext mytext.enc         # ask to see if they are different
                                        # -q tells it to not show the
                                        # differences

    <tt>Files mytext and mytext.enc differ</tt>

    % ./rot128 < mytext.enc > mytext.out # run your program on the DOS text

    % diff -q mytext mytext.out          # should have no output as they are
                                         # the same
</pre>

## Part 4 - ASCII art

Finally, you'll be creating a program called <tt>diamond</tt> that
generates a simple ASCII art diamond of variable size based on user input.

<h3>getdigit()</h3>

First, I want you to create a function called <tt>getdigit()</tt> that returns
an integer.  I.e., 

<pre>
    int getdigit();
</pre>

    with the following properties:

<ul class="padded">
    <li>Reads characters using <tt>getchar()</tt> until either it sees either
    a value between '0' and '9' or EOF.  You might find the <tt>isdigit()</tt>
    man page enlightening.
    <ul>
            <li>If it sees EOF, return -1</li>
            <li>If it sees a digit, return the integer corresponding to that.
            E.g., '3' should return 3.</li>
    </ul>
    <li>Ignore all other values input by the user, only use the first digit
    encountered.  For example, a user typing "abc123" returns 1, a user typing
    "452" returns 4, and so forth</li>
    </li>
</ul>


Prompt the user to input an size, and then use that to create a diamond
shape.  The size the user inputs is the height of one of the triangles
that make up the shape (the distance from a point to the center).


### Sample output
<pre>

    % ./diamond

    I will print a diamond for you, enter a size between 1-9: <span class="typed">abc123</span> 
    *

    % ./diamond
    I will print a diamond for you, enter a size between 1-9: <span class="typed">568</span> 
        *
       ***
      *****
     *******
    *********
     *******
      *****
       ***
        *

    %
</pre>

<h2>Part 5 - README</h2>

Finally, I want you to create a file called <tt>README</tt> (note all caps
and no file extension) which contains the following sections:

<ol class="padded">
    <li>Your name and the date</li>
    <li>A list of the programs with a short one-line description of each</li>
    <li>A list of all compilation problems, warnings, or errors.  Note that for
        full marks, it is expected that you will have corrected all of these
        things.</li>
    <li> How long it took you to complete each part.</li>
    <li>The honor code statement:
    <q class="honor">I have adhered to the Honor Code in this assignment</q>
        </li>
</ol>

You may also want to run the grader.sh script at this point.  It is similar to what we will use to grade your submission.  It is highly recommended that the script run without major issues before you submit.

## Part 6 - Submission

Now you should <tt>make clean</tt> to get rid of your executables and
handin your folder containing 4 files (don't worry if your *.c files have
different names as long as the output is the same):

<ul>
    <li>Makefile</li>
    <li>hello.c</li>
    <li>rot128.c</li>
    <li>diamond.c</li>
    <li>README</li>
</ul>


Run git commit to push your changes.  You should check via the github web interface to make sure that your files have been submitted properly.

  
<h2>Extra Credit</h2>
<ul>
	<li>Read in a number from the command line for rot128 and use that as the base for rotation.</li>
	<li>Make the diamond inverted if the user types in a negative number.  So, instead of writing a '*' inside the diamond, write it outside the diamond and a ' ' inside.</li>
</ul>

## Grading

Here is what I am looking for in this assignment:

<ul class="padded">
    <li>A working Makefile with your programs, all, and clean as targets</li>

    <li>A working hello.c with requisite output</li>

    <li>A working rot128.c</li>

    <li>A working diamond.c</li>

    <li>All programs should have comments including your name and date at the
    top.  Functions should have a brief description of what they do and an
    explanation of their return values.</li>

    <li>All programs should compile using <tt>gccx</tt> on OCCS or Clyde
    with no compiler warnings or errors.</li>
</ul>
<hr>
<address>Last Modified: Sept. 20, 2022 - <a href="mailto:rhoyle@oberlin.edu">Roberto Hoyle</a>, based on material from Benjamin Kuperman</address>
