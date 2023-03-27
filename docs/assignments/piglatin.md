---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /assignments/hw5
title: hw5

---
# CSCI 241 - Lab 5 : Pig Latin
**Due by 11:59.59pm Wednesday April 05 at 11:59pm**

The URL for this github repository is <a href="https://classroom.github.com/a/HGQ2dkDs">https://classroom.github.com/a/HGQ2dkDs</a>  If your partner is submitting an assignment, please submit one yourself, pointing at the partner's repository.


## Introduction

For this assignment create a program <tt>pig</tt> that will read in
English text and translate it into Pig Latin. In particular you'll
be working with strings in C.

I'm interested in seeing how much time students estimate an
assignment will take versus how much time they actually spend on
the assignment. What I'd like you to do is read through this
assignment and create a README with your estimated time to
complete it. (Feel free to list time for individual components
as well if you'd like.) After you get done, I'd like you to add
in the actual amount of time you spent.

<hr>

## Programming Details

Pig Latin is used by both adults and children to obfuscate what they
are saying from the other. Some of you might have toyed around with
the language yourself as a child.


### Translating Pig Latin

For this assignment, you'll be creating a programs <tt>pig</tt>. <tt>pig</tt> will read in English text and
convert it into pig latin. 


### Pig Latin Translation Rules

 There are a couple of varying
rules to Pig Latin. For this assignment, we'll be using the
following:

1. Rule 1: If the string begins with a vowel
(that is begins with 'a', 'e', 'i', 'o', or 'u') add "yay" to
the end of the string.
1. Rule 2: Otherwise, find the first occurance
of a vowel, then move all the letters before the vowel to the
end of the word and add "ay". Note: "y"
should be considered a vowel in this context.

You can consider a word to be a consecutive sequence of letters
(A-Z, a-z). "y" should be treated as a consonant if it is the first
letter of the word but as a vowel otherwise.


### Punctuation &amp; Grammar

In addition to modifying the words as specified above, your program
should also respect the following rules:

<ul>
<li>
		Puntuation in sentences should be preserved (periods, commas
		whitespace, tabs, etc.)
</li>
<li>
		Capitalization should be preserved. The original capital letter
		should be lowercased and the new initial letter should be
		capitalized. So, Linux would become Inuxlay.
</li>
<li>
		The letter pair "qu" together should be treated as a consonant,
		rather than a single character.
</li>
<li>
		Hypenated words should be treated as two words.
</li>
</ul>

Some dialects of pig latin say that you should move "silent" letters
to the end as well (such as in the word knife). You can ignore
contraction words (don't).

### Examples

In general, the words will be translated as follows:


* Linux -> Inuxlay
* yes -> esyay
* example -> exampleyay


<hr>

## Programming Notes

Some guidelines you should follow when working on your solution:

<ul>
<li>
No word we use in our testing will be longer than 100
characters.
</li>
<li>No line will be longer than 1024 characters</li>
<li>
<tt>strcpy</tt>, <tt>strcat</tt>, <tt>strdup</tt> are useful
functions for this assignment.
</li>
</ul>

You'll want to think about how to breaks this program into its
component parts.


<hr>

## Command line arguments

Additionally, I'd like you to implement a <tt>-h</tt> and
<tt>-?</tt> flag that prints out a brief usage message and exits the
program with a non-zero value. You should do the same if an
unknown flag is passed to the program.


You should have your program's main function return 0 upon
successful completion of the assigned task.



<hr>

## Extra Credit

Implement other translations.  Check out <a href="http://www.rinkworks.com/dialect/">http://www.rinkworks.com/dialect/</a> and <a href="https://www.cs.utexas.edu/users/jbc/home/chef.html">https://www.cs.utexas.edu/users/jbc/home/chef.html</a> for inspiration.

## Handin

### README

Create a file called <tt>README</tt> that contains

<ol>
<li>Your name and a description of the program</li>
<li>A listing of the files with a short one line description of the contents</li>
<li>Any known bugs or incomplete functions</li>
<li>Your estimated time from the start and the actual time taken</li>
<li>Your affirmation as to the honor code if you followed it</li>
</ol>

Now you should <tt>make clean</tt> to get rid of your executables and
handin your folder containing your source files, Makefile, and README through GitHub


<pre class="boxed">
% <tt>git add</tt>
% <tt>git commit</tt>
% <tt>git push</tt>
</pre>
Remember, if you have issues with commits due to odd merges, it is easier to start over, clone into a new directory, and copy the files over before commiting and pushing.

In addition, if you are working with a partner, you should do <tt>git pull</tt> periodically to pull their changes into your direction to avoid having to do a merge

## Grading

Here is what I am looking for in this assignment:

* A working Makefile with your program, all, and clean as targets
* Appropriately modular code
* Good comments
* Programs which run under <tt>valgrind</tt> without errors
* A statement about any valgrind errors which occur and that you could not
fix
* A README with the information above.  The listing of known bugs is
important.



<!-- Footer -->
<hr>
<footer>
Last Modified by Roberto Hoyle, November 2022. Based on material from Benjamin Kuperman and Nick Care
</footer>

