# git-summary

git-summary summarizes the status of all cloned git repositories founds within a directory (or your entire system). See an example screenshot below:

<img src="screenshot.png" width="60%">

## Currently supported operating systems: 

* Linux
* MacOS
* Cygwin
* SunOS
* Alpine based containers on Google Cloud Platform - Container Optimised OS (Chromium OS)

## Requirements

### Linux
* `sudo apt-get install gawk`

### MacOS
* `brew install coreutils`

### Alpine based containers on Google Cloud Platform (Chromium OS)
* `apk add gawk findutils`

> xargs in Chromium OS does not support -L option, findutils puts an xargs with support for -L

## Installation (on Linux-based machines)

Clone this repo somewhere and alias the script by adding this line to `~\.bashrc` (modify `$PATH` to point to the location of the cloned repo on your machine):

```
alias git-summary='<PATH>/git-summary/git-summary'
```

## Usage
General usage:

```
git-summary [options] path
```

`path` is optional and the current directory will be used if left blank.

### Options
* **-h**: Print help and exit.
* **-l**: Local summary lookup. Checks only local changes which is faster as there is no need to fetch the remote.
* **-d**: Deep lookup. Look for any git repos within the entire current directory tree. Can be slowish for large trees.
* **-q**: Quiet mode. Only print outdated repos.
* **-s**: Sorted output. (Slower as it runs sequentially to avoid race conditions).
* **-f**: Print full repo paths instead of relative ones.

## Branch status
Currently, `git-summary` does not list multiple branches per repo. However, for single repos [`git-branch-status`](https://github.com/bill-auger/git-branch-status) does this beautifully.

## Credits
A big thanks :metal: to the amazing people that contributed to the original versions of `git-summary`:

* **Forked from** [lordadamson](https://github.com/lordadamson/git-summary)
* [mzabriskie](https://github.com/mzabriskie) (Posted the original idea [here](https://gist.github.com/mzabriskie/6631607))
* [CycleMost](https://github.com/CycleMost)
* [lmj0011](https://github.com/lmj0011)
* [gimbo](https://github.com/gimbo)
* [zartc](https://github.com/zartc)

Additional thanks go to:
* [ruricolist](https://github.com/ruricolist) - Cygwin support and quiet mode.
* [romainreignier](https://github.com/romainreignier) and [tobiasw1](https://github.com/tobiasw1) - Sorted output.
* [timendum](https://github.com/timendum) - Symlink support.
* [auphofBSF](https://github.com/auphofBSF) - Alpine based containers support.
* [gaige](https://github.com/gaige) - SunOS support.
* [PontusPih](https://github.com/PontusPih) - Relative repo paths.
