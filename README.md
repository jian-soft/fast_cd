# fast_cd
linux fast change directory

add follow lines to ~/.bashrc:

```
export MARKPATH=$HOME/.marks

function cdd {
cd -P "$MARKPATH/$1" 2>/dev/null || echo "No such mark: $1"
}
function mark {
mkdir -p "$MARKPATH"; ln -s "$(pwd)" "$MARKPATH/$1"
}
function unmark {
rm -i "$MARKPATH/$1"
}
function marks {
ls -l "$MARKPATH" |grep -v total|awk  '{print $9 $10 $11}'
}
_completemarks() {
local curw=${COMP_WORDS[COMP_CWORD]}
local wordlist=$(find $MARKPATH -type l -printf "%f\t")
COMPREPLY=($(compgen -W '${wordlist[@]}' -- "$curw"))
return 0
}
complete -F _completemarks cdd unmark

```

usage:
mark name //mark current dir to symbol 'name'
cdd name //fast cd name, which is the dir's symbol name
unmark name //delete symbol
marks //show all symbols
//cdd and unmark support tab auto complete
