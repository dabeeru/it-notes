## tty
- `tty` are pseudo devices (available in `/dev/tty[0-...]`) which allows to input shell commands

## Type of commands 
- `type` command allows to check type of the command
	- **Internal** - built in function of shell (etc. `cd, set, export`)
	- **External** -  external application available in `PATH`

## Prompt
By default `$` on end of the prompt stands for regular user and `#` for `root`.

## Quotes
`"..."` - Allows to treat all tesxt inside as regular characters, but still substitution with `$` is possible
```bash
$ echo I am $USER
I am tom
```

`'...'` - treats all characters as regular characters without substitution capabilities

## Environment variables
**Environment variables** are variables which are available both in the current session and all of the sub processes spawned from that shell session.

`PATH` - tells shere where to look for executable commands
`HOME` - path to home directory of current user
`SHELL` - path to the currently used shell binary

`export VARIABLE` makes the variable available to subprocesses.
`env` - prints all environment variables

## Finding files
- `locate` - allows to locate files based on some internal db, dp can be updated with `updatedb`
- `find` - allows to look for files recursively

## Misc
### `ls`
- `ls ~user` - list home dir of specified user
- `ls -lX` - sorting by extenion
- `ls -lt` - sorting by time
- `ls -ltr` - reversing the sort
- `ls -S` - sorting by size

### Other
`mkdir -p` - creates also parent directories
`rm -ri` - you will be prompted whether to deleted the specific directories
