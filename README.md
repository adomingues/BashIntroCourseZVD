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

 
 ## scripting
 
 ## exercise 1 - loops
 Go through all fastq files and calculate how many entries each one has. We will use `wc -l`, but keep in mind that each entry jas four lines in a fastq file. First let's loop over the files, so we edit the bash script, `script.sh`, to include the loop:
 
 ```bash
 #!/bin/bash

for f in *.fastq;
do
    wc -l $f
done
```

## exercise 2 
How about if we only want to:
- work with the files *_a* and *_b* 
- that have the same number of lines.
- We also want to know how many lines they have
- but the result should be single number.
- The result should be assigned to a variable.

### pipes
To get only the number of lines we can separate the result using the program `cut`. So we *pipe* the result of `wc -l` to cut. Using pipe, `|` we are sending the result of a program to another without printing the results. Example would be `wc -l dummy1_a.fastq | cut -d ' ' -f1`. Wiht cut we can separate a line into fields and select the ones we want. Limitations of pipes:
- pipes are buffered, that is limited to memory
- programs need to read/write from stdin/stdout
- and each one needs the other to finish without errors

### subshells
To assign the number of lines to a variable we use the syntax `linenumber=$(wc -l dummy1_a.fastq | cut -d ' ' -f1)`. The `$()` will capture the output of the command instead of printing it to the terminal. Essentiallz we are running a subshell. We can see the result of *X* with `echo $linenumber`. surrounding the command with backticks will also work, `linenumber=$(wc -l dummy1_a.fastq | cut -d ' ' -f1)`, but it is deprecated. 

### basic maths
`a=5+4` is taken by bash as a string (`echo $a`). Using *let* we now get proper operations `let a=5+4; echo $a`. We can also use an evaluation method with *(())*, so `((a=(5+2)*3)); echo $a`. There other options, so let's look at our script:

```bash
for f in *.fastq;
do
    #wc -l $f
    linenumber=$(wc -l $f | cut -d ' ' -f1)
    ((linenumber /= 4)) # python style, calculates and updates the variable
    result=$((linenumber / 4)) # another solution
    echo "The file $f contains $linenumber entries (or $result)"
done 
```
Or we can use *bc* as in `bc <<< "$(wc -l $f | cut -d ' ' -f1)/4"`, or awk.

NOTE: in pure bash the output of an arithmetic operation is always an integer. So `((a=5/2)); echo $a` will *2* like in python.

### logical tests
```bash
myname="antonio domingues"
yourname="micha myers"

# with 
[ $myname = $yourname ]
# evaluates 2 strings and gives an error. Needs quotes.
[ "$myname" = "$yourname" ]
```

Now a prooper test:
```bash
myname="antonio domingues"
yourname="micha myers"

if [ "$myname" = "$yourname" ]; then
    echo "we are the same person"
else
    echo "we are all individuals"
fi

# for numbers
if [[ 2 gt 3]]; then
    echo "such a large number"
fi
```

