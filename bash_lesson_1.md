# Introducing the Shell

> Questions:
> > - "What is a command shell and why would I use one?"
> > - "How can I move around on my computer?"
> > - "How can I see what files and directories I have?"
> > - "How can I specify the location of a file or directory on my computer?"

> Objectives:
> > - "Describe key reasons for learning shell."
> > - "Navigate your file system using the command line."
> > - "Access and read help files for `bash` programs and use help files to identify useful command options."
> > - "Demonstrate the use of tab completion, and explain its advantages."

> Keypoints:
> > - "The shell gives you the ability to work more efficiently by using keyboard commands rather than a GUI."
> > - "Useful commands for navigating your file system include: `ls`, `pwd`, and `cd`."
> > - "Most commands take options (flags) which begin with a `-`."
> > - "Tab completion can reduce errors from mistyping and make work more efficient in the shell."

## What is a shell and why should I care?

A *shell* is a computer program that presents a command line interface which allows you to control your computer using commands entered with a keyboard instead of controlling graphical user interfaces (GUIs) with a mouse/keyboard combination.

There are many reasons to learn about the shell:
* Many bioinformatics tools can only be used through a command line interface, or have extra capabilities in the command line version that are not available in the GUI.
* The shell makes your work more reproducible. When you carry out your work in the command-line (rather than a GUI), your computer keeps a record of every step that you've carried out, which you can use to re-do your work when you need to. It also gives you a way to communicate unambiguously what you've done, so that others can check your work or apply your process to new data.
* Many bioinformatic tasks require large amounts of computing power and can't realistically be run on your own machine. These tasks are best performed using remote computers or cloud computing, which can only be accessed through a shell.

## Navigating your file system

![](/img/filesystem.svg)

Several commands are frequently used to create, inspect, rename, and delete files and directories.

```
$
```

> The dollar sign is a **prompt**, which shows us that the shell is waiting for input; your shell may use a different character as a prompt and may add information before the prompt. When typing commands, either from these lessons or from other sources, do not type the prompt, only the commands that follow it.

Let's find out where we are by running a command called `pwd` (which stands for "**print working directory**"). At any moment, our current working directory is our current default directory, i.e., the directory that the computer assumes we want to run commands in unless we explicitly specify something else.

```
$ pwd
```

Here, the computer's response is `/home/sateeshp`, which is the top level directory within our cloud system:

Lets import data required for this tutorial.

```
$ cp /opt/dc_workshop .
```

Let's look at how our file system is organized. At the top is our home directory, which holds all the sub-directories and files. Inside that directory there is currently one other directory:

```
dc_workshop
```

We'll be working with this sub-directory, and creating new sub-directories, throughout this workshop.

The command to change locations in our file system is `cd` followed by a directory name to change our working directory. `cd` stands for "**change directory**".

Let's say we want to navigate to the `dc_workshop` directory we saw above.  We can use the following command to get there:

```
$ cd dc_workshop
```

We can see files and sub-directories are in this directory by running `ls`, which stands for "**listing**":

```
$ ls
```

```
sra_metadata  untrimmed_fastq
```

`ls` prints the names of the files and directories in the current directory in alphabetical order, arranged neatly into columns. We can make its output more comprehensible by using the **flag** `-F`, which tells `ls` to add a trailing `/` to the names of directories:

```
$ ls -F
```

```
sra_metadata/  untrimmed_fastq/
```

Anything with a "/" after it is a directory. Things with a "\*" after them are programs. If there are no decorations, it's a file.

`ls` has lots of other options. To find out what they are, we can type:

```
$ man ls
```

Some manual files are very long. You can scroll through the file using your keyboard's down arrow or use the <kbd>Space</kbd> key to go forward one page and the <kbd>b</kbd> key to go backwards one page. When you are done reading, hit <kbd>q</kbd> to quit.

> **Challenge**
> Use the `-l` option for the `ls` command to display more information for each item in the directory. What is one piece of additional information this long format gives you that you don't see with the bare `ls` command?
>
> > **Solution**
> > ```
> > $ ls -l
> > ```
> > ```
> > total 8
> > drwxr-x--- 2 root root 4096 Mar 30 20:23 sra_metadata
> > drwxr-x--- 2 root root 4096 Mar 30 20:23 untrimmed_fastq
> > ```
> >
> > The additional information given includes the name of the owner of the file, when the file was last modified, and whether the current user has permission to read and write to the file.

> No one can possibly learn all of these arguments, that's why the manual page is for. You can (and should) refer to the manual page or other help files as needed.

Let's go into the `untrimmed_fastq` directory and see what is in there.

```
$ cd untrimmed_fastq
$ ls -F
```

```
SRR097977.fastq  SRR098026.fastq
```

This directory contains two files with `.fastq` extensions. FASTQ is a format for storing information about sequencing reads and their quality. We will be learning more about FASTQ files in a later lesson.

### Shortcut: <kbd>Tab</kbd> Completion

Typing out file or directory names can waste a lot of time and it's easy to make typing mistakes. Instead we can use tab complete as a shortcut. When you start typing out the name of a directory or file, then hit the <kbd>Tab</kbd> key, the shell will try to fill in the rest of the directory or file name.

Return to your home directory:

```
$ cd
```

then enter: cd dc<kbd>Tab</kbd>

The shell will fill in the rest of the directory name for `dc_workshop`.

Now change directories to `untrimmed_fastq` in `dc_workshop`

```
$ cd dc_workshop
$ cd untrimmed_fastq
```

Using <kbd>Tab</kbd> complete can be very helpful. However, it will only auto-complete a file or directory name if you've typed enough characters to provide a unique identifier for the file or directory you are trying to access.

If we navigate back to our `untrimmed_fastq` directory and try to access one of our sample files:

```
$ cd
$ cd dc_workshop
$ cd untrimmed_fastq
$ ls SR<kbd>Tab</kbd>
```

The shell auto-completes your command to `SRR09`, because all file names in the directory begin with this prefix. When you hit <kbd>Tab</kbd> again, the shell will list the possible choices.

```
$ ls SRR09<kbd>Tab</kbd><kbd>Tab</kbd>
```

```
SRR097977.fastq  SRR098026.fastq
```

<kbd>Tab</kbd> completion can also fill in the names of programs, which can be useful if you remember the beginning of a program name.

```
$ pw<kbd>Tab</kbd><kbd>Tab</kbd>
```

```
pwck      pwconv    pwd       pwdx      pwunconv
```

Displays the name of every program that starts with `pw`.

## Summary

We now know how to move around our file system using the command line. This gives us an advantage over interacting with the file system through a GUI as it allows us to work on a remote server, carry out the same set of operations on a large number of files quickly, and opens up many opportunities for using bioinformatics software that is only available in command line versions.

> keypoints:
> - "The `/`, `~`, and `..` characters represent important navigational shortcuts."
> - "Hidden files and directories start with `.` and can be viewed using `ls -a`."
> - "Relative paths specify a location starting from the current location, while absolute paths specify a location from the root of the file system."

## Moving around the file system

We've learned how to use `pwd` to find our current location within our file system. We've also learned how to use `cd` to change locations and `ls` to list the contents of a directory. Now we're going to learn some additional commands for moving around within our file system.

Use the commands we've learned so far to navigate to the `dc_workshop/untrimmed_fastq` directory, if you're not already there.

```
$ cd
$ cd dc_workshop
$ cd untrimmed_fastq
```

What if we want to move back up and out of this directory and to our top level directory? Can we type `cd dc_workshop`? Try it and see what happens.

```
$ cd dc_workshop
```

```
-bash: cd: dc_workshop: No such file or directory
```

Your computer looked for a directory or file called `dc_workshop` within the directory you were already in. It didn't know you wanted to look at a directory level above the one you were located in.

We have a special command to tell the computer to move us back or up one directory level.

```
$ cd ..
```

Now we can use `pwd` to make sure that we are in the directory we intended to navigate to, and `ls` to check that the contents of the directory are correct.

```
$ pwd
```

```
/home/sateeshp/dc_workshop
```

```
$ ls
```

```
sra_metadata  untrimmed_fastq
```

From this output, we can see that `..` did indeed take us back one level in our file system.

You can chain these together like so:

```
$ ls ../../
```

prints the contents of `/home`, which is one level up from your root directory.

> **Finding hidden directories**
>
> First navigate to the `dc_workshop` directory. There is a hidden directory within this directory. Explore the options for `ls` to
> find out how to see hidden directories. List the contents of the directory and
> identify the name of the text file in that directory.
>
> Hint: hidden files and folders in Unix start with `.`, for example `.my_hidden_directory`
>
> > *Solution*
> >
> > First use the `man` command to look at the options for `ls`.
> > ```
> > $ man ls
> > ```
> >
> > The `-a` option is short for `all` and says that it causes `ls` to "not ignore
> > entries starting with ." This is the option we want.
> >
> > ```
> > $ ls -a
> > ```
> >
> > ```
> > .  ..  .hidden	sra_metadata  untrimmed_fastq
> > ```
> >
> > The name of the hidden directory is `.hidden`. We can navigate to that directory
> > using `cd`.
> >
> > ```
> > $ cd .hidden
> > ```
> >
> > And then list the contents of the directory using `ls`.
> >
> > ```
> > $ ls
> > ```
> >
> > ```
> > youfoundit.txt
> > ```
> >
> > The name of the text file is `youfoundit.txt`.

## Examining the contents of other directories

By default, the `ls` commands lists the contents of the working directory (i.e. the directory you are in). You can always find the directory you are in using the `pwd` command. However, you can also give `ls` the names of other directories to view. Navigate to your home directory if you are not already there.

```
$ cd
```

Then enter the command:

```
$ ls dc_workshop
```

```
sra_metadata  untrimmed_fastq
```

This will list the contents of the `dc_workshop` directory without you needing to navigate there.

The `cd` command works in a similar way.

Try entering:

```
$ cd
$ cd dc_workshop/untrimmed_fastq
```

This will take you to the `untrimmed_fastq` directory without having to go through the intermediate directory.

> **Navigating practice**
>
> Navigate to your home directory. From there, list the contents of the `untrimmed_fastq`
> directory.
>
> > ## Solution
> >
> > ```
> > $ cd
> > $ ls dc_workshop/untrimmed_fastq/
> > ```
> >
> > ```
> > SRR097977.fastq  SRR098026.fastq
> > ```

## Full vs. Relative Paths

The `cd` command takes an argument which is a directory name. Directories can be specified using either a *relative* path or a full *absolute* path. The directories on the computer are arranged into a hierarchy. The full path tells you where a directory is in that hierarchy. Navigate to the home directory, then enter the `pwd` command.

```
$ cd
$ pwd
```

You will see:

```
/home/dcuser
```

This is the full name of your home directory. This tells you that you are in a directory called `dcuser`, which sits inside a directory called `home` which sits inside the very top directory in the hierarchy. The very top of the hierarchy is a directory called `/` which is usually referred to as the *root directory*. So, to summarize: `dcuser` is a directory in `home` which is a directory in `/`.

Now enter the following command:

```
$ cd /home/sateeshp/dc_workshop/.hidden
```

This jumps forward multiple levels to the `.hidden` directory. Now go back to the home directory.

```
$ cd
```

You can also navigate to the `.hidden` directory using:

```
$ cd dc_workshop/.hidden
```

These two commands have the same effect, they both take us to the `.hidden` directory. The first uses the absolute path, giving the full address from the home directory. The second uses a relative path, giving only the address from the working directory. A full path always starts with a `/`. A relative path does not.

> A relative path is like getting directions from someone on the street. They tell you to "go right at the stop sign, and then turn left on Main Street". That works great if you're standing there together, but not so well if you're trying to tell someone how to get there from another country. A full path is like GPS coordinates. It tells you exactly where something is no matter where you are right now.
> You can usually use either a full path or a relative path depending on what is most convenient. If we are in the home directory, it is more convenient to enter the relative path since it involves less typing.

Over time, it will become easier for you to keep a mental note of the structure of the directories that you are using and how to quickly navigate amongst them.

> **Relative path resolution**
>
> Using the filesystem diagram below, if `pwd` displays `/Users/thing`,
> what will `ls ../backup` display?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original pnas_final pnas_sub`
>
> > *Solution*
> >  1. No: there *is* a directory `backup` in `/Users`.
> >  2. No: this is the content of `Users/thing/backup`,
> >   but with `..` we asked for one level further up.
> >  3. No: see previous explanation.
> >    Also, we did not specify `-F` to display `/` at the end of the directory names.
> >  4. Yes: `../backup` refers to `/Users/backup`.

### Navigational Shortcuts

There are some shortcuts which you should know about. Dealing with the home directory is very common. The tilde character, `~`, is a shortcut for your home directory. Navigate to the `dc_workshop` directory:

```
$ cd
$ cd dc_workshop
```

Then enter the command:

```
$ ls ~
```

```
dc_workshop
```

This prints the contents of your home directory, without you needing to type the full path.

The commands `cd`, and `cd ~` are very useful for quickly navigating back to your home directory. We will be using the `~` character in later lessons to specify our home directory.
