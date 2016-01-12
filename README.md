# BashIntroCourseZVD
Introduction to Bash by the ZDV (JGU, Mainz)

## Enable a terminal

since we are logging in via putty, we need to start an actual bash terminal from the putty terminal:

```bash
 xterm &
```

Done. We are now on a proper shell :) A layer between the user and the OS. A shell is not a terminal, but a terminal giver access to a shell. In principle, one could access a shell from the an internet browser.

## BASH 
The world famous Bourne again shell. Useful:
- for simple procedure script, there is sequencial:
- simple arithmetics,
- and a few bash commands suffice (ls, rename, wc, find, etc.)
Not for:
- complex maths,
- complex logical operations.

## Help
Just use `man ls` or even `man man`. `$` is the prompt. Think of it as the "ready, start, go" sign of the shell. you see it, you can send commands.

## [TAB]
It is beatuifull. Just start writing the path and press [TAB] to auto-complete:

```bash
ls /clust[TAB]
ls /cluster
```

It also works for commands:

```bash
hist[TAB]
history
fc -l # same as history
```

Mind that auto-complete requires that the command/path is unique. Otherwise it will list the several posibilities. Another neat posibility to find previously typed commands is hiting `CTRL+r` in the shell and start typing the command. Very very useful.

NOTE: `CTRL+c`, or copy from the terminal, does not seem to work on windows.

## Linux file structure

- `/` represents the start of the tree
- `~` is the home directory of the user
- `pwd` is very useful to find out the working current directory

## File permissions
`ls -l` displays the file permission of all files and folders in the current directory. The first character tells wether the file is a regular file (-), adn directory (d), a symbolic link (l). There are more types but these are the most common. Then comes the user, group and all users rights. Each is a set of 3 charcters that if present:
- r, able to read
- w, able to write,
- x, executable
- `-` means no permission.

`ls -l` also lists the number of blocks the listed files take up on disk. For human readable file size use `ls -lh`. Adding the argument `a` will list hidden files. Every directory will list two folders:
> .
> ..

One is the current directory the other the directory up.

### Change permissions
```bash
chmod g+w # give your group write permissions 
chmod g-x # take your group exectution permissions 
chmod a+x # anyone can execute
chmod g-r -R # remove read permissions from group in all files (recursively)
```

`chown` also exist to change file owner.

## Create directories
```bash
mkdir directory
mkdir -p directory/subdir1/subdir2 # creates mutiple directories at once, nested
```

Now we can have fun changing directories with tab completion. Using `cd` without a path goes back to the home directory. So less typing than `cd ~`. Another neat trick is to use `cd $OLDPWD` to go back to subdir2.

We can create a new test file with `echo "asdf" > dummy.txt`. If we want a hidden file just add a dot to the file name: `echo "asdf" > .dummy.txt`.

