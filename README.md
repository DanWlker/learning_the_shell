# Learning The Shell

## Operators

### $()

Subshell, not actually a command but more of syntax, allows you to run commands then inject the output of the command into another thing (Refer examples below)

### > or <

Redirecting stream to, only applies to stdout

### >> or <<

Appends rather than write to file when compared to single variants above

## grep

### Useful flags

1. `-i`: Case insensitive
2. `-v`: Returns the reverse of what you searching (what didn't match)

Grep from one file only

    grep searchString fileName.txt

## ripgrep

### Useful flags

1. `-s`: Case sensitive
2. `-v`: Returns the reverse of what you searching (what didn't match)

## fzf

### Useful flags

1. `--query \'celeste.zip`
2. `--query "'celeste.zip | 'celeste-osx"`: query with or `|`
3. `--preview 'cat {}'`

### Command specific syntax

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

## find

### Useful flags

1. `-iname`: Insensitive name
2. `-name`: Case sensitive

Pipe find to grep/ripgrep to filter not include or include, you can chain them apparently

    find . -iname pubspec.yaml | rg -v 'Dart-Code' | rg -v 'packages'| rg -v '/flutter'
    find . -iname pubspec.yaml | grep -v 'Dart-Code' | grep -v 'packages'| grep -v '/flutter'
    fd 'pubspec.yaml' | rg -v 'Dart-Code' | rg -v 'packages'| rg -v '/flutter'

## fd

### Useful flags

1. `-e`: Find files with a specific extension
2. `-s`: Case sensitive
3. `-u`: include ignored and hidden files in the search, alias for --hidden and --no-ignore
4. `-t {type}`: Specify the type of thing to look for, file, directory etc.

## sed (gnu sed, not  the one in mac)

Edit text in a scriptable manner (similar to `:s` in vim)

### Useful flags

1. `-i`: Does the replacement for the file in place

Pipe from a command and edit the output

    command | sed 's/apple/mango/g'

Replace and write to file, don't use `>`, it will result in blank file

    sed -i 's/apple/mango/g' pubspec.yaml

## mv

### Useful flags

1. `-f`: Do not prompt for confirmation before overwriting
2. `-i` : ask for confirmation before overwriting
3. `-n` : prevents existing file from being overwritten

Move multiple files at once

    mv file1 file2 file3 /path/to/destination

## cp

### Useful flags

1. `-i` : ask for confirmation before overwriting
2. `-R` : use this if want copy directory

Copy multiple files at once

    cp file1 file2 file3 /path/to/destination

## unzip

### Useful flags

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

## jq

[Read this for guide](https://www.baeldung.com/linux/jq-command-json)

### Useful flags

1. `-c`: Compact output
2. `-r`: Raw output, useful to pipe to other stuff

### Command specific syntax

1. `.var`, `.` `.field.innerfield`
2. `keys`: get the keys instead of the values
3. `length`: get the length of a json array
4. `|`
5. `has("keyName")`
6. `map(.field)`: always ouputs an arrya, can be combined with has inside the ()
7. `min`, `max`
8. `select()`: selects the objects that fullfill the condition
9. `del()`: similar to select but deletes the keys from the json

Get json from clipboard and display it with colors

    pbpaste | jq

Get json from clipboard, minifiy it `-c`, and copy it back to clipboard

    pbpaste | jq -c | pbcopy

Get specific field from json, use `[n]` to get the nth element if its a list, leave it empty `[]` to get the whole list

    cat filename.json | jq '.field.field[2]'

Create an array from the items and pipe to min

    jq '[.[].price] | min' < temp.json

Select items based on condition

    jq '.[] | select(.color=="yellow" and .price>=0.5)' fruits.json

## yq

### Useful flags

1. `-Poy`: Convert json to yaml

Read from a file

    cat pubspec.yaml | yq
    yq eval pubspec.yaml

Get yaml from clipboard

    pbpaste | yq

Get specific field name from yaml

    cat pubspec.yaml | yq '.dependencies.libgit2dart'

Convert json to yaml

    pbpaste | yq -Poy
    yq -Poy thing.json

## xargs

Execute a command with piped arguments coming from another command, a file, etc.
The input is treated as a single block of text and split into separate pieces on spaces, tabs, newlines and end-of-file.

 Run a command using the input data as arguments

    arguments_source | xargs comman

## awk

Print the fifth column (a.k.a. field) in a space-separated file:

    awk '{print $5}' path/to/file

Print with comparison

    awk '$2 >= 300 {print $1}' data.txt

## cut

 Print a field range of each line with a specific delimiter:

    command | cut -d "," -f 1
