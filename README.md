# git-summary
Summarizes multiple git repository status within a directory.

# Requirements

## Linux
* `sudo apt-get install gawk`

## MacOS
* `brew install coreutils`

# Usage
Call `git-status.sh [options]` from within a directory containing git repos to get their current status.

Options are listed below:
* **-h**: Print help and exit.
* **-l**: Local status lookup. Checks only local changes which is faster as there is no need to fetch the remote.
* **-d**: Deep lookup. Will look for any git repos within the entire current directory tree. Can be slowish for large trees.

# Credits
A big thanks to the amazing people that wrote the original versions of git-summary:

* **Forked from** [lordadamson](https://github.com/lordadamson/git-summary)
* [mzabriskie](https://github.com/mzabriskie) (Posted the original idea posted [here](https://gist.github.com/mzabriskie/6631607))
* [CycleMost](https://github.com/CycleMost)
* [lmj0011](https://github.com/lmj0011)
* [gimbo](https://github.com/gimbo)
* [zartc](https://github.com/zartc)
