# AGENTS

The `dev/` directory in this repo is a git submodule that points to `ZLUDA-dev`.

Common tasks:

```sh
# initialize submodules after clone
git submodule update --init --recursive

# check parent repo status
git status

# check submodule status from parent
git submodule status

# check submodule status directly
cd dev && git status

# push submodule changes
cd dev && git push origin main

# push parent repo (after submodule pointer changes)
cd .. && git push origin master
```
