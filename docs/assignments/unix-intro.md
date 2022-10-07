---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /assignments/hw1
title: hw1

---

# CSCI 241 - Homework 1 - Unix Introduction
Due by 11:59.59pm Tuesday, September 20



For this assignment you will be:

* Learning how to use the editor to create scipts
* Learn how to chain Unix commands with pipes
* Learn how to use Unix commands for simple data analysis
* Learning how to use github for submission

## Part 0 - git

We're going to start things off nice and simple.  I'll recommend you create a
<tt>cs241</tt> directory in your CS account and a <tt>hw1</tt> directory inside
that.



First off, go to <a href="https://classroom.github.com/a/ZXSX2GEJ">https://classroom.github.com/a/ZXSX2GEJ</a> and create a repository in the CS 241 organization.


Click on the link and head towards the created repository.  It will be called <b>hw1-username</b>



  Go back to your CS account to the cs241 directory above, and clone the repository into this account.  Refer to the github tutorials for how to do this step, or to Tuesday's class. 



Download the file <a href="starters/hw1.zip">hw1.zip</a> into the directory.  You can download it to your local machine and then copy it over with scp, CyberDuck, or some other SFTP client, or you can type:

<pre>
wget https://occs.cs.oberlin.edu/~rhoyle/22f-cs241/assignments/starters/hw1.zip
</pre>

to download it from the Internet.

Alternatively, we can take advantage of the fact that occs and clyde share the same filesystem, and we can copy it from my pub directory, where I keep all course-related files:

<pre>
cp ~rhoyle/pub/cs241/hw1/hw1.zip .
</pre>

and that will copy "unix-intro-files.zip" to the currect directory.  Note how we used "." in the command above to indicate the curent directory.

However you do it, once you have the file where you want it, you can unzip it with

<pre>
unzip hw1.zip
</pre>

This will add 5 files, 3 of which are empty, to your current directory:

* README
* diskhogger.sh
* data_file_analysis
* grade.sh
* modified.sh

You will add one more file in Part 3, below.  These 6 files are the only ones that you need to turn in with your submission.

The grade.sh file will normally run some tests on your code to see if it works.  Since these are shell scripts, it's hard to do this without giving away the solution, so for now it only checks to see if the files exist with the right names.  We will encounter more sophisticated grader files later.

## Part 1 - Diskhogger

Give me a one-line set of pipes that will print the top 5 largest files in the current directory in a human-readable format (i.e. use abbreviations such as "K" for kilobyte and "M" for megabyte, instead of listing the raw numbers).

The commands <tt>ls</tt>, <tt>cut</tt>, <tt>tr</tt>, <tt>head</tt>, and <tt>tail</tt> may be useful.  Check out their man pages.

Insert the command into the marked place on diskhogger.sh, and you'll be able to run it as:

./diskhogger.sh

and it should output the top 5 files with their size.

A sample run is:
<pre>
hoyle@occs:~/pub/cs241/hw1$ ./diskhogger.sh
11K zombie
11K pipe-eof
11K pipe
10K dup-exec
9.9K alarm-handle
hoyle@occs:~/pub/cs241/hw1$
</pre>

<h2>Part 2 - Modified</h2>

Give me a list of all the .c files that have been modified in ~rhoyle/pub/cs241 in the past 90 days.

You may find the commands <tt>find</tt> and <tt>xargs</tt> useful.

Like in part 1, enter the command in the appropriate place in the modified.sh shell script.

A sample run is below.  Your output may differ based on the curent date.

<pre>
hoyle@occs:~/pub/cs241/hw1$ ./modified.sh 
-rw-r--r-- 1 rhoyle rhoyle 1653 Dec  7 11:59 /usr/users/noquota/faculty/rhoyle/pub/cs241/structs/struct_pointer.c
-rw-r--r-- 1 rhoyle rhoyle 534 Dec  7 11:33 /usr/users/noquota/faculty/rhoyle/pub/cs241/structs/struct_size.c
-rw-r--r-- 1 rhoyle rhoyle 897 Dec  7 10:08 /usr/users/noquota/faculty/rhoyle/pub/cs241/structs/union_demo.c
-rw-r--r-- 1 rhoyle rhoyle 1572 Dec 14 11:20 /usr/users/noquota/faculty/rhoyle/pub/cs241/structs/linked_list.c
-rw-rw-r-- 1 rhoyle rhoyle 536 Jan 12 11:24 /usr/users/noquota/faculty/rhoyle/pub/cs241/bytes/printbit.c
-rw-r--r-- 1 rhoyle rhoyle 1071 Dec 21 11:30 /usr/users/noquota/faculty/rhoyle/pub/cs241/week10/varargs.c
-rw-r--r-- 1 rhoyle rhoyle 702 Dec  2 11:55 /usr/users/noquota/faculty/rhoyle/pub/cs241/mdarrays/square_array.c
-rw-r--r-- 1 rhoyle rhoyle 1044 Dec  2 12:14 /usr/users/noquota/faculty/rhoyle/pub/cs241/sort/sort.c
-rw-r--r-- 1 rhoyle rhoyle 186 Dec  2 11:30 /usr/users/noquota/faculty/rhoyle/pub/cs241/c-intro/hello.c
-rw-r--r-- 1 rhoyle rhoyle 745 Dec 16 11:43 /usr/users/noquota/faculty/rhoyle/pub/cs241/io/scanf.c
-rw-rw-r-- 1 rhoyle rhoyle 312 Dec 14 10:50 /usr/users/noquota/faculty/rhoyle/pub/cs241/io/bigfile2.c
-rw-r--r-- 1 rhoyle rhoyle 358 Dec 14 11:53 /usr/users/noquota/faculty/rhoyle/pub/cs241/io/fopen.c
-rw-r--r-- 1 rhoyle rhoyle 539 Dec 14 12:37 /usr/users/noquota/faculty/rhoyle/pub/cs241/io/bigfile.c
-rw-rw-r-- 1 rhoyle rhoyle 1124 Dec  2 11:26 /usr/users/noquota/faculty/rhoyle/pub/cs241/strings/strdup.c
-rw-rw-r-- 1 rhoyle rhoyle 75 Dec  7 11:08 /usr/users/noquota/faculty/rhoyle/pub/cs241/gdb/crash.c
hoyle@occs:~/pub/cs241/hw1$ 
</pre>

## Part 3 - Top

Give me a command to list the process that is using the most physical memory.  You may find the commands *top* and *head* useful.
 

## Part 4 - README

Finally, I want you to create a file called <tt>README</tt> (note all caps
and no file extension) which contains the following sections:

* Your name and the date
* A list of the programs with a short one-line description of each
* An estimate of how long you spent on each Part
* A list of all problems, warnings, or errors.  Note that for full marks, it is expected that you will have corrected all of these things.
* The honor code statement:
<q class="honor">I have adhered to the Honor Code in this assignment</q>
		

## Part 5 - Submission

Run git add, commit, and push to send your changes to GitHub.  You should check via the GitHub web interface to make sure that your files have been submitted properly.

  
## Extra Credit - COVID Data Analysis

I often find myself using shell tools to answer questions about a data file
that I'm working on.

The file that we will work on can be downloaded from <a href="https://coronavirus.ohio.gov/static/dashboards/COVIDSummaryData.csv">https://coronavirus.ohio.gov/static/dashboards/COVIDSummaryData.csv</a>.  You should use <tt>wget</tt> or <tt>curl</tt> to download it.




Answer the following questions in your <tt>README</tt> file (and give the
commands used to find the answer):

* How many cases are in Lorain county?
* How many cases are there for Males vs. Females?
* Which county has the most cases?
* Which county has the most cases for 20-29 year olds?

Potentially useful commands to look at include <tt>cut</tt>, <tt>sort</tt>,
and <tt>uniq</tt>.

Please include the commands you used to generate your answers and the answers that you get in the file data_file_analysis.  Include the downloaded data file with your submission as the file will change over time.
