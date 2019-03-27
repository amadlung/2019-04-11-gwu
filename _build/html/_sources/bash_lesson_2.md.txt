Bash lesson 2
===

> objectives:
- View, search within, copy, move, and rename files. Create new directories.
- Use wild cards (`*`) to perform operations on multiple files.
- Make a file read only
- Use the `history` command to view and repeat recently used commands.

>keypoints:
- "You can view file contents using `less`, `cat`, `head` or `tail`."
- "The commands `cp`, `mv`, and `mkdir` are useful for manipulating existing files and creating new directories."
- "You can view file permissions using `ls -l` and change permissions using `chmod`."
- "The `history` command and the up arrow on your keyboard can be used to repeat recently used commands."

## Working with Files

### Our data set: FASTQ files

Now that we know how to navigate around our directory structure, lets start working with our sequencing files. We did a sequencing experiment and have two results files, which are stored in our `untrimmed_fastq` directory.

### Wild cards

Navigate to your `untrimmed_fastq` directory.

```
$ cd ~/shell_data/untrimmed_fastq
```

We are interested in looking at the FASTQ files in this directory. We can list all files with the .fastq extension using the command:

```
$ ls *.fastq
```

```
SRR097977.fastq  SRR098026.fastq
```

The `*` character is a special type of character called a wildcard, which can be used to represent any number of any type of character. Thus, `*.fastq` matches every file that ends with `.fastq`.

This command:

```
$ ls *977.fastq
```

```
SRR097977.fastq
```

lists only the file that ends with `977.fastq`.

We can use the command `echo` to see how the wildcard character is interpreted by the shell.

```
$ echo *.fastq
```

```
SRR097977.fastq SRR098026.fastq
```

The `*` is expanded to include any file that ends with `.fastq`.

This command:

```
$ ls /usr/bin/*.sh
```

```
/usr/bin/amuFormat.sh  /usr/bin/gettext.sh  /usr/bin/gvmap.sh
```

Lists every file in `/usr/bin` that ends in the characters `.sh`.

> **Home vs. Root**
>
> The `/` character is another navigational shortcut and refers to your root directory.
> The root directory is the highest level directory in your file system and contains
> files that are important for your computer to perform its daily work, but which you usually won't
> have to interact with directly. In our case,
> the root directory is two levels above our home directory, so `cd` or `cd ~` will take you to `/home/dcuser`
> and `cd /` will take you to `/`, which is equivalent to `~/../../`. Try not to worry if this is confusing,
> it will all become clearer with practice.
>
> While you will be using the root at the beginning of your absolute paths, it is important that you avoid
> working with data in these higher-level directories, as your commands can permanently alter files that the
> operating system needs to function. In many cases, trying to run commands in root directories will require
> special permissions which are not discussed here, so it's best to avoid it and work within your home directory.

> **Exercise**
> Do each of the following tasks from your current directory using a single
> `ls` command for each.
>
> 1.  List all of the files in `/usr/bin` that start with the letter 'c'.
> 2.  List all of the files in `/usr/bin` that contain the letter 'a'.
> 3.  List all of the files in `/usr/bin` that end with the letter 'o'.
>
> Bonus: List all of the files in `/usr/bin` that contain the letter 'a' or the
> letter 'c'.
>
> Hint: The bonus question requires a Unix wildcard that we haven't talked about
> yet. Trying searching the internet for information about Unix wildcards to find
> what you need to solve the bonus problem.
>
> > *Solution*
> > 1. `ls /usr/bin/c*`
> > 2. `ls /usr/bin/*a*`
> > 3. `ls /usr/bin/*o`
> > Bonus: `ls /usr/bin/*[ac]*`

## Command History

If you want to repeat a command that you've run recently, you can access previous commands using the up arrow on your keyboard to go back to the most recent command. Likewise, the down arrow takes you forward in the command history.

A few more useful shortcuts:

- <kbd>Ctrl</kbd> + <kbd>C</kbd> will cancel the command you are writing, and give you a
fresh prompt.
- <kbd>Ctrl</kbd>+<kbd>R</kbd> will do a reverse-search through your command history.  This
is very useful.
- <kbd>Ctrl</kbd>+<kbd>L</kbd> or the `clear` command will clear your screen.

You can also review your recent commands with the `history` command, by entering:

```
$ history
```

to see a numbered list of recent commands. You can reuse one of these commands directly by referring to the number of that command.

For example, if your history looked like this:

```
259  ls *
260  ls /usr/bin/*.sh
261  ls *R1*fastq
```

then you could repeat command #260 by entering:

```
$ !260
```

Type `!` (exclamation point) and then the number of the command from your history. You will be glad you learned this when you need to re-run very complicated commands.

> **Exercise**
> Find the line number in your history for the command that listed all the .sh
> files in `/usr/bin`. Rerun that command.
>
> > *Solution*
> > First type `history`. Then use `!` followed by the line number to rerun that command.

## Examining Files

We now know how to switch directories, run programs, and look at the contents of directories, but how do we look at the contents of files?

One way to examine a file is to print out all of the contents using the program `cat`.

Enter the following command from within the `untrimmed_fastq` directory:

```
$ cat SRR098026.fastq
```

This will print out all of the contents of the `SRR098026.fastq` to the screen.

> **Exercise**
>
> 1. Print out the contents of the `~/shell_data/untrimmed_fastq/SRR097977.fastq` file. What is the last line of the file?
> 2.  From your home directory, and without changing directories,
> use one short command to print the contents of all of the files in
> the `~/shell_data/untrimmed_fastq` directory.
>
> > *Solution*
> > 1. The last line of the file is `TC:CCC::CCCCCCCC<8?6A:C28C<608'&&&,'$`.
> > 2. `cat ~/shell_data/untrimmed_fastq/*`


`cat` is a terrific program, but when the file is really big, it can be annoying to use. The program, `less`, is useful for this case. `less` opens the file as read only, and lets you navigate through it. The navigation commands are identical to the `man` program.

Enter the following command:

```
$ less SRR097977.fastq
```

Some navigation commands in `less`

 - <kbd>Space</kbd> : to go forward
 - <kbd>b</kbd>    : to go backward
 - <kbd>g</kbd>    : to go to the beginning
 - <kbd>G</kbd>    : to go to the end
 - <kbd>q</kbd>    : to quit

`less` also gives you a way of searching through files. Use the "/" key to begin a search. Enter the word you would like to search for and press `enter`. The screen will jump to the next location where that word is found.

Remember, the `man` program actually uses `less` internally and therefore uses the same commands, so you can search documentation using "/" as well!

There's another way that we can look at files, and in this case, just look at part of them. This can be particularly useful if we just want to see the beginning or end of the file, or see how it's formatted.

The commands are `head` and `tail` and they let you look at the beginning and end of a file, respectively.

```
$ head SRR098026.fastq
```

```
@SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
NNNNNNNNNNNNNNNNCNNNNNNNNNNNNNNNNNN
+SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
!!!!!!!!!!!!!!!!#!!!!!!!!!!!!!!!!!!
@SRR098026.2 HWUSI-EAS1599_1:2:1:0:312 length=35
NNNNNNNNNNNNNNNNANNNNNNNNNNNNNNNNNN
+SRR098026.2 HWUSI-EAS1599_1:2:1:0:312 length=35
!!!!!!!!!!!!!!!!#!!!!!!!!!!!!!!!!!!
@SRR098026.3 HWUSI-EAS1599_1:2:1:0:570 length=35
NNNNNNNNNNNNNNNNANNNNNNNNNNNNNNNNNN
```

```
$ tail SRR098026.fastq
```

```
+SRR098026.247 HWUSI-EAS1599_1:2:1:2:1311 length=35
#!##!#################!!!!!!!######
@SRR098026.248 HWUSI-EAS1599_1:2:1:2:118 length=35
GNTGNGGTCATCATACGCGCCCNNNNNNNGGCATG
+SRR098026.248 HWUSI-EAS1599_1:2:1:2:118 length=35
B!;?!A=5922:##########!!!!!!!######
@SRR098026.249 HWUSI-EAS1599_1:2:1:2:1057 length=35
CNCTNTATGCGTACGGCAGTGANNNNNNNGGAGAT
+SRR098026.249 HWUSI-EAS1599_1:2:1:2:1057 length=35
A!@B!BBB@ABAB#########!!!!!!!######
```

The `-n` option to either of these commands can be used to print the first or last `n` lines of a file.

```
$ head -n 1 SRR098026.fastq
```

```
@SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
```

```
$ tail -n 1 SRR098026.fastq
```

```
A!@B!BBB@ABAB#########!!!!!!!######
```

## Details on the FASTQ format

Although it looks complicated (and it is), it's easy to understand the [fastq](https://en.wikipedia.org/wiki/FASTQ_format) format with a little decoding. Some rules about the format include...

1. Always begins with '@' and then information about the read
2. The actual DNA sequence
3. Always begins with a '+' and sometimes the same info in line 1
4. Has a string of characters which represent the quality scores; must have same number of characters as line 2

We can view the first complete read in one of the files our dataset by using `head` to look at the first four lines.

```
$ head -n4 SRR098026.fastq
```

```
@SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
NNNNNNNNNNNNNNNNCNNNNNNNNNNNNNNNNNN
+SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
!!!!!!!!!!!!!!!!#!!!!!!!!!!!!!!!!!!
```

All but one of the nucleotides in this read are unknown (`N`). This is a pretty bad read!

Line 4 shows the quality for each nucleotide in the read. Quality is interpreted as the probability of an incorrect base call (e.g. 1 in 10) or, equivalently, the base call accuracy (eg 90%). To make it possible to line up each individual nucleotide with its quality score, the numerical score is converted into a code where each individual character represents the numerical quality score for an individual nucleotide. For example, in the line above, the quality score line is:

```
!!!!!!!!!!!!!!!!#!!!!!!!!!!!!!!!!!!
```

The `#` character and each of the `!` characters represent the encoded quality for an individual nucleotide. The numerical value assigned to each of these characters depends on the sequencing platform that generated the reads. The sequencing machine used to generate our data uses the standard Sanger quality PHRED score encoding, using by Illumina version 1.8 onwards. Each character is assigned a quality score between 0 and 40 as shown in the chart below.

```
Quality encoding: !"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHI
                  |         |         |         |         |
Quality score:    0........10........20........30........40
```

Each quality score represents the probability that the corresponding nucleotide call is incorrect. This quality score is logarithmically based, so a quality score of 10 reflects a base call accuracy of 90%, but a quality score of 20 reflects a base call accuracy of 99%. These probability values are the results from the base calling algorithm and dependent on how much signal was captured for the base incorporation.

Looking back at our read:

```
@SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
NNNNNNNNNNNNNNNNCNNNNNNNNNNNNNNNNNN
+SRR098026.1 HWUSI-EAS1599_1:2:1:0:968 length=35
!!!!!!!!!!!!!!!!#!!!!!!!!!!!!!!!!!!
```

we can now see that the quality of each of the `N`s is 0 and the quality of the only nucleotide call (`C`) is also very poor (`#` = a quality score of 2). This is indeed a very bad read.


## Creating, moving, copying, and removing

Now we can move around in the file structure, look at files, and search files. But what if we want to copy files or move them around or get rid of them? Most of the time, you can do these sorts of file manipulations without the command line, but there will be some cases (like when you're working with a remote computer like we are for this lesson) where it will be impossible. You'll also find that you may be working with hundreds of files and want to do similar manipulations to all of those files. In cases like this, it's much faster to do these operations at the command line.

### Copying Files

When working with computational data, it's important to keep a safe copy of that data that can't be accidentally overwritten or deleted. For this lesson, our raw data is our FASTQ files.  We don't want to accidentally change the original files, so we'll make a copy of them and change the file permissions so that we can read from, but not write to, the files.

First, let's make a copy of one of our FASTQ files using the `cp` command.

Navigate to the `shell_data/untrimmed_fastq` directory and enter:

```
$ cp SRR098026.fastq SRR098026-copy.fastq
$ ls -F
```

```
SRR097977.fastq  SRR098026-copy.fastq  SRR098026.fastq
```

We now have two copies of the `SRR098026.fastq` file, one of them named `SRR098026-copy.fastq`. We'll move this file to a new directory called `backup` where we'll store our backup data files.

### Creating Directories

The `mkdir` command is used to make a directory. Enter `mkdir` followed by a space, then the directory name you want to create.

```
$ mkdir backup
```

### Moving / Renaming

We can now move our backup file to this directory. We can move files around using the command `mv`.

```
$ mv SRR098026-copy.fastq backup
$ ls backup
```

```
SRR098026-copy.fastq
```

The `mv` command is also how you rename files. Let's rename this file to make it clear that this is a backup.

```
$ cd backup
$ mv SRR098026-copy.fastq SRR098026-backup.fastq
$ ls
```

```
SRR098026-backup.fastq
```

### File Permissions

We've now made a backup copy of our file, but just because we have two copies doesn't make us safe. We can still accidentally delete or overwrite both copies. To make sure we can't accidentally mess up this backup file, we're going to change the permissions on the file so that we're only allowed to read (i.e. view) the file, not write to it (i.e. make new changes).

View the current permissions on a file using the `-l` (long) flag for the `ls` command.

```
$ ls -l
```

```
-rw-r--r-- 1 dcuser dcuser 43332 Nov 15 23:02 SRR098026-backup.fastq
```

The first part of the output for the `-l` flag gives you information about the file's current permissions. There are ten slots in the permissions list. The first character in this list is related to file type, not permissions, so we'll ignore it for now. The next three characters relate to the permissions that the file owner has, the next three relate to the permissions for group members, and the final three characters specify what other users outside of your group can do with the file. We're going to concentrate on the three positions
that deal with your permissions (as the file owner).


Here the three positions that relate to the file owner are `rw-`. The `r` means that you have permission to read the file, the `w` indicates that you have permission to write to (i.e. make changes to) the file, and the third position is a `-`, indicating that you don't have permission to carry out the ability encoded by that space (this is the space where `x` or executable ability is stored, we'll talk more about this in [a later lesson](http://www.datacarpentry.org/shell-genomics/05-writing-scripts/)).

Our goal for now is to change permissions on this file so that you no longer have `w` or write permissions. We can do this using the `chmod` (change mode) command and subtracting (`-`) the write permission `-w`.

```
$ chmod -w SRR098026-backup.fastq
$ ls -l
```

```
-r--r--r-- 1 dcuser dcuser 43332 Nov 15 23:02 SRR098026-backup.fastq
```

### Removing

To prove to ourselves that you no longer have the ability to modify this file, try deleting it with the `rm` command.

```
$ rm SRR098026-backup.fastq
```

You'll be asked if you want to override your file permissions.

```
rm: remove write-protected regular file ‘SRR098026-backup.fastq’?
```

If you enter `n` (for no), the file will not be deleted. If you enter `y`, you will delete the file. This gives us an extra measure of security, as there is one more step between us and deleting our data files.

**Important**: The `rm` command permanently removes the file. Be careful with this command. It doesn't
just nicely put the files in the Trash. They're really gone.

By default, `rm` will not delete directories. You can tell `rm` to delete a directory using the `-r` (recursive) option. Let's delete the backup directory we just made.

Enter the following command:

```
$ cd ..
$ rm -r backup
```

This will delete not only the directory, but all files within the directory. If you have write-protected files in the directory, you will be asked whether you want to override your permission settings.

> **Exercise**
>
> Starting in the `shell_data/untrimmed_fastq/` directory, do the following:
> 1. Make sure that you have deleted your backup directory and all files it contains.
> 2. Create a copy of each of your FASTQ files. (Note: You'll need to do this individually for each of the two FASTQ files. We haven't
> learned yet how to do this
> with a wild-card.)
> 3. Use a wildcard to move all of your backup files to a new backup directory.
> 4. Change the permissions on all of your backup files to be write-protected.
>
> > *Solution*
> >
> > 1. `rm -r backup`
> > 2. `cp SRR098026.fastq SRR098026-backup.fastq` and `cp SRR097977.fastq SRR097977-backup.fastq`
> > 3. `mkdir backup` and `mv *-backup.fastq backup`
> > 4. `chmod -w backup/*-backup.fastq`
> > It's always a good idea to check your work with `ls -l backup`. You should see something like:
> >
> > ```
> > -r--r--r-- 1 dcuser dcuser 47552 Nov 15 23:06 SRR097977-backup.fastq
> > -r--r--r-- 1 dcuser dcuser 43332 Nov 15 23:06 SRR098026-backup.fastq
> > ```

---
objectives:
- "Employ the `grep` command to search for information within files."
- "Print the results of a command to a file."
- "Construct command pipelines with two or more stages."
keypoints:
- "`grep` is a powerful search tool with many options for customization."
- "`>`, `>>`, and `|` are different ways of redirecting output."
- "`command > file` redirects a command's output to a file."
- "`command >> file` redirects a command's output to a file without overwriting the existing contents of the file."
- "`command_1 | command_2` redirects the output of the first command as input to the second command."
- "for loops are used for iteration"
- "`basename` gets rid of repetitive parts of names"
---

## Searching files

We discussed in a previous episode how to search within a file using `less`. We can also search within files without even opening them, using `grep`. `grep` is a command-line utility for searching plain-text files for lines matching a specific set of characters (sometimes called a string) or a particular pattern (which can be specified using something called regular expressions). We're not going to work with regular expressions in this lesson, and are instead going to specify the strings we are searching for.

Let's give it a try!

> **Nucleotide abbreviations**
>
> The four nucleotides that appear in DNA are abbreviated `A`, `C`, `T` and `G`.
> Unknown nucleotides are represented with the letter `N`. An `N` appearing
> in a sequencing file represents a position where the sequencing machine was not able to
> confidently determine the nucleotide in that position. You can think of an `N` as a `NULL` value
> within a DNA sequence.

We'll search for strings inside of our fastq files. Let's first make sure we are in the correct directory.

```
$ cd ~/shell_data/untrimmed_fastq
```

Suppose we want to see how many reads in our file have really bad segments containing 10 consecutive unknown nucleoties (Ns). Let's search for the string NNNNNNNNNN in the SRR098026 file.

> **Determining quality**
>
> In this lesson, we're going to be manually searching for strings of `N`s within our sequence
> results to illustrate some principles of file searching. It can be really useful to do this
> type of searching to get a feel for the quality of your sequencing results, however, in you
> research you will most likely use a bioinformatics tool that has a built-in program for
> filtering out low-quality reads. You'll learn how to use one such tool in
> [a later lesson](http://www.datacarpentry.org/wrangling-genomics/00-readQC/).

```
$ grep NNNNNNNNNN SRR098026.fastq
```

This command returns a lot of output to the terminal. Every single line in the SRR098026 file that contains at least 10 consecutive Ns is printed to the terminal, regardless of how long or short the file is. We may be interested not only in the actual sequence which contains this string, but in the name (or identifier) of that sequence. We discussed in a previous lesson that the identifier line immediately precedes the nucleotide sequence for each read in a FASTQ file. We may also want to inspect the quality scores associated with each of these reads. To get all of this information, we will return the line immediately before each match and the two lines immediately after each match.

We can use the `-B` argument for grep to return a specific number of lines before each match and the `-A` argument to return a specific number of lines after each matching line. Here we want the line before and the two lines after each matching line so we add `-B1 -A2` to our grep command.

```
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq
```

One of the sets of lines returned by this command is:

```
@SRR098026.177 HWUSI-EAS1599_1:2:1:1:2025 length=35
CNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNNN
+SRR098026.177 HWUSI-EAS1599_1:2:1:1:2025 length=35
#!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
```

> **Exercise**
>
> 1. Search for the sequence `GNATNACCACTTCC` in the `SRR098026.fastq` file.
> Have your search return all matching lines and the name (or identifier) for each sequence
> that contains a match.
>
> 2. Search for the sequence `AAGTT` in both FASTQ files.
> Have your search return all matching lines and the name (or identifier) for each sequence
> that contains a match.
>
> > *Solution*
> > 1. `grep -B1 GNATNACCACTTCC SRR098026.fastq`
> > 2. `grep -B1 AAGTT *.fastq`

## Redirecting output

`grep` allowed us to identify sequences in our FASTQ files that match a particular pattern. All of these sequences were printed to our terminal screen, but in order to work with these sequences and perform other opperations on them, we will need to capture that output in some way.

We can do this with something called "redirection". The idea is that we are taking what would ordinarily be printed to the terminal screen and redirecting it to another location. In our case, we want to print this information to a file so that we can look at it later and use other commands to analyze this data.

The command for redirecting output to a file is `>`.

Let's try out this command and copy all the records (including all four lines of each record) in our FASTQ files that contain 'NNNNNNNNNN' to another file called `bad_reads.txt`.

```
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq > bad_reads.txt
```

> **File extensions**
>
> You might be confused about why we're naming our output file with a `.txt` extension. After all,
> it will be holding FASTQ formatted data that we're extracting from our FASTQ files. Won't it
> also be a FASTQ file? The answer is, yes - it will be a FASTQ file and it would make sense to
> name it with a `.fastq` extension. However, using a `.fastq` extension will lead us to problems
> when we move to using wildcards later in this episode. We'll point out where this becomes
> important. For now, it's good that you're thinking about file extensions!


The prompt should sit there a little bit, and then it should look like nothing happened. But type `ls`. You should see a new file called `bad_reads.txt`.

We can check the number of lines in our new file using a command called `wc`. `wc` stands for **word count**. This command counts the number of words, lines, and characters in a file.

```
$ wc bad_reads.txt
```

```
  537  1073 23217 bad_reads.txt
```

This will tell us the number of lines, words and characters in the file. If we want only the number of lines, we can use the `-l` flag for `lines`.

```
$ wc -l bad_reads.txt
```

```
537 bad_reads.txt
```

Because we asked `grep` for all four lines of each FASTQ record, we need to divide the output by four to get the number of sequences that match our search pattern.

> **Exercise**
>
> How many sequences in `SRR098026.fastq` contain at least 3 consecutive Ns?
>
>> *Solution*
>>
>>
>> ```
>> $ grep NNN SRR098026.fastq > bad_reads.txt
>> $ wc -l bad_reads.txt
>> ```
>>
>> ```
>> 249
>> ```


We might want to search multiple FASTQ files for sequences that match our search pattern. However, we need to be careful, because each time we use the `>` command to redirect output to a file, the new output will replace the output that was already present in the file. This is called "overwriting" and, just like you don't want to overwrite your video recording of your kid's first birthday party, you also want to avoid overwriting your data files.

```
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq > bad_reads.txt
$ wc -l bad_reads.txt
```

```
537 bad_reads.txt
```

```
$ grep -B1 -A2 NNNNNNNNNN SRR097977.fastq > bad_reads.txt
$ wc -l bad_reads.txt
```

```
0 bad_reads.txt
```

Here, the output of our second  call to `wc` shows that we no longer have any lines in our `bad_reads.txt` file. This is because the second file we searched (`SRR097977.fastq`) does not contain any lines that match our
search sequence. So our file was overwritten and is now empty.

We can avoid overwriting our files by using the command `>>`. `>>` is known as the "append redirect" and will
append new output to the end of a file, rather than overwriting it.

```
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq > bad_reads.txt
$ wc -l bad_reads.txt
```

```
537 bad_reads.txt
```

```
$ grep -B1 -A2 NNNNNNNNNN SRR097977.fastq >> bad_reads.txt
$ wc -l bad_reads.txt
```

```
537 bad_reads.txt
```

The output of our second call to `wc` shows that we have not overwritten our original data.

We can also do this with a single line of code by using a wildcard.

```
$ grep -B1 -A2 NNNNNNNNNN *.fastq > bad_reads.txt
$ wc -l bad_reads.txt
```

```
537 bad_reads.txt
```

> **File extensions - part 2**
>
> This is where we would have trouble if we were naming our output file with a `.fastq` extension.
> If we already had a file called `bad_reads.fastq` (from our previous `grep` practice)
> and then ran the command above using a `.fastq` extension instead of a `.txt` extension, `grep`
> would give us a warning.
>
> ```
> grep -B1 -A2 NNNNNNNNNN \*.fastq > bad_reads.fastq
> ```
>
> ```
> grep: input file ‘bad_reads.fastq’ is also the output
> ```
>
> `grep` is letting you know that the output file `bad_reads.fastq` is also included in your
> `grep` call because it matches the `*.fastq` pattern. Be careful with this as it can lead to
> some surprising output.

Since we might have multiple different criteria we want to search for, creating a new output file each time has the potential to clutter up our workspace. We also so far haven't been interested in the actual contents of those files, only in the number of reads that we've found. We created the files to store the reads and then counted the lines in the file to see how many reads matched our criteria. There's a way to do this, however, that doesn't require us to create these intermediate files - the pipe command (`|`).

This is probably not a key on your keyboard you use very much, so let's all take a minute to find that key.
What `|` does is take the output that is scrolling by on the terminal and uses that output as input to another command. When our output was scrolling by, we might have wished we could slow it down and look at it, like we can with `less`. Well it turns out that we can! We can redirect our output from our `grep` call through the `less` command.

```
$ grep -B1 -A2 NNNNNNNNNN SRR098026.fastq | less
```

We can now see the output from our `grep` call within the `less` interface. We can use the up and down arrows
to scroll through the output and use `q` to exit `less`.

Redirecting output is often not intuitive, and can take some time to get used to. Once you're comfortable with redirection, however, you'll be able to combine any number of commands to do all sorts of exciting things with your data!

None of the command line programs we've been learning do anything all that impressive on their own, but when you start chaining them together, you can do some really powerful things very efficiently. Let's take a few minutes to practice.

> **Exercise**
>
> Now that we know about the pipe (`|`), write a single command to find the number of reads
> in the `SRR098026.fastq` file that contain at least two regions of 5 unknown
> nucleotides in a row, separated by any number of known nucleotides. Do this without creating
> a new file.
>
>> *Solution*
>>
>> ```
>> $ grep "NNNNN*NNNNN" SRR098026.fastq | wc -l
>> ```
>>
>> ```
>> 186
>> ```

## File manipulation and more practice with pipes

Let's use the tools we've added to our tool kit so far, along with a few new ones, to example our SRA metadata file. First, let's navigate to the correct directory.

```
$ cd
$ cd shell_data/sra_metadata
```

This file contains a lot of information about the samples that we submitted for sequencing. We took a look at this file in an earlier lesson. Here we're going to use the information in this file to answer some questions about our samples.

#### How many of the read libraries are paired end?

The samples that we submitted to the sequencing facility were a mix of single and paired end libraries. We know that we recorded information in our metadata table about which samples used which library preparation method, but we don't remember exactly where this data is recorded. Let's start by looking at our column headers to see which column might have this information. Our column headers are in the first row of our data table, so we can use `head` with a `-n` flag to look at just the first row of the file.

```
$ head -n 1 SraRunTable.txt
```

```
BioSample_s	InsertSize_l	LibraryLayout_s	Library_Name_s	LoadDate_s	MBases_l	MBytes_l	ReleaseDate_s Run_s SRA_Sample_s Sample_Name_s Assay_Type_s AssemblyName_s BioProject_s Center_Name_s Consent_s Organism_Platform_s SRA_Study_s g1k_analysis_group_s g1k_pop_code_s source_s strain_s
```

That is only the first line of our file, but because there are a lot of columns, the output likely wraps around your terminal window and appears as multiple lines. Once we figure out which column our data is in, we can use a command called `cut` to extract the column of interest.

Because this is pretty hard to read, we can look at just a few column header names at a time by combining the `|` redirect and `cut`.

```
$ head -n 1 SraRunTable.txt | cut -f1-4
```

`cut` takes a `-f` flag, which stands for "field". This flag accepts a list of field numbers, in our case, column numbers. Here we are extracting the first four column names.

```
BioSample_s InsertSize_l      LibraryLayout_s	Library_Name_s
```

The Library Layout_s column looks like it should have the information we want.  Let's look at some of the data from that column. We can use `cut` to extract only the 3rd column from the file and then use the `|` operator with `head` to look at just the first few lines of data in that column.

```
$ cut -f3 SraRunTable.txt | head -n 10
```

```
Library Layout_s
SINGLE
SINGLE
SINGLE
SINGLE
SINGLE
SINGLE
SINGLE
SINGLE
PAIRED
```

We can see that there are (at least) two categories, SINGLE and PAIRED.  We want to search all entries in this column for just PAIRED and count the number of matches. For this, we will use the `|` operator twice
to combine `cut` (to extract the column we want), `grep` (to find matches) and `wc` (to count matches).

```
$ cut -f3 SraRunTable.txt | grep PAIRED | wc -l
```

```
2
```

We can see from this that we have only two paired-end libraries in the samples we submitted for
sequencing.

> **Exercise**
>
> How many single-end libraries are in our samples?
>
>> *Solution*
>> ```
>> $ cut -f3 SraRunTable.txt | grep SINGLE | wc -l
>> ```
>>
>> ```
>> 35
>> ```

#### How many of each class of library layout are there?

We can extract even more information from our metadata table if we add in some new tools: `sort` and `uniq`. The `sort` command will sort the lines of a text file and the `uniq` command will filter out repeated neighboring lines in a file. You might expect `uniq` to extract all of the unique lines in a file. This isn't what it does, however, for reasons involving computer memory and speed. If we want to extract all unique lines, we can do so by combining `uniq` with `sort`. We'll see how to do this soon.

For example, if we want to know how many samples of each library type are recorded in our table, we can extract the third column (with `cut`), and pipe that output into `sort`.

```
$ cut -f3 SraRunTable.txt | sort
```

If you look closely, you might see that we have one line that reads "Library Layout_s". This is the header of our column. We can discard this information using the `-v` flag in `grep`, which means return all the lines that **do not** match the search pattern.

```
$ cut -f3 SraRunTable.txt | grep -v LibraryLayout_s | sort
```

This command returns a sorted list (too long to show here) of PAIRED and SINGLE values. We can use the `uniq` command to see a list of all the different categories that are present. If we do this, we see that the only two types of libraries we have present are labelled PAIRED and SINGLE. There aren't any other types in our file.

```
$ cut -f3 SraRunTable.txt | grep -v LibraryLayout_s | sort | uniq
```

```
PAIRED
SINGLE
```

If we want to count how many of each we have, we can use the `-c` (count) flag for `uniq`.

```
$ cut -f3 SraRunTable.txt | grep -v LibraryLayout_s | sort | uniq -c
```

```
2 PAIRED
35 SINGLE
```

> **Exercise**
>
> 1. How many different sample load dates are there?
> 2. How many samples were loaded on each date?
>
>> *Solution*
>>
>> 1. There are two different sample load dates.
>>
>>    ```
>>    cut -f5 SraRunTable.txt | grep -v LoadDate_s | sort | uniq
>>    ```
>>
>>    ```
>>    25-Jul-12
>>    29-May-14
>>    ```
>>
>> 2. Six samples were loaded on one date and 31 were loaded on the other.
>>
>>    ```
>>    cut -f5 SraRunTable.txt | grep -v LoadDate_s | sort | uniq -c
>>    ```
>>
>>    ```
>>     6 25-Jul-12
>>    31 29-May-14
>>    ```

#### Can we sort the file by library layout and save that sorted information to a new file?

We might want to re-order our entire metadata table so that all of the paired-end samples appear together and all of the single-end samples appear together. We can use the `-k` (key) flag for `sort` to sort based on a particular column. This is similar to the `-f` flag for `cut`.

Let's sort based on the third column (`-k3`) and redirect our output to a new file.

```
$ sort -k3 SraRunTable.txt > SraRunTable_sorted_by_layout.txt
```

#### Can we extract only paired-end records into a new file?

We also might want to extract the information for all samples that meet a specific criterion (for example, are paired-end) and put those lines of our table in a new file. First, we need to check to make sure that the pattern we're searching for ("PAIRED") only appears in the column where we expect it to occur (column 3). We know from earlier that there are only two paired-end samples in the file, so we can `grep` for "PAIRED" and see how many results we get.

```
$ grep PAIRED SraRunTable.txt | wc -l
```

```
2
```

There are only two results, so we can use "PAIRED" as our search term to extract the paired-end samples to a new file.

```
$ grep PAIRED SraRunTable.txt > SraRunTable_only_paired_end.txt
```

> ## Exercise
> Sort samples by load date and export each of those sets to a new file (one new file per
> unique load date).
>
> > ## Solution
> >
> > ```
> > $ grep 25-Jul-12 SraRunTable.txt > SraRunTable_25-Jul-12.txt
> > $ grep 29-May-14 SraRunTable.txt > SraRunTable_29-May-14.txt
> > ```

## Writing for loops

Loops are key to productivity improvements through automation as they allow us to execute commands repeatedly.
Similar to wildcards and tab completion, using loops also reduces the amount of typing (and typing mistakes).
Loops are helpful when performing operations on groups of sequencing files, such as unzipping or trimming multiple files. We will use loops for these purposes in subsequent analyses, but will cover the basics of them for now.

When the shell sees the keyword `for`, it knows to repeat a command (or group of commands) once for each item in a list. Each time the loop runs (called an iteration), an item in the list is assigned in sequence to the **variable**, and the commands inside the loop are executed, before moving on to  the next item in the list. Inside the loop, we call for the variable's value by putting `$` in front of it. The `$` tells the shell interpreter to treat the **variable** as a variable name and substitute its value in its place, rather than treat it as text or an external command.

Let's write a for loop to show us the first two lines of the fastq files we downloaded earlier. You will notice shell prompt changes from `$` to `>` and back again as we were typing in our loop. The second prompt, `>`, is different to remind us that we haven’t finished typing a complete command yet. A semicolon, `;`, can be used to separate two commands written on a single line.

```
$ cd ../untrimmed_fastq/
```

```
$ for filename in *.fastq
> do
> head -n 2 ${filename}
> done
```

The for loop begins with the formula `for <variable> in <group to iterate over>`. In this case, the word `filename` is designated as the variable to be used over each iteration. In our case `SRR097977.fastq` and `SRR098026.fastq` will be substituted for `filename` because they fit the pattern of ending with .fastq in directory we've specified. The next line of the for loop is `do`. The next line is the code that we want to excute. We are telling the loop to print the first two lines of each variable we iterate over. Finally, the
word `done` ends the loop.

After executing the loop, you should see the first two lines of both fastq files printed to the terminal. Let's create a loop that will save this information to a file.

```
$ for filename in *.fastq
> do
> head -n 2 ${filename} >> seq_info.txt
> done
```

Note that we are using `>>` to append the text to our `seq_info.txt` file. If we used `>`, the `seq_info.txt` file would be rewritten every time the loop iterates, so it would only have text from the last variable used. Instead, `>>` adds to the end of the file.

### Using Basename in for loops

Basename is a function in UNIX that is helpful for removing a uniform part of a name from a list of files. In this case, we will use basename to remove the `*.fastq` from the files that we've been working with. Inside our for loop, we create a new `name` variable. We call the `basename` function inside the parenthesis, then give our variable name from the for loop, in this case `$filename`, and finally state that `.fastq` should be removed from the file name. It's important to note that we're not changing the actual files, we're creating and manipulating a new variable called `name`. The line `> echo $name` will print to the terminal the variable `name`
each time the for loop runs. Because we are iterating over two files, we expect to see two lines of output.

```
$ for filename in *.fastq
> do
> name=$(basename ${filename} .fastq)
> echo ${name}
> done
```

Although the utility of basename may still seem unclear, it will become very useful in subsequent analysis, such as trimming many reads in a for loop.
