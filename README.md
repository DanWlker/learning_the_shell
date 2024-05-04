# fzf

Piping output from fzf to cat

    cat $(fzf)

To see the preview:

    fzf --preview 'cat {}'

Put in query in advanced:
    
    fzf --query \'celeste.zip 
    fzf --query "'celeste.zip | 'celeste-osx"


# ls

Pipe ls to grep/ripgrep to find specific file

    ls -l | grep "Desktop"
    ls -l | rg "Desktop"

