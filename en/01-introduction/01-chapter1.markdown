# Version Control #

## Preface ##

This is a modified version of Pro Git.  The original version, by Scott Chacon, is available from http://git-scm.com/book.

This version, by Allen B. Downey, is available from https://github.com/AllenDowney/amgit.

Both books are under the Creative Commons Attribution Non Commercial Share Alike 3.0 license, which you can read at
http://creativecommons.org/licenses/by-nc-sa/3.0/

My reasons for making a modified version are:

1.	I am frustrated with most existing books about Git because they talk too much about how Git is implemented, and not enough about the mental models users need to use it.
2.	Most existing books are not well suited for beginners.  They tend to use a lot of vocabulary without providing careful definitions.
3.	And they waste ink comparing Git to other version control systems that beginners have not used. 
4.	Most existing books are not organized around the use cases beginners are most likely to see.
5.	On the other hand, the existing books contain lots of useful material that I did not want to re-create.

So modifying an existing book is the way to go.  I want to thank Scott Chacon for making his book available under a license that makes this possible (and Creative Commons for providing the license).

## What's the problem? ##

Suppose you are writing a paper.  Maybe you keep the files related to the paper in a folder on a hard drive in your computer.  This folder probably contains the text of the paper, data files, programs that process the data, figures and tables generated, and maybe files that contain your notes and related work.  If there are a large number of files, you might have them organized in sub-folders.

As you work on the project over a few weeks, or months, what are some of the problems you run into?

*	While you are processing data, you might discover errors in the data files.  Each time you find an error, you edit the data file and save the changes.  But after a while you might lose track of what the original data looked like, and what you edited.  If you are planning to publish a scientific paper, you might have a hard time defending the integrity of the data.
*	As you develop the programs that process the data, you might add a new feature and then discover that one of the old features is broken.  If you did not save the previous version of the program, you might have a hard time figuring out what you changed and why it is not working.
*	You might have copies of your files on a computer at work, a laptop, and a computer at home.  When you make changes, you have to propagate the changes from one copy to the others, and it is easy to lose track of which changes are in which locations.
*	If you are collaborating with other people, you have to make sure that all collaborators have all the files they need, and you have to keep track of who is working on which file.  If two people edit the same file at the same time, one of the changes might get lost, or they might clash with each other.  And it might be hard to keep track of who did what.
*	You probably want different people to have different access to your files.  Collaborators should be able to read and edit.  You might want some other people to read, but not edit.  And you might want to prevent others from reading, or you might want to make it easy for others to discover your work.
*	Finally, there is always the possibility that your hard drive crashes, or your computer is damaged or stolen, and you lose all your data.

Version control systems (VCS) addresses address all of these problems.  A VCS provides tools that allow you to store your files in a named repository.  A repository is a collection of files, similar to a folder on a hard drive, but with additional capabilities.   In particular, 

*	A repository keeps track of the history of its files, so it is easy to see who changed what, and when, and why.
*	It is easy to see older versions of the files, and compare any two versions, and if you mess something up, you can go back to an older version.
*	It is easy to make copies of a repository, so everyone collaborating on a project can have their own copy.
*	You can keep a copy at a remote location, on a professionally-maintained server, so there is very little chance of losing your data.
*	And when people collaborate on a project, the repository helps them coordinate their work.  They can make and test changes independently, and then move changes from one copy to another in an organized way.
*	Version controls systems provide servers that give you control over who can read and write your files.  You can make them as secure, or as public, as you want.
*	Some version control systems provide additional services that are not strictly part of VCS, but that help projects run smoothly; for example, a wiki for documentation, a code review system, or an issue tracker (which is like a glorified to-do list).   

There are several popular version control systems.  I won't go into their pros and cons.  I am only going to tell you about one of them, Git.  To show you how to use it, I will walk you through several common use cases.  But before you can use Git, you have to install it.





## Installing Git ##


### Installing on Linux ###

To see if you already have git, open a terminal and type:

	$ git help
	
If you have it, you will see a help message and you can skip to the next section.  Otherwise your system might tell you how to install it.

If you’re on Fedora, you can use yum:

	$ yum install git-core git-gui

If you’re on a Debian-based distribution like Ubuntu, try apt-get:

	$ apt-get install git-core git-gui
	

### Installing on Mac ###

There are two ways to install Git on a Mac. The easiest is to use the graphical Git installer, which you can download from the Google Code page (see Figure 1-7):

	http://code.google.com/p/git-osx-installer

Insert 18333fig0107.png
Figure 1-7. Git OS X installer.

The other major way is to install Git via Homebrew (`http://brew.sh/`). If you have Homebrew installed, install Git via

	$ brew install git

### Installing on Windows ###

The msysGit project has one of the easier installation procedures. Simply download the installer exe file from GitHub and run it:

	http://msysgit.github.com/

After it’s installed, you have both a command-line version (including an SSH client that will come in handy later) and the standard GUI.

Note on Windows usage: you should use Git with the provided msysGit shell (Unix style), it allows to use the complex lines of command given in this book. If you need, for some reason, to use the native Windows shell / command line console, you have to use double quotes instead of simple quotes (for parameters with spaces in them) and you must quote the parameters ending with the circumflex accent (^) if they are last on the line, as it is a continuation symbol in Windows.

### Installing from Source ###

If any of those methods fail, you can install Git from source, but it is not as easy.

To compile Git, you need to have the following libraries that Git depends on: curl, zlib, openssl, expat, and libiconv. If you’re on a system that has yum (such as Fedora) or apt-get (such as a Debian based system), you can use one of these commands to install all of the dependencies:

	$ yum install curl-devel expat-devel gettext-devel \
	  openssl-devel zlib-devel

	$ apt-get install libcurl4-gnutls-dev libexpat1-dev gettext \
	  libz-dev libssl-dev

When you have all the necessary dependencies, you can get the latest version from the Git web site:

	http://git-scm.com/download

Then, compile and install:

	$ tar -zxf git-1.7.2.2.tar.gz
	$ cd git-1.7.2.2
	$ make prefix=/usr/local all
	$ sudo make prefix=/usr/local install

Once you have Git installed, you can use Git to get updated versions of Git.  But we'll git to that later.  I mean, "Get to that."


## First-Time Git Setup ##

Now that you have Git on your system, you’ll want to do a few things to customize your Git environment. You should have to do these things only once; they’ll stick around between upgrades. You can also change them at any time by running through the commands again.

Git comes with a tool called git config that lets you get and set configuration variables that control all aspects of how Git looks and operates. These variables can be stored in three different places:

*	`/etc/gitconfig` file: Contains values for every user on the system and all their repositories. If you pass the option` --system` to `git config`, it reads and writes from this file specifically.
*	`~/.gitconfig` file: Specific to your user. You can make Git read and write to this file specifically by passing the `--global` option.
*	config file in the git directory (that is, `.git/config`) of whatever repository you’re currently using: Specific to that single repository. Each level overrides values in the previous level, so values in `.git/config` trump those in `/etc/gitconfig`.

On Windows systems, Git looks for the `.gitconfig` file in the `$HOME` directory (`%USERPROFILE%` in Windows’ environment), which is `C:\Documents and Settings\$USER` or `C:\Users\$USER` for most people, depending on version (`$USER` is `%USERNAME%` in Windows’ environment). It also still looks for /etc/gitconfig, although it’s relative to the MSys root, which is wherever you decide to install Git on your Windows system when you run the installer.

### Your Identity ###

The first thing you should do when you install Git is to set your user name and e-mail address. This is important because every Git commit uses this information, and it’s immutably baked into the commits you pass around:

	$ git config --global user.name "John Doe"
	$ git config --global user.email johndoe@example.com

Again, you need to do this only once if you pass the `--global` option, because then Git will always use that information for anything you do on that system. If you want to override this with a different name or e-mail address for specific projects, you can run the command without the `--global` option when you’re in that project.

### Your Editor ###

Now that your identity is set up, you can configure the default text editor that will be used when Git needs you to type in a message. By default, Git uses your system’s default editor, which is generally Vi or Vim. If you want to use a different text editor, such as Emacs, you can do the following:

	$ git config --global core.editor emacs

### Your Diff Tool ###

Another useful option you may want to configure is the default diff tool to use to resolve merge conflicts. Say you want to use vimdiff:

	$ git config --global merge.tool tkdiff

Git accepts kdiff3, tkdiff, meld, xxdiff, emerge, vimdiff, gvimdiff, ecmerge, and opendiff as valid merge tools. You can also set up a custom tool; see Chapter 7 for more information about doing that.

### Checking Your Settings ###

If you want to check your settings, you can use the `git config --list` command to list all the settings Git can find at that point:

	$ git config --list
	user.name=Scott Chacon
	user.email=schacon@gmail.com
	color.status=auto
	color.branch=auto
	color.interactive=auto
	color.diff=auto
	...

You may see keys more than once, because Git reads the same key from different files (`/etc/gitconfig` and `~/.gitconfig`, for example). In this case, Git uses the last value for each unique key it sees.

You can also check what Git thinks a specific key’s value is by typing `git config {key}`:

	$ git config user.name
	Scott Chacon

## Getting Help ##

If you ever need help while using Git, there are three ways to get the manual page (manpage) help for any of the Git commands:

	$ git help <verb>
	$ git <verb> --help
	$ man git-<verb>

For example, you can get the manpage help for the config command by running

	$ git help config

These commands are nice because you can access them anywhere, even offline.
If the manpages and this book aren’t enough and you need in-person help, you can try the `#git` or `#github` channel on the Freenode IRC server (irc.freenode.net). These channels are regularly filled with hundreds of people who are all very knowledgeable about Git and are often willing to help.

