# Learning The Shell

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
