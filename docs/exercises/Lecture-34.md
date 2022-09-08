---
layout: default
title: Lecture 34
classdate: 2020-05-06
---
# {{ page.title }} -- {{ page.classdate | date_to_string: "ordinal", "US" }}

## Setup
1. Log in to clyde.

## Task
1. Go to
   [https://github.com/systems-programming/hello](https://github.com/systems-programming/hello)
   and fork the repository.
2. Pick one of the changes below (or anything else you desire), and you're
   going to implement the change and create a pull request for it in
   subsequent steps.
   Change ideas:
   - Add a `Makefile` that will build the program.
   - Use `getenv(3)` to get the `USER` variable and use that to customize the
     message.
   - Exit with the `EXIT_SUCCESS` value defined in `stdlib.h` rather than just
     `0`.
   - Write a `goodbye` program.
   - Write some other simple program.
   - Add an image to the `README.md`
   - Add a `.travis.yml` file to make sure the program builds when changes are
     committed.
   - Make any other changes you desire.
3. Once you have a change in mind, pick a short name that is reflective of
   that change and create and checkout a new branch.
   ```
   $ git checkout -b my-feature
   ```
   (Pick a better name than `my-feature`!)
4. Implement your change and commit your change.
5. Push your change to Github.
   ```
   $ git push -u origin my-feature
   ```
6. You should get a message about visiting a URL to submit a pull request. Go
   to the URL and submit the pull request.
7. Modify the pull request by making another commit and pushing again. (Assume
   you got asked to make a change to what you committed.)
8. Squash your changes using
   ```
   $ git rebase -i master
   ```
   This should open your editor. You'll want to `pick` the top-most commit and
   `squash` the others.
   Save the file and exit the editor.
   Push them to Github.
9. Check out your pull request (and others) by going to
   [https://github.com/systems-programming/hello/pulls](https://github.com/systems-programming/hello/pulls).
   Feel free to leave comments on other people's pull requests.
10. If your PR is accepted, fetch from `upstream` (you'll need to add that
    first) and delete your branches.
    ```
    $ git remote add upstream https://github.com/systems-programming/hello.git
    $ git fetch upstream master:master
    $ git branch -d my-feature # you may need -D instead of -d
    $ git push origin -d my-feature # or delete from the web interface
    ```
    If your PR needs to be modified to handle upstream changes, fetch from
    upstream, rebase, fix any conflicts, and push.
    ```
    $ git remote add upstream https://github.com/systems-programming/hello.git
    $ git fetch upstream master:master
    $ git rebase master
    $ git push
    ```
    If there were conflicts after the `rebase`, you'll need to fix them, run
    `$ git add` on the file with conflicts and then `$ git rebase --continue`.
