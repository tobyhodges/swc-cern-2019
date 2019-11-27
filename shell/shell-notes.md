# Unix Shell

## Setup

- Download example files (http://swcarpentry.github.io/shell-novice/data/data-shell.zip)

## Introduction

- What is a shell?
  - program providing interface for users to type commands
  - can't easily see what options are available to us
  - need to learn at least a few commands to help us navigate and find anything else we will need later
- Why learn to use a shell?
  - combine tools into powerful pipelines - do quite complex things quickly
  - write these pipelines in *scripts* - more easily repeat & reproduce analyses
  - view history of commands, exact options used
  - interact with remote machines - less info to send over network
- Open a terminal
- explain prompt - do not type this out - mine looks different from yours
- `ls`
- `ks` - explain error message

## Navigating Files & Directories

- `pwd`
- home directory
- paths -> figure on slide
- `/` means root at the start, separator after that
- `ls` again
- `ls -F` -> slashes on directory names
  - our first option "flag"
- syntax/structure of a command -> figure on slide
  - `ls -F /`
- `ls` has a lot of options
  - `ls --help` is one of them, which we can use to find out about others
- you may also be able to use `man ls`
  - navigation of `man` output; keep this brief as Git Bash doesn't include manpages
- most commands have a `--help` option
- `ls -j` -> invalid option
- if you don't have access to `man` can search the Internet instead
- EXERCISES -> slide
- `ls -F Desktop`
- `ls -F Desktop/data-shell`
- how do we actually get to these other locations?
- `cd Desktop`
- `cd data-shell`
- `cd data`
- `pwd`
- `ls -F`
- how do we move back up the tree?
- we might try `cd data-shell`
- `cd ..`
- `pwd`
- `ls -F -a`
- other hidden files/directories include `.` and might include thinsg like `.bash_profile`, `.git/`
- `.` and `..` are treated the same by every program - not special to `cd`
- `cd`
- `pwd` -> home directory again - useful if you ever get lost
- `cd Desktop/data-shell/data`
- `pwd`
- `cd /path/to/home/Desktop/data-shell/data` -> absolute and relative paths
- `cd ~`
- `cd /bin`
- `cd ~/Desktop`
- `cd -`
- EXERCISES -> slides
- tab completion

## Working With Files and Directories

- make sure we're in `data-shell` directory
- `ls -F`
- `mkdir thesis`
- `ls -F`
- show how this is equivalent in file explorer
- spaces in directory names are bad
- `ls -F thesis`
- `cd thesis`
- `nano draft.txt`
- explain Nano interface
- exit
- `ls`
- `touch my_file.txt` - and look at size, do same for another file already in existence
  - can be useful for initialising files that programs expect to be able to find
- extensions are optional but can be helpful for people (and sometimes software) to predict contents/format
- `cd ~/Desktop/data-shell`
- `mv thesis/draft.txt thesis/quotes.txt`
- `ls thesis`
- `mv thesis/quotes.txt .`
- `ls`
- EXERCISE -> slide
- `cp quotes.txt thesis/quotations.txt`
- `ls thesis`
- `cp -r these thesis_backup`
- `ls thesis thesis_backup`
- EXERCISE -> slide
- `rm quotes.txt`
- `ls`
- on the command line, deleting is forever
- `rm -i thesis-backup/quotations.txt`
- `rm thesis`
- `rm -r thesis`
- wildcards
- `cd ~/Desktop/data-shell/molecules`
- `ls`
- `ls *.pdb`
- `ls *ethane.pdb`
- `ls ?ethane.pdb`
- `ls ???ane.pdb`
- `ls *.pdf` -> error
- EXERCISE -> slide

## Pipes & Filters

- make sure we're in `molecules`
- `wc cubane.pdb` -> lines; words; characters
- `wc *.pdb`
- `wc -l *.pdb`
- `wc -l` -> how to escape this?
- which file contains most lines? easy to answer with small number of files - what if we had 1000s?
- `wc -l *.pdb > lengths.txt`
- `cat lengths.txt`
- `sort lengths.txt`
- `sort -n lengths.txt`
- `sort -n lengths.txt > lengths-sorted.txt`
- `head lengths-sorted.txt`
- `head -n 1 lengths-sorted.txt` -> first time we've provided value to option flag
- `echo The echo command prints text`
- EXERCISE -> slide
- all these intermediate files are confusing, annoying, and messy
- `sort -n lengths.txt | head -n 1`
- `wc -l *.pdb | sort -n | head -n 1`
- EXERCISE -> slide
- this linking together of commands is what makes Unix so successful. Combining simple tools easily into complex workflows
- another example:
- `cd ~/Desktop/data-shell/data/animal-counts`
- `cat animals.txt`
- `cut -d , -f 2 animals.txt`
- `cut -d , -f 2 animals.txt | uniq`
- one final example, using Nelle's data from the North Pacific Gyre:
- `cd ../north-pacific-gyre/2012-07-03`
- `wc -l *.txt`
- `wc -l *.txt | sort -n | head -n 5` -> one of the files is missing 60 lines
-  check whether any others have too many lines
- `wc -l *.txt | sort -n | tail -n 5` -> what's up with those two files ending with Z.txt? Her lab uses Z to indicate samples with missing data
- `ls *Z.txt`
- how can she create pipelines that exclude these two files, without deleting them?
- `ls *[AB].txt`

## Loops

- `cd ../creatures`
- `head basilisk.dat`
- we would like to print out classification of each creature in the directory
- for each file, we need to execute `head` followed by `tail` -> can't do this with a piped command
- need to use a loop

```bash
for filename in basilisk.dat minotaur.dat unicorn.dat
do
  head -n 2 $filename | tail -n 1
done
```

```bash
for x in basilisk.dat minotaur.dat unicorn.dat
do
  head -n 2 $x | tail -n 1
done
```

```bash
for temperature in basilisk.dat minotaur.dat unicorn.dat
do
  head -n 2 $temperature | tail -n 1
done
```

```bash
for filename in *.dat
do
  head -n 2 $filename | tail -n 1
done
```

```bash
for filename in *.dat
do
  echo $filename
  head -n 100 $filename | tail -n 20
done
```

- `history`
- `history | tail -n 5`
- `![number]`

## Shell Scripts

- `cd ~/Desktop/data-shell/molecules`
- `head -n 15 octane.pdb | tail -n 5`
- save to file, `middle.sh` with Nano
- `bash middle.sh`
- `head -n 15 "$1" | tail -n 5`
- why quotes? double quotes vs single quotes
