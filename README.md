# Learning The Shell

## Permission

10 character string, ex drwxrwxrwx

1st char
`d` or `-`, d means directory, `-` means file

`r`, `w`, `x` can be present or `-` (which means permission is not granted)

2, 3, 4 applies to the owner (user that created the directory)

5, 6, 7 applies to group

8, 9, 10 applies to everyone else

Ex.

-rwxrwxrwx: A file where everyone can do everything

-rwxr-xr-x: A file where everyone can read and execute, but only the owner can write

drwxr-xr-x: A directory where everyone can read (ls the contents) and execute (cd into it), but only the owner can write (modify the contents)

drwx------: A directory where only the owner can read, write and execute

## Operators

### $()

Subshell, not actually a command but more of syntax, allows you to run commands then inject the output of the command into another thing (Refer examples below)

### >

Redirect stream to, redirects stdout

### 2>

Redirect stream to, redirects stderr

### >>

Appends rather than write to file when compared to single variants above

### --

To signify the end of command options, after which only positional ("non-option") arguments are accepted.[1](https://stackoverflow.com/a/2427987) [2](https://unix.stackexchange.com/questions/11376/what-does-double-dash-double-hyphen-mean/11382#11382)

Ex.

    grep -- -t

### \ 

1. Before command ex. `\mv`
    - [To avoid shell aliases](https://www.linuxquestions.org/questions/linux-newbie-8/use-of-%5C-backslash-in-commands-4175427927/)

1. Escape charecter ex. `mv file\ name.txt`
    - To escape spaces

3. New line when at end of the command

### $?

This will give you the last exit code

### $VISUAL or $EDITOR

[README, in short, check for $VISUAL first, then check for $EDITOR, this applies more if you're writing cli apps](https://unix.stackexchange.com/questions/4859/visual-vs-editor-what-s-the-difference)

### IO Redirection

[IO Redirection Explained](https://www.youtube.com/watch?v=7VaO2dxs_kg)

[Advanced IO Redirection](https://www.instagram.com/reel/C9Fht23vTPi/)

[How pipes work](https://stackoverflow.com/questions/9834086/what-is-a-simple-explanation-for-how-pipes-work-in-bash)

## sudo

Rerun the previous command with sudo

    sudo !!

## mkdir

### Useful flags

1. `-p`: Create intermediate directories as required

Make multiple directories and subdirectories at once

    mkdir -p ./home/{folder_a, folder_b}/{folder_x, folder_y, folder_z}

## touch

Make multiple files with similar names at once

    touch file{1..10}.txt

## kill

### Useful flags (refer explanations below)

1. `-9`
2. `-2`
3. `-17`
4. `-1`

Terminate a program

    kill PID

Terminate a program using the SIGHUP (hang up) signal. Many daemons will reload instead of terminating:

    kill -1|HUP process_id

Terminate a program using the SIGINT (interrupt) signal. This is typically initiated by the user pressing `Ctrl + C`:

    kill -2|INT process_id

Signal the operating system to immediately terminate a program (which gets no chance to capture the signal):

    kill -9|KILL process_id

Signal the operating system to pause a program until a SIGCONT ("continue") signal is received:

    kill -17|STOP process_id

## chmod

change the permissions of any file or directory that you own

### Useful flags

1. `-R`: recursive, do this to all the contents of the directory as well
1. `-x`: removes the executable permission from file
1. `+x`: adds the executable permission to file

### Command specific syntax

1. `u=`: user
1. `g=`: group
1. `o=`: others

Changing permissions recursively in a directory

    chmod -R u=rwx,g=,o= DIRECTORY

Remove executable permission

    chmod -x file.sh

## chown

1. `-R`: recursive, do this to all the contents of the directory as well

Change owner of entire directory recursively to root

    sudo chown -R root DIRECTORY

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

`**` to fuzzy find from current location 

    cd ~/Documents/**
    kill -9 **

## cd/zoxide

Return to last location

    cd -

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

Replace and write to file, don't use `>`, it will result in blank file (only applies if you are reading and writing to the same file, you can use `>` and `<` for different files)

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

## rsync

### Useful flags

1. `-P`: Check progress for individual files
1. `--info=progress2 --info=name0`: Check progress as a whole
1. `-a`: Archive mode, use this if you are making backups
1. `-z`: Whether to compress when sending
1. `--delete`: Delete at destination if origin does not exist (usually for backups)
1. `--remove-source-files`: Remove source files after deletion
1. `-r`: Copy dir recursively
1. `--dry-run`: Test run before actually copying
1. `-v`: Verbose, use it with dry run to check if dry run works 

### Useful tutorials

1. [How to Use the rsync Command to Transfer Files (Linux Crash Course Series)](https://www.youtube.com/watch?v=KG78O53u8rY)
1. [Linux/Mac Terminal Tutorial: How To Use The rsync Command - Sync Files Locally and Remotely](https://www.youtube.com/watch?v=qE77MbDnljA)

Rsync to a folder in a remote server (can be sync from server to local as well by reversing the `username@ip...` with `origin/`)

    rsync -rv --dry-run origin/ username@ip.address.1.128:/dest/dest_folder

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
6. `map(.field)`: always ouputs an array, can be combined with has inside the ()
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

### Useful flags

1. `-I`: Specifies a placeholder
1. `-n`: Number of args for the program input
1. `-P`: Number of processes per time

Execute a command with piped arguments coming from another command, a file, etc.
The input is treated as a single block of text and split into separate pieces on spaces, tabs, newlines and end-of-file.

### Useful tutorials

1. [Xargs Should Be In Your Command Line Toolbag](https://www.youtube.com/watch?v=rp7jLi_kgPg)

1. [Xargs Explained](https://www.youtube.com/watch?v=HK1wAV9x4-A)

Run a command using the input data as arguments

    arguments_source | xargs command

How to use `-I`

    ls | xargs -I {} echo "/home/{}"
    ls | cut -d '.' -f1 | xargs -I {} mv {}.txt {}.text

How to use `-n`

    seq 5 | xargs -n 1 -P 1 bash -c 'echo $0; sleep 1'
    seq 5 | xargs -n 2 -P 2

How to use `-P`

    seq 5 | xargs -n 1 -P 2 bash -c 'echo $0; sleep 1'

Launch new shell to run commands

    cat hostnames | xargs -I{} P4 sh -c "host -t A {} 8.8.8.8 | tail -n1"

## awk

Print the fifth column (a.k.a. field) in a space-separated file:

    awk '{print $5}' path/to/file

Print with comparison

    awk '$2 >= 300 {print $1}' data.txt

## cut (Use Awk if possible)

### Useful tutorials

[Two Powerful Command Line Utilities 'cut' And 'tr'](https://www.youtube.com/watch?v=_0IFtMFYroU)

 Print a field range of each line with a specific delimiter:

    command | cut -d "," -f 1

## tr

### Useful flags

1. `-s`: Squeeze repeating characters
1. `-d`: Delete
1. `-c`: Complement (usually with `-d`, delete everytheng except)

### Command specific syntax

1. `[:lower:]` 
1. `[:upper:]`
1. `[:digit:]`

Squeeze repeating characters

    echo "abc   def" | tr -s ' ' ':'

Change lower to uppercase

    echo "abc   def" | tr -s '[:lower:]' '[:upper:]'

## ps (weird abit on macos)

### Useful flags

1. `-H`: This is not working on macos, use `pstree`
1. `-j`: Print information associated with the following keywords: user, pid, ppid, pgid, sess, jobc, state, tt, time, and command.
1. `aujx`: This variation can basically everything

### Output info

1. TTY (terminal)
    - `?`: started by system

1. STAT
    - `s`: root process / process leader, has child
    - `S`: waiting for user input, can't be disturbed
    - `R`: actively running
    - `T`: process is stopped (like <C-Z> moving process to background)

1. START : check start time of process

Show processes particular to this terminal session

    ps

Show all processes

    ps x

## netstat (weird abit on macos)

### Useful flags

1. `-b`: show which process started the connection
1. `-a`: show the state of all sockets (current connenctions and which tcp and udp ports are listening)
1. `-n`: shows only numbers and not names
1. `-p`: shows pid

## lsof (macos netstat is abit different, so use this to see ports)

1. `-i`: this can be used with or without a `:port`, if no port then it shows all

## ifconfig

### Useful flags

1. `-a`: shows you the list of interfaces along with their IP and MAC addresses (the latter one only if applicable)
1. `-l`: list all available interfaces on the system, with no other additional informatio

To find current mac ip address

    ifconfig -l | xargs -n1 ipconfig getifaddr

To find ipv4 / inet addresses

    ifconfig | grep inet

## ipconfig

Show the IP address of an interface:

      ipconfig getifaddr interface_name

## ab

### Useful flags

1. `-n`: number of requests
1. `-c`: number of concurrent requests to make at a time
1. `-t`: time limit
1. `-p`: File containing data to POST. Remember also to set -T
1. `-u`: File containing data to PUT. Remember also to set -T
1. `-T`: Content-type header to use for POST/PUT data, eg. 'application/x-www-form-urlencoded' Default is 'text/plain'
1. `-e`: Output CSV file with percentages served

Execute 100 HTTP GET requests to a given URL:

      ab -n 100 url

Execute 100 HTTP GET requests, in concurrent batches of 10, to a URL:

      ab -n 100 -c 10 url

Execute 100 HTTP POST requests to a URL, using a JSON payload from a file:

      ab -n 100 -T application/json -p path/to/file.json url
 Set the maximum number of seconds ([t]imeout) to spend for benchmarking (30 by default):

      ab -t 60 url

Write the results to a CSV file:

      ab -e path/to/file.csv

## traceroute?

## dns?

## easiest firewall?
