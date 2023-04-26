# git-post-push
Remainder on how to create a "post-push" hooks

Basically needed a small script to sync some of my submodules after a git push while developping

In your `.git/hooks` folder or `.git/modules/XXXXXX/hooks`

`reference-transaction`:
```sh
#!/bin/bash
set -eu

SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )
cd "$SCRIPT_DIR"

while read oldvalue newvalue refname
do
    if [ $1 = committed -a $refname = refs/remotes/origin/main ]
    then
        exec ./post-push &
    fi
done
```

`post-push`:
```sh
#!/bin/bash

SCRIPT_DIR=$( cd -- "$( dirname -- "${BASH_SOURCE[0]}" )" &> /dev/null && pwd )

# `git` command is not going to use the current directory if those are set
unset GIT_DIR
unset GIT_WORK_TREE

cd "$SCRIPT_DIR"
cd ../../../../../../project-a/XXXXXX
git pull

cd "$SCRIPT_DIR"
cd ../../../../../../project-b/XXXXXX
git pull
```