# Learning The Shell

## Operators

### $()

Subshell, not actually a command but more of syntax, allows you to run commands then inject the output of the command into another thing (Refer examples below)

### > or <

Redirecting stream to, only applies to stdout

### >> or <<

Appends rather than write to file when compared to single variants above

## fzf

### Useful flags

1. `--query \'celeste.zip`
2. `--query "'celeste.zip | 'celeste-osx"` : query with or `|`
3. `-preview 'cat {}'`

### Command related specific syntax

1. 'celeste.zip: `'` searches exact match
2. 'celeste.zip | 'celeste-osx : or is `|`

Piping output from fzf to cat/bat

    cat $(fzf)
    bat $(fzf)

## ls/eza

Pipe ls to grep/ripgrep to find specific file

    ls -l | grep "Desktop"
    ls -l | rg "Desktop"
    eza -l | rg "Desktop"

## find/fd

Pipe find/fd to grep/ripgrep to filter not include or include, you can chain them apparently

    find . -iname pubspec.yaml | rg -v 'Dart-Code' | rg -v 'packages'| rg -v '/flutter'
    find . -iname pubspec.yaml | grep -v 'Dart-Code' | grep -v 'packages'| grep -v '/flutter'
    fd 'pubspec.yaml' | rg -v 'Dart-Code' | rg -v 'packages'| rg -v '/flutter'

## tr

### Useful flags

1. `-d "stuff"` : delete "stuff"
2. `-cd "stuff"` : delete everything except stuff
3. `-s " "` : truncate repeating " " to single

### Command related specific syntax

1. [:lower:] : lower case
2. [:upper:] : upper case
3. [:space:]
4. [:digit:]

Replace stuff in file, some extra options:

    cat README.md | tr "[:lower:]" "[:upper:]"
    tr "[:lower:]" "[:upper:]" <README.md

Pipe stuff from and into new file

    tr "[:lower:]" "[:upper:]" <README.md >newStuff.txt

## mv

### Useful Flags

1. `-b` : take a backup of an existing file that will be ovewritten
2. `-i` : ask for confirmation before overwriting
3. `-n` : prevents existing file from being overwritten

Move multiple files at once

    mv file1 file2 file3 /path/to/destination

## cp

### Useful Flags

1. `-b` : take a backup of an existing file that will be ovewritten
2. `-i` : ask for confirmation before overwriting
3. `-r` : use this if want copy directory

Copy multiple files at once

    cp file1 file2 file3 /path/to/destination

## unzip

### Useful Flags

1. `-d destination_folder` : unzip to particular folder
2. `-l path/to/archive.zip` : list without unzip

Unzip to same folder as the zip name

    unzip file.zip

## git

View diff for file

    git diff filename.txt

View diff for staged file

    git diff --staged filename.txt

List worktree

    git worktree list

Remove worktree

    git worktree remove worktree_folder

Add worktree for existing branch

    git worktree add destination_folder branch_to_checkout

Add worktree for non existing branch

    git worktree add destination_folder -b branch_to_checkout

Git blame see when the change was introduced (merged) into the branch instead of when the change was made (default behaviourw will ignore merge commits)

    git blame --first-parent file

Git draw graph

    git log --graph --oneline --all
    git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cd) %C(bold blue)<%an>%Creset%n' --abbrev-commit --date=local --branches

## mount

Mounting an smb folder to a folder

    mount -t smbfs //userneame@server_ip/folder ./mount_folder

## history

Search through history with grep

    history 50 | grep brew

## tput

Clear the whole terminal including scrollback buffer

    tput reset

## ln

### Useful flags

1. `-s`: creates a symlink, without this it creates a hard link

Create a symlink

    ln -s /path/to/file_or_directory path/to/symlin

Ovewrite existing symlink

    ln -sf /path/to/new_file path/to/symlink

## tail

Displays the last part of the file

### Useful flags

1. `-f`: Keep reading the last few lines of the file until a \<C-c\> is pressed

Follow the changes in a log file

    tail -f logfile

## sed

Edit text in a scriptable manner (similar to `:s` in vim)

Pipe from a command and edit the output

    command | sed 's/apple/mango/g'

## xargs

Execute a command with piped arguments coming from another command, a file, etc.
The input is treated as a single block of text and split into separate pieces on spaces, tabs, newlines and end-of-file.

 Run a command using the input data as arguments

    arguments_source | xargs comman
