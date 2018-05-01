# gitcolorbash

## Color bash and show git branch

- Open `~/.bash_profile` and add the below code, code can also be added in `~/.bashrc`

```sh
parse_git_branch() {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}


function branch_status {
    branch=`branch`
    [ -n "$branch" ] && echo " ($branch)`dirty_status`" || echo
}

function branch {
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/\1/'
}

function dirty_status {
    git status --porcelain | (
        unset dirty deleted untracked newfile copied renamed
        while read line ; do
            case "${line//[[:space:]]/}" in
                @('M'|'UU')*)  dirty='!' ; ;;
                'D'*)          deleted='x' ; ;;
                '??'*)         untracked='?' ; ;;
                'A'*)          newfile='+' ; ;;
                'C'*)          copied='*' ; ;;
                'R'*)          renamed='>' ; ;;
            esac
        done
        bits="$dirty$deleted$untracked$newfile$copied$renamed"
        [ -n "$bits" ] && echo "[$bits]" || echo
    )
}

export PS1="\u@\h \[\033[32m\]\w\[\033[33m\]\$(branch_status)\[\033[00m\] \n$ "
```
- Restart the terminal
