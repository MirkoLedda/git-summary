#!/bin/bash

# git-summary - summarize git repos at some path
#
# Forked from https://github.com/lordadamson/git-summary
#
# Freely distributed under the MIT license. 2018@MirkoLedda

# Colorcode
GREEN='\e[0;32m'
ORANGE='\e[0;33m'
RED='\e[0;31m'
PURPLE='\e[0;35m'
NC='\e[0m' # No Color

usage() {
    sed 's/^        //' <<EOF
        git-summary - summarize git repos at some path

        Usage: git-summary.sh [-h] [-l] [-d] [path]

        Given a path to a folder containing one or more git repos,
        print a status summary table showing, for each repo:

          - the folder name
          - the currently checked out branch
          - a short 2-column status string showing whether there are:
              * Local Changes:
                - untracked files                          "?_"
                - uncommitted new files                    "+_"
                - uncommitted changes                      "M_"
                - (nothing)                                " _"
              * Remote Changes:
                - unpulled commits for the current branch  "_v"
                - unpushed commits for the current branch  "_^"
                - (nothing)                                "_ "

        Arguments:

          -h    Print this message

          -l    Local operation only. Without this the script runs
                "git fetch" in each repo before checking for unpushed/
                unpulled commits.  As this can be time consuming, this
                flag lets you skip that.

          -d    Deep lookup. Will search within the entire tree of the
                current folder.

          path  Path to folder containing git repos; if omitted, the
                current working directory is used.

EOF
}

# Main
git_summary() {

    detect_OS

    local local_only=0
    local opt
    local deeplookup=0
    while getopts "hld" opt; do
        case "${opt}" in
            h) usage ; exit 1 ;;
            l) local_only=1 ;;    # Will skip "git fetch"
            d) deeplookup=1 ;;
        esac
    done
    shift $((OPTIND-1))

    # Use provided path, or default to pwd
    local target=$(${readlink_cmd} -f ${1:-`pwd`})
    local repos=$(list_repos $target $deeplookup)

    if [[ -z $repos ]]; then
    	exit
    fi

    # We compute the repo names and branch names here so we can
    # compute their maximum lengths and lay things out nicely.  This
    # can all be done much more easily via the column(1) utility, but
    # that has to consume all its input before it can write anything
    # out, which isn't great when you're running "git fetch" on a
    # whole bunch of repos.  Doing it like this allows us to write the
    # output to stdout incrementally.

    local branches=$(repo_branches $target)
    local max_repo_len=$(max_len "$repos")
    local max_branch_len=$(max_len "$branches")
    local template=$(printf "%%b%%-%ds  %%-%ds  %%-5s" $max_repo_len $max_branch_len)
    print_header "$template" $max_repo_len $max_branch_len

    local f
    for f in $repos ; do
        summarize_one_git_repo $f "$template" "$local_only" >&1
    done
}

# Autodetect the OS
detect_OS() {
  if [ "$(uname)" == "Darwin" ]; then  # macOS
    readlink_cmd="greadlink"
    dirname_cmd="gdirname"
    gawk_cmd="awk"
  elif [ "$(expr substr $(uname -s) 1 5)" == "Linux" ]; then  # Linux
    readlink_cmd="readlink"
    dirname_cmd="dirname"
    gawk_cmd="gawk"
  fi
}


print_header () {
    local template="$1"
    local max_repo_len=$2
    local max_branch_len=$3
    print_divider () {
        printf '=%.0s' $(seq 1 $max_repo_len)
        printf '  '
        printf '=%.0s' $(seq 1 $max_branch_len)
        printf '  '
        printf '=%.0s' $(seq 1 5)
        printf '\n'
    };

    echo
    printf "$template\n" $NC Repository Branch State
    print_divider
}


summarize_one_git_repo () {

    local f=$1
    local template=$2
    local local_only=$3

    local app_name=$f
    local branch_name=`git -C $f symbolic-ref HEAD | sed -e "s/^refs\/heads\///"`
    local numState=0

    ### Check remote state
    local rstate=""
    local has_upstream=`git -C $f rev-parse --abbrev-ref @{u} 2> /dev/null | wc -l`
    if [ $has_upstream -ne 0 ] ; then
        if [ $local_only -eq 0 ] ; then
            git -C $f fetch -q &> /dev/null
        fi
        # Unpulled and unpushed on *current* branch
        local unpulled=`git -C $f log --pretty=format:'%h' ..@{u} | wc -c`
        local unpushed=`git -C $f log --pretty=format:'%h' @{u}.. | wc -c`

        if [ $unpulled -ne 0 ]; then
          rstate="${rstate}v"
          numState=1
        else
          rstate="${rstate} "
        fi

        if [ $unpushed -ne 0 ]; then
          rstate="${rstate}^"
          numState=1
        else
          rstate="${rstate} "
        fi

    else
        rstate="--"
    fi

    ### Check local state
    local state=""
    local untracked=`git -C $f status | grep Untracked -c`
    local new_files=`git -C $f status | grep "new file" -c`
    local modified=`git -C $f status | grep modified -c`

    if [ $untracked -ne 0 ]; then
      state="${state}?"
      numState=2
    else
      state="${state} "
    fi

    if [ $new_files -ne 0 ]; then
      state="${state}+"
      numState=2
    else
      state="${state} "
    fi

    if [ $modified  -ne 0 ]; then
      state="${state}M"
      numState=2
    else
      state="${state} "
    fi

    ### Print to stdout
    if [ $numState -eq 0 ]; then
      printf "$template\n" $GREEN $app_name $branch_name "$state$rstate" >&1
    elif [ $numState -eq 1 ]; then
      printf "$template\n" $ORANGE $app_name $branch_name "$state$rstate" >&1
    elif [ $numState -eq 2 ]; then
      printf "$template\n" $RED $app_name $branch_name "$state$rstate" >&1
    fi
}


# Given the path to a git repo, compute its current branch name.
repo_branch () {
    git -C $1 symbolic-ref HEAD | sed -e "s/^refs\/heads\///"
}


# Given a path to a folder containing some git repos, compute the
# names of the folders which actually do contain git repos.
list_repos () {
	# https://stackoverflow.com/questions/23356779/how-can-i-store-find-command-result-as-arrays-in-bash
	git_directories=()

  local find_cmd
  if [ $deeplookup -eq 0 ]; then
    find_cmd="find $1 -maxdepth 2 -type d -name .git -print0"
  else
    find_cmd="find $1 -type d -name .git -print0"
  fi

	while IFS=  read -r -d $'\0'; do
		git_directories+=("$REPLY")
	done < <($find_cmd 2>/dev/null)

	for i in ${git_directories[*]}; do
		if [[ ! -z $i ]]; then
    		$dirname_cmd -z $i | xargs -0 -L1
    	fi
  done
}


# Given the path to a folder containing git some repos, compute the
# names of the current branches in the repos.
repo_branches () {
    local path=$1
    local repo
    for repo in $(list_repos $path) ; do
        echo $(repo_branch $repo)
    done
}


max_len () {
    echo "$1" | $gawk_cmd '{ print length }' | sort -rn | head -1
}


git_summary $@
