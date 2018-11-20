# Introduction

*Much of this page was copied directly from the [HPCC](http://hpcc.ucr.edu/) Linux manual page.

Linux used as the operating system for most super computers.

#### GNU/Linux Distributions

* [Ubuntu](https://www.ubuntu.com/) - A beginner-friendly Linux OS based on Debian. A good choice for most people.
* [OpenSuSE](https://www.opensuse.org/) - An alternative to Ubuntu for new users.
* [Debian](https://www.debian.org/) - A general-purpose Linux OS with a large software package repository and support community.
* [Red Hat Enterprise Linux (RHEL)](https://www.redhat.com/) - A general-purpose Linux OS supported by Red Hat, Inc. Requires purchase.
* [CentOS](https://www.centos.org/) - A community-supported version of RHEL that's free to download and use. The UCR HPCC cluster runs on CentOS 7.
* [Fedora](https://getfedora.org/) - A developer-oriented Linux OS sponsored by Red Hat.
* [Arch Linux](https://www.archlinux.org/) - A highly-customizable Linux OS for power users.

[Family tree of the GNU/Linux distributions](https://upload.wikimedia.org/wikipedia/commons/1/1b/Linux_Distribution_Timeline.svg)

## Basics

#### Command-Line Syntax for this Manual

* Remember the UNIX/Linux command line is case sensitive!
* All commands in this manual are printed in gray code boxes.
* The hash (pound) sign "#" indicates end of a command and the start of a comment.
* The notation <...> refers to variables and file names that need to be specified by the user. The symbols < and > need to be excluded.

#### Orientation

Viewing and changing the present working directory:

```bash
pwd               # "Print working directory"; show your current path

ls                # "List" contents of current directory
ls -l             # Similar to ls, but provides additional info on files and directories
ls -a             # List all files, including hidden files (.name) as well
ls -R             # Lists subdirectories recursively
ls -t             # Lists files in chronological order

cd <dir_name>     # "Change directory" to specified path
cd                # Brings you back to your home directory
cd ..             # Moves one directory up
cd ../../         # Moves two directories up (and so on)
cd -              # Go back to you were previously (before the last directory change)
```

The tilde symbol (~) gets interpreted as the path to your home directory when
by itself or at the beginning of a word:

```bash
echo ~            # View the full (complete) path of your home
find ~            # List all your files (including everything in sub-directories)
ls ~              # List the top level files of your home directory
du -sch ~/*       # Calculate the "disk usage" of files in your home
```

Viewing file info, user, and host:

```bash
stat <file-name>  # Show last modification time stamps, permissions, and size of a file

whoami            # Shows your user name (same as "echo $USER")
hostname          # Shows on which machine you are (same as "echo $HOSTNAME")
```

#### Files and directories

```bash
mkdir <dir_name>   # Creates specified directory
rmdir <dir_name>   # Removes empty directory
rm <file_name>     # Removes file_name
rm -r <dir_name>   # Removes directory including its contents, but asks for confirmation
rm -rf <dir_name>  # Same as above, but turns confirmation off. Use with caution
cp <name> <path>   # Copy file/directory as specified in path (-r to include content in directories)
mv <name1> <name2> # Renames directories or files
mv <name> <path>   # Moves file/directory as specified in path
```

#### Copy and paste

The methods differ depending where you are.

* In a **command line** environment:

  Cut last word with keyboard only  
  `Ctrl+w`  
  Press multiple times to cut more than one word

  Paste with keyboard only  
  `Ctrl+y`

* In a non-command line **desktop** environment (e.g. Firefox):

  Copy  
  `Ctrl+c`

  Paste  
  `Ctrl+v`

* **Command line** <-> **desktop** exchange:

  Copy text out of the command line and into the desktop:  
  `Shift+Ctrl+c        or        Apple+c`

  Paste text from the desktop into the command line:  
  `Shift+Ctrl+v        or        Apple+v`

* On any Linux desktop!
  * Copy with mouse only
    * Simply select the text with the mouse 
  * Paste with mouse only
    * Click the middle mouse button or both left/right buttons simultaneously

#### Handy shortcuts

* At the command prompt:
    * up(down)_key                 - scrolls through command history
    * `history` shows all commands you have used recently
* Auto Completion:
    * <something-incomplete> TAB   - completes program_path/file_name
    * Taking control over the cursor (the pointer on the command line):

```bash
Ctrl+a    # Cursor to beginning of command line
Ctrl+e    # Cursor to end of command line
Ctrl+w    # Cut last word
Ctrl+k    # Cut to the end of the line
Ctrl+y    # Paste ("yank") content that was cut earlier (by Ctrl-w or Ctrl-k)
```

* When specifying file names:
  * `.` (dot)            - refers to the present working directory
  * `~` (tilde) or `~/`  - refers to user's home directory

#### Other Useful Unix Commands

```bash
df          # disk space
free -g     # memory info in Megabytes
uname -a    # shows tech info about machine
bc          # command-line calculator (to exit type 'quit')
wget ftp://ftp.ncbi.nih.... # file download from web
/sbin/ifconfig # give IP and other network info
ln -s <original_filename> <new_filename> # creates symbolic link to file or directory
du -sh      # displays disk space usage of current directory
du -sh *    # displays disk space usage of individual files/directories
du -s * | sort -nr # shows disk space used by different directories/files sorted by size
```

#### Unix Help

```bash
help <command>  # Show help for a Bash command
man <something> # Show the manual page for a program (press the 'q' key to exit) 
man wc          # Manual on program 'word count' wc
wc --help       # Short help on wc
soap -h         # For less standard programs 
```

Online help: [Google](https://www.google.com/) is your friend.

Universally available Linux commands, with detailed examples and explanations: <https://www.linuxconfig.org/linux-commands>

## Finding files, directories and applications

```bash
find -name "*pattern*"            # Searches for *pattern* in and below current directory
find /usr/local -name "*blast*"   # Finds file names *blast* in specfied directory
find /usr/local -iname "*blast*"  # Same as above, but case insensitive
```

* Additional useful arguments: -user <user name>, -group <group name>, -ctime <number of days ago changed> 

```bash
find ~ -type f -mtime -2                # Finds all files you have modified in the last two days
locate <pattern>                        # Finds files and dirs that are written into update file
which <application_name>                # Location of application
whereis <application_name>              # Searches for executables in set of directories
yum list installed | grep <mypattern>   # Find CentOS packages and refine search with grep pattern
```

## Finding things in files

```bash
grep <pattern> <file>       # Provides lines in 'file' where pattern 'appears'
                            # If pattern is shell function use single-quotes: '>'


grep -H <pattern>           # -H prints out file name in front of pattern
grep 'pattern' <file> | wc  # pipes lines with pattern into word count wc (see chapter 8)
                            # wc arguments: -c: show only bytes, -w: show only words,
                            # -l: show only lines; help on regular expressions:
                            # $ man 7 regex or man perlre


find /home/my_dir -name '*.txt' | xargs grep -c ^.*  # Counts line numbers on many
                            # files and records each count along with individual file
                            # name; find and xargs are used to circumvent the Linux
                            # wildcard limit to apply this function on thousands of files.
```

## Ownership Overview

In Linux (and Unix systems in general), access to files and directories is
controlled by a system of owners, groups, and permission bits. Changing these
settings is necessary to control access by other users.
The permission system also affects what files can be executed.

## Ownership Levels

* **user (u)** - User ownership of a file/directory. This user has the special
right to change the permission bits and group ownership.
* **group (g)** - Group ownership of a file/directory. Members of this group may
be assigned greater access rights than non-members.
* **other (o)** - Everyone else that isn't the owning user or from the owning
group.

## Permission Bits

The elemental permissions in Linux/Unix are read, write, and execute. Users and
groups can have one many, or none of these rights. Their meanings are as follows:

|   | Letter | Number | File | Directory |
|---|---|---|---|---|
| Read | r | 4 | View the contents | View the listings |
| Write | w | 2 | Modify the contents | Create a new file, or rename or delete existing files |
| Execute | x | 1 | Execute a program/script | Traversal rights |

## Checking Permissions

Annotated output for `ls -la`:

```
---------- File type (d = directory, - = regular file, l = symlink)
|--------- User permission triplet
||  ------ Group permission triplet
||  |  --- Other permission triplet
||  |  |
||  |  |       [user] [group]
drwx-----x  61 aleong operations   4096 Feb 24 16:39 ./
drwxr-xr-x 688 root   root       262144 Feb 24 11:05 ../
drwx------   2 aleong operations   4096 Feb  2 22:45 .ssh/
drwxr-xr-x   5 aleong operations   4096 Dec 12 15:57 Downloads/
drwxr-xr-x   2 aleong operations   4096 Jan  9 16:29 bin/
-rw-------   1 aleong operations   7960 Feb 23 18:37 .bash_history
-rw-r--r--   1 aleong operations    306 Nov  3 15:08 .bashrc
-rw-r--r--   1 aleong operations    677 Apr  8  2013 .profile
-rw-r--r--   1 aleong operations    128 Nov 30 12:38 .tmux.conf
-rw-r--r--   1 aleong operations  12126 Nov  2 13:14 .vimrc
lrwxrwxrwx   1 aleong operations     23 Sep 12 10:49 bigdata -> /bigdata/operations/aleong/
-rw-r--r--   1 aleong operations   5657 Sep 19 11:31 bookmarks.html
lrwxrwxrwx   1 aleong operations     23 Sep 12 10:49 shared -> /bigdata/operations/shared/
```

Assign write and execute permissions to user and group

`chmod ug+rx my_file`

To remove all permissions from all three user groups

```bash
chmod ugo-rwx my_file
            # '+' causes the permissions selected to be added
            # '-' causes them to be removed
            # '=' causes them to be the only permissions that the file has.

chmod +rx public_html/ or $ chmod 755 public_html/ # Example for number system:
```

## Change ownership

```bash
chown <user> <file or dir>         # changes user ownership
chgrp <group> <file or dir>        # changes group ownership
chown <user>:<group> <file or dir> # changes user & group ownership
```

## Process Management

```bash
top               # view top consumers of memory and CPU (press 1 to see per-CPU statistics)
who               # Shows who is logged into system
w                 # Shows which users are logged into system and what they are doing

ps                        # Shows processes running by user
ps -e                     # Shows all processes on system; try also '-a' and '-x' arguments
ps aux | grep <user_name> # Shows all processes of one user
ps ax --tree              # Shows the child-parent hierarchy of all processes
ps -o %t -p <pid>         # Shows how long a particular process was running.
                          # (E.g. 6-04:30:50 means 6 days 4 hours ...)

Ctrl z <enter>       # Suspend (put to sleep) a process
fg                   # Resume (wake up) a suspended process and brings it into foreground
bg                   # Resume (wake up) a suspended process but keeps it running
                     # in the background.

Ctrl c               # Kills the process that is currently running in the foreground
kill <process-ID>    # Kills a specific process
kill -9 <process-ID> # NOTICE: "kill -9" is a very violent approach.
                     # It does not give the process any time to perform cleanup procedures.
kill -l                      # List all of the signals that can be sent to a proccess
kill -s SIGSTOP <process-ID> # Suspend (put to sleep) a specific process
kill -s SIGCONT <process-ID> # Resume (wake up) a specific process

nice -n <nice_value> <cmd> # Run a program with lower priority. Be nice to other headnode users.
                           # Higher "nice" values mean lower priority. Range 0-20
renice -n <priority_value> <process-ID> # Changes the priority of an existing process.
```

#### More on Terminating Processes

[DigitalOcean - How To Use ps, kill, and nice to Manage Processes in Linux](https://www.digitalocean.com/community/tutorials/how-to-use-ps-kill-and-nice-to-manage-processes-in-linux)

## Vim Manual

#### Basics

`vim <my_file_name> # open/create file with vim`

Once you are in Vim the most important commands are `i` ,  `:`  and `ESC`. The `i` key brings you into the insert mode for typing. `ESC` brings you out of there. And the `:` key starts the command mode at the bottom of the screen. In the following text, all commands starting with `:` need to be typed in the command mode. All other commands are typed in the normal mode after hitting the `ESC` key.

**Modifier Keys to Control Vim**

```bash
i   # INSERT MODE
ESC # NORMAL (NON-EDITING) MODE
:   # commands start with ':'
:w  # save command; if you are in editing mode you have to hit ESC first!!
:q  # quit file, don't save
:q! # exits WITHOUT saving any changes you have made
:wq # save and quit
R   # replace MODE
r   # replace only one character under cursor
q:  # history of commands (from NORMAL MODE!), to reexecute one of them, select and hit enter!
:w new_filename     # saves into new file
:#,#w new_filename  # saves specific lines (#,#) to new file
:#                  # go to specified line number
```

#### Vim Help

* **Online Help**
  * Find help on the web. Google will find answers to most questions on **vi** and **vim** (try searching for both terms).
  * [Purdue University Vi Tutorial](https://engineering.purdue.edu/ECN/Support/KB/Docs/ViTextEditorTutorial)
  * Animated Vim Tutorial: https://linuxconfig.org/vim-tutorial
  * Useful list of vim commands:
    * [Vim Commands Cheat Sheet](http://www.fprintf.net/vimCheatSheet.html)
    * [VimCard](http://tnerual.eriogerg.free.fr/vimqrc.pdf)

* **Help from Command Line**
> `vimtutor # open vim tutorial from shell`

* **Help in Vim**
```bash
:help                # opens help within vim, hit :q to get back to your file
:help <topic>        # opens help on specified topic
:help_topic| CTRL-]  # when you are in help this command opens help topic specified between |...|,
                     # CTRL-t brings you back to last topic
:help <topic> CTRL-D # gives list of help topics that contain key word
: <up-down keys>     # like in shell you get recent commands!!!!
```

* **Moving Around in Files**

```bash
$        # moves cursor to end of line
A        # same as $, but switches to insert mode
0 (zero) # moves cursor to beginning of line
CTRL-g   # shows at status line filename and the line you are on
SHIFT-G  # brings you to bottom of file, type line number (isn't displayed) then SHIFT-G # brings you to specified line#
```

* **Line Wrapping and Line Numbers**

```bash
:set nowrap # no word wrapping, :set wrap # back to wrapping
:set number # shows line numbers, :set nonumber # back to no-number mode
```

* **Working with Many Files & Splitting Windows**

```bash
vim -o *.txt # opens many files at once and displays them with horizontal
             # split, '-O' does vertical split
vim *.txt    # opens many files at once; ':n' switches between files
```

```bash
:wall or :qall # write or quit all open files
:args *.txt    # places all the relevant files in the argument list
:all           # splits all files in the argument list (buffer) horizontally
CTRL-w         # switch between windows
:split         # shows same file in two windows
:split <file-to-open> # opens second file in new window
:vsplit        # splits windows vertically, very useful for tables, ":set scrollbind" let's you scroll all open windows simultaneously
:close         # closes current window
:only          # closes all windows except current one
```

* **Spell Checking & Dictionary**

```bash
:set spell # turns on spell checking
:set nospell # turns spell checking off
:! dict <word> # meaning of word
:! wn 'word' -over # synonyms of word
```

* **Enabling Syntax Highlighting**

```bash
:set filetype=perl # Turns on syntax coloring for a chosen programming language.
:set syntax on # Turns syntax highlighting on
:set syntax off # Turns syntax highlighting off
```

* **Undo and Redo**

```bash
u      # undo last command
U      # undo all changes on current line
CTRL-R # redo one change which was undone
```

* **Deleting Things**

```bash
x   # deletes what is under cursor
dw  # deletes from curser to end of word including the space
de  # deletes from curser to end of word NOT including the space
cw  # deletes rest of word and lets you then insert, hit ESC to continue with NORMAL mode
c$  # deletes rest of line and lets you then insert, hit ESC to continue with with NORMAL mode
d$  # deletes from cursor to the end of the line
dd  # deletes entire line
2dd # deletes next two lines, continues: 3dd, 4dd and so on.
```

* **Copy & Paste**

```bash
yy # copies line, for copying several lines do 2yy, 3yy and so on
p # pastes clipboard behind cursor
```

* **Search in Files**

```bash
/my_pattern # searches for my_pattern downwards, type n for next match
?my_pattern # seraches for my_pattern upwards, type n for next match
:set ic # switches to ignore case search (case insensitive)
:set hls # switches to highlight search (highlights search hits)
```

* **Replacements with Regular Expression Support**

Great intro: [A Tao of Regular Expressions](http://www.scootersoftware.com/RegEx.html)

```bash
:s/old_pat/new_pat/  # replaces first occurrence in a line
:s/old_pat/new_pat/g  # replaces all occurrence in a line
:s/old_pat/new_pat/gc  # add 'c' to ask for confirmation
:#,#s/old_pat/new_pat/g  # replaces all occurrence between line numbers: #,#
:%s/old_pat/new_pat/g  # replaces all occurrence in file
:%s/\(pattern1\)\(pattern2\)/\1test\2/g  # regular expression to insert, you need here '\' in front of parentheses (<# Perl)
:%s/\(pattern.*\)/\1 my_tag/g  # appends something to line containing pattern (<# .+ from Perl is .* in VIM)
:%s/\(pattern\)\(.*\)/\1/g  # removes everything in lines after pattern
:%s/\(At\dg\d\d\d\d\d\.\d\)\(.*\)/\1\t\2/g  # inserts tabs between At1g12345.1 and Description
:%s/\n/new_pattern/g  # replaces return signs
:%s/pattern/\r/g  # replace pattern with return signs!!
:%s/\(\n\)/\1\1/g  # insert additional return signs
:%s/\(^At\dg\d\d\d\d\d.\d\t.\{-}\t.\{-}\t.\{-}\t.\{-}\t\).\{-}\t/\1/g  # replaces content between 5th and 6th tab (5th column), '{-}' turns off 'greedy' behavior
:#,#s/\( \{-} \|\.\|\n\)/\1/g  # performs simple word count in specified range of text
:%s/\(E\{6,\}\)/<font color="green">\1<\/font>/g  # highlight pattern in html colors, here highlighting of >= 6 occurences of Es
:%s/\([A-Z]\)/\l\1/g  # change uppercase to lowercase, '%s/\([A-Z]\)/\u\1/g' does the opposite
:g/my_pattern/ s/\([A-Z]\)/\l\1/g | copy $  # uses 'global' command to apply replace function only on those lines that match a certain pattern. The 'copy $' command after the pipe '|' prints all matching lines at the end of the file.
:args *.txt | all | argdo %s/\old_pat/new_pat/ge | update  # command 'args' places all relevant files in the argument list (buffer); 'all' displays each file in separate split window; command 'argdo' applies replacement to all files in argument list (buffer); flag 'e' is necessary to avoid stop at error messages for files with no matches; command 'update' saves all changes to files that were updated.
```

* **Useful Utilities in Vim**

  * Matching Parentheses
    * Place cursor on (, [ or { and type % # cursor moves to matching parentheses

  * Printing and Inserting Files
    * `:ha  # prints entire file`
    * `:#,#ha  # prints specified lines: #,#`
    * `:r <filename>  # inserts content of specified file after cursor`

  * Convert Text File to HTML Format
    * `:runtime! syntax/2html.vim  # run this command with open file in Vim`

  * Shell Commands in Vim
    * `:!<SHELL_COMMAND> <ENTER>  # executes any shell command, hit <enter> to return`
    * `:sh  # switches window to shell, 'exit' switches back to vim`

  * Using Vim as Table Editor
    * `v` starts visual mode for selecting characters
    * `V` starts visual mode for selecting lines`
    * `CTRL-V` starts visual mode for selecting blocks (use CTRL-q in gVim under Windows). This allows column-wise selections and operations like inserting and deleting columns. To restrict substitute commands to a column, one can select it and switch to the command-line by typing `:`. After this the substitution syntax for a selected block looks like this: `'<,'>s///.`
    * `:set scrollbind` starts simultaneous scrolling of 'vsplitted' files. To set to horizontal binding of files, use command `:set scrollopt=hor` (after first one). Run all these commands before the `:split` command.
    * `:AlignCtrl I= \t then :%Align` This allows to align tables by column separators (here '\t') when the [Align utility from Charles Campbell's](http://vim.sourceforge.net/scripts/script.php?script_id=294) is installed. To sort table rows by selected lines or block, perform the visual select and then hit F3 key. The rest is interactive. To enable this function, one has to include in the `.vimrc` file the [Vim sort script](https://cluster.hpcc.ucr.edu/%7Etgirke/Documents/UNIX/vim/vim_sort_fct.txt) from Gerald Lai.

* **Modify Vim Settings**

The default settings in Vim are controlled by the `.vimrc` file in your home directory.

  * see last chapter of vimtutor (start from shell)
  * useful [.vimrc sample](http://phuzz.org/vimrc.html)
  * when vim starts to respond very slowly then one may need to delete the `.viminf*` files in home directory

## Text Viewing

```bash
more <my_file>  # views text, use space bar to browse, hit 'q' to exit
less <my_file>  # a more versatile text viewer than 'more', 'q' exits, 'G' moves to end of text,
                # 'g' to beginning, '/' find forward, '?' find backwards
cat  <my_file>  # concatenates files and prints content to standard output
```

## Text Editors

* **Vi** and **Vim**
  * Non-graphical (terminal-based) editor. Vi is guaranteed to be available on any system. Vim is the improved version of vi.
* **Emacs**
  * Non-graphical or window-based editor. You still need to know keystroke commands to use it. Installed on all Linux distributions and on most other Unix systems.
* **XEmacs**
  * More sophisticated version of emacs, but usually not installed by default. All common commands are available from menus. Very powerful editor, with built-in syntax checking, Web-browsing, news-reading, manual-page browsing, etc.
* **Pico**
  * Simple terminal-based editor available on most versions of Unix. Uses keystroke commands, but they are listed in logical fashion at bottom of screen.
* **Nano**
  * A simple terminal-based editor which is default on modern Debian systems.


## The Unix Shell
When you log into UNIX/LINUX system, then is starts a program called the Shell. It provides you with a working environment and interface to the operating system. Usually there are several different shell programs installed. The shell program bash is one of the most common ones.

```bash
finger <user_name> # shows which shell you are using
chsh -l # gives list of shell programs available on your system (does not work on all UNIX variants)
<shell_name> # switches to different shell
```

#### STDIN, STDOUT, STDERR, Redirections, and Wildcards

See [LINUX HOWTOs](http://www.tldp.org/HOWTO/Bash-Prog-Intro-HOWTO-3.html)


By default, UNIX commands read from standard input (STDIN) and send their output to standard out (STDOUT).

You can redirect them by using the following commands:

```bash
<beginning-of-filename>*         # * is wildcard to specify many files
ls > file                        # prints ls output into specified file
command < my_file                # uses file after '<' as STDIN
command >> my_file               # appends output of one command to file
command | tee my_file            # writes STDOUT to file and prints it to screen
command > my_file; cat my_file   # writes STDOUT to file and prints it to screen
command > /dev/null              # turns off progress info of applications by redirecting
                                 # their output to /dev/null
grep my_pattern my_file | wc     # Pipes (|) output of 'grep' into 'wc'
grep my_pattern my_non_existing_file 2 > my_stderr # prints STDERR to file
```

#### Useful shell commands

```bash
cat <file1> <file2> > <cat.out>      # concatenate files in output file 'cat.out'
paste <file1> <file2> > <paste.out>  # merges lines of files and separates them by tabs (useful for tables)
cmp <file1> <file2>                  # tells you whether two files are identical
diff <fileA> <fileB>                 # finds differences between two files
head -<number> <file>                # prints first lines of a file
tail -<number> <file>                # prints last lines of a file
split -l <number> <file>             # splits lines of file into many smaller ones
csplit -f out fasta_batch "%^>%" "/^>/" "{*}" # splits fasta batch file into many files
                                     # at '>'
sort <file>                          # sorts single file, many files and can merge (-m)
                                     # them, -b ignores leading white space, ...
sort -k 2,2 -k 3,3n input_file > output_file # sorts in table column 2 alphabetically and
                                     # column 3 numerically, '-k' for column, '-n' for
                                     # numeric
sort input_file | uniq > output_file # uniq command removes duplicates and creates file/table
                                     # with unique lines/fields
join -1 1 -2 1 <table1> <table2>     # joins two tables based on specified column numbers
                                     # (-1 file1, 1: col1; -2: file2, col2). It assumes
                                     # that join fields are sorted. If that is not the case,
                                     # use the next command:
sort table1 > table1a; sort table2 > table2a; join -a 1 -t "$(echo -e '\t')" table1a table2a > table3                               # '-a <table>' prints all lines of specified table!
                                     # Default prints only all lines the two tables have in
                                     # common. '-t "$(echo -e '\t')" ->' forces join to
                                     # use tabs as field separator in its output. Default is
                                     # space(s)!!!
cat my_table | cut -d , -f1-3        # cut command prints only specified sections of a table,
                                     # -d specifies here comma as column separator (tab is
                                     # default), -f specifies column numbers.
grep                                 # see chapter 4
egrep                                # see chapter 4
```

## Screen

Screen references

1. [Screen Turorial](http://fosswire.com/post/2008/08/video-tutorial-getting-started-with-gnu-screen/)
2. [Screen Cheat Sheet](http://aperiodic.net/screen/quick_reference)

#### Starting a New Screen Session

```bash
screen                 # Start a new session
screen -S <some-name>  # Start a new session and gives it a name
```

Commands to Control Screen

```bash
Ctrl-a d #  Detach from the screen session
Ctrl-a c # Create a new window inside the screen session
Ctrl-a Space # Switch to the next window
Ctrl-a a # Switch to the window that you were previously on
Ctrl-a " # List all open windows. Double-quotes " are typed with the Shift key
Ctrl-d or type exit # Exit out of the current window. Exiting form the last window will end the screen session
Ctrl-a [ # Enters the scrolling mode. Use Page Up and Page Down keys to scroll through the window. Hit the Enter key twice to return to normal mode. 
```

#### Attaching to Screen Sessions

From any computer, you can attach to a screen session after SSH-ing into a server.

```bash
screen -r              # Attaches to an existing session, if there is only one
screen -r              # Lists available sessions and their names, if there are more then one session running
screen -r <some-name>  # Attaches to a specific session
screen -r <first-few-letters-of-name> # Type just the first few letters of the name
                       # and you will be attached to the session you need
```

#### Destroying Screen Sessions

1. Terminate all programs that are running in the screen session. The standard way to do that is: `Ctrl-c`
2. Exit out of your shell: `exit`
3. Repeat steps 1 and 2 until you see the message: `[screen is terminating]`

There may be programs running in different windows of the same screen session. That's why you may need to terminate programs and exit shells multiple time.

#### Tabs and a Reasonably Large History Buffer

For a better experience with screen, run

```bash
cp ~/.screenrc ~/.screenrc.backup 2> /dev/null
echo 'startup_message off
defscrollback 10240
caption always "%{=b dy}{ %{= dm}%H %{=b dy}}%={ %?%{= dc}%-Lw%?%{+b dy}(%{-b r}%n:%t%{+b dy})%?(%u)%?%{-dc}%?%{= dc}%+Lw%? %{=b dy}}"
' > ~/.screenrc
```
## Simple One-Liner Shell Scripts

Web page for [script download](http://linuxcommand.org/script_library.php).

Renames many files *.old to *.new. To test things first, replace 'do mv' with 'do echo mv':

```bash
for i in *.input; do mv $i ${i/\.old/\.new}; done
for i in *\ *; do mv "$i" "${i// /_}"; done # Replaces spaces in files by underscores
```

Run an application in loops on many input files:

```bash
for i in *.input; do ./application $i; done
```

Run fastacmd from BLAST program in loops on many *.input files and create corresponding *.out files:

```bash
for i in *.input; do fastacmd -d /data/../database_name -i $i > $i.out; done
```

Run SAM's target99 on many input files:

```bash
for i in *.pep; do target99 -db /usr/../database_name -seed $i -out $i; done
Search in many files for a pattern and print occurrences together with file names.
for j in 0 1 2 3 4 5 6 7 8 9; do grep -iH <my_pattern> *$j.seq; done
```

Example of how to run an interactive application (tmpred) that asks for file name input/output:

```bash
for i in *.pep; do echo -e "$i\n\n17\n33\n\n\n" | ./tmpred $i > $i.out; done
```

Run BLAST2 for all *.fasa1/*.fasta2 file pairs in the order specified by file names and write results into one file:

```bash
for i in *.fasta1; do blast2 -p blastp -i $i -j ${i/_*fasta1/_*fasta2} >> my_out_file; done
```
    This example uses two variables in a for loop. The content of the second variable gets specified in each loop by a replace function.

Runs BLAST2 in all-against-all mode and writes results into one file ('-F F' turns low-complexity filter off):

```bash
for i in *.fasta; do for j in *.fasta; do blast2 -p blastp -F F -i $i -j $j >> my_out_file; done; done;
```

## Simple One-Liner Perl Scripts

*Small collection of useful one-liners:*

```perl
perl -p -i -w -e 's/pattern1/pattern2/g' my_input_file
            # Replaces a pattern in a file by a another pattern using regular expressions.
            # $1 or \1: back-references to pattern placed in parentheses
            # -p: lets perl know to write program
            # -i.bak: creates backup file *.bak, only -i doesn't
            # -w: turns on warnings
            # -e: executable code follows
```

*Parse lines based on patterns:*

```perl
perl -ne 'print if (/my_pattern1/ ? ($c=1) : (--$c > 0)); print if (/my_pattern2/ ? ($d = 1) : (--$d > 0))' my_infile > my_outfile
            # Parses lines that contain pattern1 and pattern2.
            # The following lines after the pattern can be specified in '$c=1' and '$d=1'.
            # For logical OR use this syntax: '/(pattern1|pattern2)/'.
```

## SSH

SSH is the main protocal used to communicate with or establish a terminal on remote systems.

#### Example

```bash
ssh <username>@<remote-address>
```

If you want to use a specific ssh key for this connection use the `-i` flag.

```bash
ssh -i ~/.ssh/mykey <username>@<remote-address>
```

If your username on the remote system is the same as your local username then you can omit the username from the command.

#### Passwordless login.

You can use ssh (RSA) keys to login with out the need for a password. (this is much quicker and very useful)

* Generate your ssh keys `ssh-keygen`
  * Enter a blank password when asked in order to have a passwordless key
  * By default the key will be stored as `.ssh/id_rsa`
    * You can change this path to which ever you like
* Copy the public key to the remote system (there is now a tool for this)
  * `ssh-copy-id <username>@<remote-system>`
  * You can pass the `-i` flag to this command to copy a specific public key.
* This places your public key into the the file `.ssh/authorized_keys` on the remote system.
  * You can do this manually if you like.

Once this is complete you can login without a password.

#### SSH remote commands

Often you may want to just run a command on the remote system.

Just put the command or muplitple commands surounded by quotes after the ssh command 

```bash
ssh <username>@<remote-host> <command-you-want-to-run>

ssh <username>@<remote-host> "<command-you-want-to-run>; <command-you-want-to-run>; <command-you-want-to-run>"
```

## Remote Copy: wget, scp, ncftp

#### Wget

Use wget to download a file from the web:

```bash
wget ftp://ftp.ncbi.nih.... # file download from www; add option '-r' to download entire directories
```

#### SCP

Use scp to copy files between machines (ie. laptop to server):

```bash
scp source target # Use form 'userid@machine_name' if your local and remote user ids are different.
                  # If they are the same you can use only 'machine_name'.
```

Here are more scp examples:

```bash
scp user@remote_host:file.name . # Copies file from server to local machine (type from local
                                 # machine prompt). The '.' copies to pwd, you can specify                                              # here any directory, use wildcards to copy many files.

scp file.name user@remote_host:~/dir/newfile.name
                                                                       # Copies file from local machine to server.
                              
scp -r user@remote_host:directory/ ~/dir
                                 # Copies entire directory from server to local machine.
```

#### Nice FTP

From the linux command line run ncftp and use it to get files:

```bash
ncftp
ncftp> open ftp.ncbi.nih.gov
ncftp> cd /blast/executables
ncftp> get blast.linux.tar.Z (skip extension: @)
ncftp> bye
```

## Archiving and Compressing

#### Creating Archives

```bash
tar -cvf my_file.tar mydir/    # Builds tar archive of files or directories. For directories, execute command in parent directory. Don't use absolute path.    
tar -czvf my_file.tgz mydir/   # Builds tar archive with compression of files or directories. For
                               # directories, execute command in parent directory. Don't use absolute path.
zip -r mydir.zip mydir/        # Command to archive a directory (here mydir) with zip.
tar -jcvf mydir.tar.bz2 mydir/ # Creates *.tar.bz2 archive
```

#### Viewing Archives

```bash
tar -tvf my_file.tar
tar -tzvf my_file.tgz
```

#### Extracting Archives

```bash
tar -xvf my_file.tar
tar -xzvf my_file.tgz
gunzip my_file.tar.gz # or unzip my_file.zip, uncompress my_file.Z,
                      # or bunzip2 for file.tar.bz2
find -name '*.zip' | xargs -n 1 unzip # this command usually works for unzipping
                      # many files that were compressed under Windows
tar -jxvf mydir.tar.bz2 # Extracts *.tar.bz2 archive
```

Try also:

```bash
tar zxf blast.linux.tar.Z
tar xvzf file.tgz
```

Important options:

```bash
f: use archive file
p: preserve permissions
v: list files processed
x: exclude files listed in FILE
z: filter the archive through gzip 
```

## Environment Variables

```bash
xhost user@host                # adds X permissions for user on server.
echo $DISPLAY                  # shows current display settings
export DISPLAY=<local_IP>:0    # change environment variable
unsetenv DISPLAY               # removes display variable
env                            # prints all environment variables
```

List of directories that the shell will search when you type a command:

```bash
echo $PATH
```
You can edit your default DISPLAY setting for your account by adding it to file .bash_profile
