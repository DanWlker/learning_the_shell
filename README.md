# fzf

## Useful flags:
1. `--query \'celeste.zip `
2. `--query "'celeste.zip | 'celeste-osx"` : query with or `|`
3. `-preview 'cat {}'`

## Command related specific syntax
1. 'celeste.zip: `'` searches exact match
2. 'celeste.zip | 'celeste-osx : or is `|`

Piping output from fzf to cat

    cat $(fzf)

# ls

Pipe ls to grep/ripgrep to find specific file

    ls -l | grep "Desktop"
    ls -l | rg "Desktop"

# tr

## Useful flags:
1. `-d "stuff"` : delete "stuff"
2. `-cd "stuff"` : delete everything except stuff
3. `-s " "` : truncate repeating " " to single

## Command related specific syntax
1. [:lower:] : lower case
2. [:upper:] : upper case
3. [:space:]
4. [:digit:]


Replace stuff in file, some extra options:

    cat README.md | tr "[:lower:]" "[:upper:]"
    tr "[:lower:]" "[:upper:]" <README.md

Pipe stuff from and into new file

    tr "[:lower:]" "[:upper:]" <README.md >newStuff.txt
