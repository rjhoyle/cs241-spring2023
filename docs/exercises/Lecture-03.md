---
layout: default
title: Lecture 5
classdate: 2020-09-08
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Create an account on [GitHub](https://github.com) using your Oberlin email
   address if you don't already have one. If you already have an account, you
   can add your Oberlin email address on the appropriate
   [settings page](https://github.com/settings/emails).
2. Log in to GitHub and then visit [this
   page](https://classroom.github.com/a/WkALqmQ8) and click the `Accept this
   assignment` button.
3. After a few seconds, the repository will be ready, click the
   `https://github.com/systems-programming/hw0-username` link that appears
   (it won't be literally `hw0-username`, it will have your GitHub
   username.
4. Log in to clyde.
5. Run the following commands, but substitute your name, email address, and
   editor of choice in the appropriate spots
   ```
   $ git config --global user.name 'YOUR NAME HERE'
   $ git config --global user.email 'YOUR.EMAIL@oberlin.edu'
   $ git config --global core.editor 'YOUR EDITOR'
   ```

## Task
1.  Under "Quick setup" on the GitHub page you opened in step 3 of the Setup,
    click the `HTTPS` button and copy the URL to the right of it. Run
    ```
    $ git clone https://github.com/somethingsomething...
    ```
    where you replace `https://github.com/somethingsomething/...` with the
    actual URL you copied.
2.  `cd` into the directory `git clone` just created. There are no files there
    yet, so open your editor of choice and create a `README` file. It's
    traditional to include information about what the project is, how to
    build/install it, and examples of usage. In this case, the project is
    called `HW0`. Mine contained these contents.
    ```
    HW0
    
    Information about what this does here.
    
    Usage examples
    
    Build instructions
    ```
3.  Use `$ git add README` to add `README` to the staging area.
4.  Use `$ git commit` to commit it. This will open the editor you configured
    above. Enter a commit message. The first line should be a short summary.
    Then add a blank line, and starting on the third line, you can give a more
    detailed explanation of what you're committing. Write your commit message,
    save the file, and exit the editor. If all has gone well, your file will be
    committed.
5.  Use `$ git push` to push the new commit to GitHub. You'll need to enter
    your GitHub username and password.
6.  Reload the GitHub page in your browser from step 1. You should see your
    `README` in the repository and GitHub will display the text in the page below
    the file listing.
7.  That README is okay, but it's kinda boring. It's just plain text. Let's
    replace it with one we can write in
    [Markdown](https://guides.github.com/features/mastering-markdown/). Use
    `git mv` to rename `README` to `README.md`.
8.  Edit `README.md`. You can read more about Markdown later. For now, use
    lines starting with `#` to make section headings.
    ``` markdown
    # HW0
    
    Information about what this does here.
    
    # Usage examples
    
    # Build instructions
    ```
9.  Run `$ git status` to see some information about the files in the repo. You
    should see under "Changes to be committed" that the file was renamed and
    under "Changes not staged for commit" that the file was edited.
10. To see what would be committed if you were to run `$ git commit` right
    now, run `$ git diff --cached`. This is the difference between what's in
    the repo and what has been staged by the `git mv` earlier.
11. To see the difference between what is in your working directory and what
    has been staged, run `$ git diff` (without the `--cached`). Lines in red
    (starting with `-`) will be deleted. Lines in green (starting with `+`)
    will be added.
12. Stage your changes by running `$ git add README.md`.
13. Run `$ git status`, `$ git diff --cached`, and `$ git diff` to see what
    changed from before the `git add`.
14. Commit the changes with an appropriate commit message.
15. Reload the GitHub page for the repository. You'll see nothing has changed.
    Why not? What step did we forget? Perform that step.
16. Reload the page again after performing the missing step.
17. Try creating and adding new files and directories. Commit them. Modify
    some files, commit your changes. View your changes using `$ git log`
18. Just for fun, read Steve Losh's [Git Koans](http://stevelosh.com/blog/2013/04/git-koans/).

## Make life easier
Entering your username and password every time you push or pull from GitHub is
_terrible_. You definitely don't want to have to do that every time. There are
two options to make life more pleasant for you.

The first is to use the "git credential helper" to securely store your
credentials. See this StackOverflow
[answer](https://stackoverflow.com/a/51505417) for some details.

The [second
method](https://help.github.com/en/articles/connecting-to-github-with-ssh) is
to give GitHub your SSH key. This is the method I usually use. Note that the
instructions for generating an SSH key say to use
```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
but I would suggest the following instead.
```
$ ssh-keygen -t ed25519 -C "your.email@oberlin.edu"
```
Note that the key pair you generate this way will be called `id_ed25519` and
`id_ed25519.pub` instead of `id_rsa` and `id_rsa.pub`. The only real advantage
here is that Ed25519 is much faster than RSA.

If you use the second method, the URI you want to copy for cloning will be
`git@github.com:somethingsomething/...` rather than
`https://github.com/somethingsomething/...`.
