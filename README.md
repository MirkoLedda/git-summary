# git-summary
Did you ever forgot what is going on with your local git repos? Like, where they actually are located? If you made changes to repos that you did not commit or push? or if you are a few commits behind `origin/master`?

All Github users will experience that one way or another and `git-summary` is here at the rescue! This `bash` script will neatly list the status of all git repos found within a directory.

## Requirements
### Linux
* `sudo apt-get install gawk`

### MacOS
* `brew install coreutils`

## Installation
### Via aliasing
A clean way to use `git-summary` is to clone this repo somewhere on your system and alias it so you can call the command `git-summary` from any directory. Add this line to `~\.bashrc` with `<PATH>` being the path to the cloned repo:

```
alias git-summary='<PATH>/git-summary/git-summary'
```

> Don't worry, if you forget where you cloned this repo, you will be able to easily find it with `git-status` :wink:

### Via binary lookup
If you don't like cloning directories, you can simply copy `git-summary` in `/usr/local/bin`.

## Usage
Call `git-summary [options]` to get a summary of all git repo within the current folder.

Available options are listed below:
* **-h**: Print help and exit.
* **-l**: Local status lookup. Checks only local changes which is faster as there is no need to fetch the remote.
* **-d**: Deep lookup. Will look for any git repos within the entire current directory tree. Can be slowish for large trees.

## Credits
A big thanks :metal: to the amazing people that wrote the original versions of `git-summary`:

* **Forked from** [lordadamson](https://github.com/lordadamson/git-summary)
* [mzabriskie](https://github.com/mzabriskie) (Posted the original idea posted [here](https://gist.github.com/mzabriskie/6631607))
* [CycleMost](https://github.com/CycleMost)
* [lmj0011](https://github.com/lmj0011)
* [gimbo](https://github.com/gimbo)
* [zartc](https://github.com/zartc)
