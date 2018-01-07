# git-summary
If you ever experiences one of the following situations, `git-summary` is for you.

* I don't remember where some of my repositories are...
* Did I forgot to push that commit?
* Do I have a repo in my system that is outdated?
* Did someone pushed new commits to master in one of my repos?
* Did I commit that quick change I made before the pizza delivery guy rang my door?

It is quite likely this ever happened to you and `git-summary` is here at the rescue! This `bash` script will neatly list the status of all git repos found within a specific directory or your entire system.

## Requirements
### Linux
* `sudo apt-get install gawk`

### MacOS
* `brew install coreutils`

## Installation
### Via aliasing
A clean way to use `git-summary` is to clone this repo and alias the script. To do so, add the following line to `~\.bashrc`:

```
alias git-summary='<PATH>/git-summary/git-summary'
```

> `<PATH>` is the path to the cloned repo. Don't worry, if you ever forget where you cloned this repo, you will be able to easily find it with `git-status` :wink:

### Via binary lookup
If you don't like cloning directories, you can simply copy `git-summary` in `/usr/local/bin`.

## Usage
General usage:

```
git-summary [options] path
```

Note that `path` is optional and the current directory will be used if left blank.

### Options
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
