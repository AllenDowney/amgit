# The first use case #

## Outline ##

In each chapter I walk you through a use case for Git.  In this chapter
we make a copy of an existing repository, make a change, and add the
change back to the repository.  Here are the steps.

1.   Create an account on GitHub.
2.   Find the repository I created for this exercise.
3.   "Fork" the repository, making a copy on GitHub that belongs to you.
4.   "Clone" the repository, making a copy on your computer.
5.   Modify one of the files in the repository.
6.   Commit the change to your local repository.
7.   Push the change back to your repository on GitHub.
8.   Ask me to pull the change into my repository.

This might not be the first use case a beginner encounters, but
(for other reasons) it is a good place to start.

Note: from here on I will sometimes use "repo" as shorthand for "repository".
I'm not making that up; it is a pretty common term.


## Sign up for Github ##

GitHub is a web-based hosting service for Git users.  In general a hosting
service provides storage space on remote servers, network access, and
tools and applications for interacting with stored data.  GitHub
provides storage for Git repositories and tools for interacting with them.

There are other hosting services for Git, but GitHub is one of the
most popular.  It is so popular that people sometimes say "GitHub" when
they mean "Git", so just to be clear:

*    Git is an application that runs on your computer
and helps you manage repositories.
*   You can use Git to manage repos stored on your own computer or
on any computer configured as a Git server.
*  Anybody can set up and run a Git server.  A company that runs
Git servers professionally is a Git hosting service.
*   GitHub is one of many Git hosting services.

Ok, go to http://github.com.  If you already
have an account, log in.  Otherwise, you will have to create one.

You can choose any available username you like, but there are a few
things you might want to think about:

1.   Working on GitHub involves interacting with other people.  They will
see your username, so choose wisely.
2.   Some people, like `AllenDowney`, use their full names, but the most
common schema seems to be one-word lower-case usernames.  For example,
Scott Chacon is `schacon`.
3.    If you want to be anonymous, you can choose a username unrelated
to your real name; however,
4.    Many software engineers use GitHub as part of their professional
portfolio.  If a potential employer wants to check out your skills, they
might look at your GitHub repositories.

It is probably a good idea to think of everything you do on GitHub
as part of your public professional reputation.


## Finding a project ##

When you log in, you will see the GitHub home page, which includes
links to tutorials and updates on your repositories.

You can use the search box in the upper left to find people and projects.
Type "progit" in the search box and hit enter.  You should see a list
of projects with "progit" in the name; the first hit is probably
the original version of this book.

If you search for "AllenDowney", GitHub reports, "We couldn't find any
repositories matching 'AllenDowney'".  That's because it was looking
for repositories.  If you hit "Users" in the
left column, you should find my profile.

If you search for "Blair Walden Project," you should find a project
named `blair-walden-project`, which I created for use with this exercise.
If you have any trouble finding it, the direct link is
https://github.com/AllenDowney/blair-walden-project.

The project home page displays the name and description of the
project, a list of files in the repo, and the contents of README.md,
which contains more information about the project.  As it explains,
this repo "is the home of a collaborative writing project, a mash-up
of _Walden_ and _The Blair Witch Project_, in which it turns out that
Thoreau wasn't alone in the woods after all."

I initialized this repo with a copy of Henry David Thoreau's _Walden_
from Project Gutenberg.  We will use this repo to make a modified
version of _Walden_ that includes supernatural elements in the style
of _The Blair Witch Project_.


## Forking ##

The full name of the project is `AllenDowney/blair-walden-project`, which
is a hint that I am the owner of this repo.  You can read the contents,
and you can even make a copy on your computer, but you cannot modify
the repo (unless I add you as a collaborator).

However, you can "fork" the repo, which means that you make a copy
on GitHub that belongs to you.  In the upper-right corner of the project
home page, press "Fork".

You will be asked whether you want the fork to be public or private.
It's up to you, but I suggest you make it public.

[Add description of the result here.]

When you fork a repository, you get a copy of the files, and also
a copy of the history.  But after the fork, the two repositories are
distinct: changes you make in your repo do not affect mine, and
the other way around.


## Cloning ##

To get a copy of the repository onto your computer, you "clone" it.
Like forking, cloning creates a copy of the repo, including its
history.  The difference is that when you fork someone else's
repository, the new fork belongs to you.  When you clone a repo,
the clone has the same owner as the original.

To clone a repo from GitHub, you need to know its URL.  Go to the
project home page (of your fork, not mine) and look in the lower
right.  You should see a text box labelled "HTTPS clone URL".
Copy the URL in this box or click the icon to copy it to your
clipboard.

Then open a terminal, move to a directory where you want to store
your local copy of the repo, and type:

     $ git clone <URL>

where `<URL>` is the URL you copied from the project page.  It should
look like this

     $ git clone https://github.com/<yourusername>/blair-walden-project

If you made your fork private, you will be prompted for your GitHub
username and password.  If it's public, you can clone it
without authenticating.

The clone command generates a few lines of output with details,
probably something like this:

    Cloning into 'blair-walden-project'...
    remote: Counting objects: 12, done.
    remote: Compressing objects: 100% (9/9), done.
    remote: Total 12 (delta 2), reused 6 (delta 1)
    Unpacking objects: 100% (12/12), done.

But the numbers will probably be different.

List the contents of the current folder, you should see a new
folder with the same name as the project.  

You now have two repos: the remote copy on GitHub and the local
copy on your computer.


## What's in a repo? ##

`cd` into the new folder and 
list the contents; you should see two files:

* 205-0.txt, which contains the text of "The Blair Walden Project."
* README.md, which contains the project description, and

These are the "working files" in the repo, which means these are the
files you will modify.

This folder also contains a hidden sub-folder named `.git`.  In UNIX
you can see the contents of this folder by typing

    $ ls -a .git

Feel free to look around, but you don't really need to know what's in there,
and you should not modify any of the files in `.git` directly.

Abstractly, a repo has three parts:

1.  The working files,
2.  The object store, which contains the history of the repo, and
3.  The index, which keeps track of changes you have made.

The object store and the index are stored in `.git`.

Note: when I say "abstractly," that means I lying.  Or, more
generously, I am giving you a simplified version of reality that has
enough information to let you get work done without dealing with
unnecessary details.

When you clone a repo, you get a copy of the working files, a copy
of the object store, and a fresh new index that doesn't have any
changes in it.


## The log ##

Git records every change in the object store.  To see some of the
history of the repo, type:

    $ git log

You should see something like this:

    commit be9a05aff259387cb42c9e8348c803404d1f73b9
    Author: Allen Downey <downey@allendowney.com>
    Date:   Fri Jun 28 15:15:41 2013 -0400

    Adding a new header, fake preface, and tagline

    commit 511af78111f4ff0170b41d20983c34b75ca12435
    Author: Allen Downey <downey@allendowney.com>
    Date:   Fri Jun 28 14:33:49 2013 -0400

    Adding Walden

The log is a list of "commits" in reverse chronological order (sort of).
A commit is a change or set of changes in the working
files.  Each log entry includes:

1. The commit ID, which is a unique sequence of hexadecimal digits used
   to identify the commit,
2. The author of the commit,
3. The time and date, and
4. A description of the commit provided by the author explaining
what changed and, ideally, why.

To see more of the log, press SPACE.  When you want to exit, press `q`.


## Status ##

Type

    $ git status

If you have not modified any of the working files, you should get a
message like this:

    # On branch master
    nothing to commit, working directory clean

The working directory is "clean" if there are no changes in the local
repo that are not also in the remote repo.


## Get creative ##

The next step is to modify one of the working files.  Use the
editor of your choice to open `205-0.txt`.  Search for the word
"Preface" and read the short preface I added to this fake version
of the book.

_The Blair Walden Project_ is a collaborative writing exercise, so
you might want to put some thought into your contribution.

1.  The premise of the mashup is that Henry David Thoreau moves to
a cabin in the woods in 1845 (which is true) and then
2.  Disappears (which is not true), and
3.  A year later his journal is found (also not true), and
4.  This book is supposed to be the found journal.

So the plan is that we are going to take the actual text of _Walden_
and add creepy scary parts, similar to the events in the movie
_The Blair Witch Project_.  Got it?

Here are some tips:

*    The "journal" should contain hints about what happened, but never
be too explicit.
*    In the first half of the book, keep it subtle.  We can get more
overt as we go along.
*     You can jump in anywhere, but it might be best to find a "hook,"
or a place in the original text where it makes sense to add something
creepy.
*	If you can imitate Thoreau's writing style, that's great.  If not,
at least try to avoid jarring changes in style and obvious
anachronisms.
*    If English is not your first language, you can still participate
in this exercise.  Just do your best!
*  You can make your contribution as long or as short as you like.

When you are done, you will have the option to make a "pull request";
that is, you can ask me to include your change as part of my original
version.  I encourage you to exercise this option.  I will generally
accept all requests unless the contents are obviously inappropriate.


## Status again ##

Type

    $ git status

If you changed `205-0.txt`, you should get a
message like this:

    # On branch master
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #	modified:   205-0.txt
    #
    no changes added to commit (use "git add" and/or "git commit -a")

Git lists the files that are modified and provides some suggestions
about what you might want to do next.

This message includes some vocabulary I have not explained yet, but
be patient; we will get there soon.


## Commit the change ##

When you modify a working file, Git doesn't record the change
until you tell it to.  When you have finished adding a new creepy
part to the book, type

    $ git commit -am "Adding a new creepy part."

"Commit" means that you are copying the change into the Object store,
making it a permanent part of the repo history.

The flags, `a` and `m`, modify the behavior of commit.

* `a` stands for "all" and means that you want to commit all changes
since the last commit (or since the repo was cloned).

* `m` stands for "message" and means that you are providing the
commit message on the command line.

If you don't use `m`, Git opens an editor where you can type the
commit message.  The purpose of the commit message is to document
the change; the message you provide becomes part of the log.

"Commit" is both a verb and a noun.  When you commit, you copy a
set of changes into the Object store, which creates a new commit.

If you run `git log` again, the top entry should be the new commit
you just created.


## Push ##

The next step is to copy the new commit from the local repo back
to your repo on GitHub.  Copying a commit from a local copy to a
remote repo is called a "push", and the command is:

    $ git push <DEST> <SOURCE>

where `<DEST>` is the remote repo you are pushing to, and `<SOURCE>`
is the source of the change.

For `<DEST>` you can use the same URL you cloned, but it is more
common to use `origin`, which automatically refers to the remote
repo from which the local repo was cloned.

Usually `<SOURCE>` is the name of a branch, but since we haven't talked
about branches yet, I won't explain in detail.
In the simple case where there is only one branch, we can use
`master` which refers to the commit you just created.

So, to push the change to your repo on GitHub, type:

    $ git push origin master

This is the most common form of the `push` command, and the only
one we will use for a while.

For many projects, this would be the end of one work cycle.  The
most common Git work flow looks something like this:

1.   Clone a repo.
2.   Modify working files.
3.   Commit a set of changes.
4.   Push the changes back to `origin`.
5.   Go to step 2.

However, if you want your contribution to _The Blair Walden Project_
included in my repo, there is one more step.


## Pull request ##

You could try to push the change from your local copy to my repo
on GitHub, but it would not work because you do not have permission
to modify my repo.  That's why you created a fork.

However, now that you have pushed the change into _your_ repo, you can
ask me to "pull" it into my repo.  I will be able to pull your change,
because I can read your repo and write mine.

To make a pull request, go back to the home page for your repo
on GitHub.  

1. Along the right-hand side, click "Pull requests".
2. On the next page, near the top-left corner, click "New pull request".
3. GitHub provides default choices for the source and destination that 
are probably right.  Click "Click to create a pull request for this comparison".
4. Fill in a title for the pull request, something like "Contributing a new
creepy part."
5. Fill in a comment explaining why you think I should accept your pull
request (this part is optional).
6. Click "Send pull request".

You will be notified when I accept your pull request, and then you can
check back to see that your change appears in my repo.
As I said, I will generally
accept all pull requests unless the contents are obviously inappropriate.

One small bit of legalese: by making a pull request, you are making
a contribution to my project; that is, you are giving me a license
to include your original material in my collaborative work.


## Destroy the local copy ##

If you have committed your change and pushed it to your remote
repo, type

    $ git status

You should get

    # On branch master
    nothing to commit, working directory clean

Which means that there are no changes in the local repo that
are not on the remote repo, too.  In that case, you can delete
the local repo with no loss of data.

One of the nice things about Git is that it keeps your data safe.
If you keep a copy of your repo on GitHub or another professionally
maintained hosting service, the chances of losing data are very
small.  And if that's not good enough, you can keep copies on several
different servers.

Also, it is hard to mess up a remote repository.  The only way to
access the files on GitHub is through Git or the web, and
these interfaces generally prevent you from messing things up.

On the other hand, it is relatively easy to mess up a local copy.
For example, you might accidentally modify the files in `.git`, or
delete the entire folder.

If something like that happens, or if your local copy is not working
and you don't know why, you have a few options.

If there were no changes in the local copy that you want to save,
you can just delete the local copy and clone a new one.  Local
copies are disposable!

Otherwise:

1.  Move the local copy into a temporary folder, or just change the name. 
2.  Clone a new local copy to replace the one you just moved/renamed.
3.  Use `git status` to identify the modified files you want to keep
and copy them from the old repo to the new.
4.  Use `git commit` to commit the changes, and `git push` to push
them up to the remote repo.
5.  Once you are sure you have recovered the changes you care about,
delete the old local copy.




