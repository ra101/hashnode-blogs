---
title: "Boost your Git Squash-Rebase Workflow with these Commands! (auto-squash, auto-commit, auto-rebase, auto-push)"
datePublished: Thu Aug 17 2023 14:55:09 GMT+0000 (Coordinated Universal Time)
cuid: cllfa9jn0000309l8bcjo0ofu
slug: git-auto-squash
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692284569550/90ef554d-2f54-4e0a-8573-bccb88908720.jpeg

---

I recently joined a company, that follows squash-rebase flow here, and as my Git KT was going on, I figured I should build these commands before actually starting development, now that I have started development, these turned out to be quite handy!

---

### What is Git Squash-Rebase Workflow?

Git Squash Rebase is a workflow in which:

* multiple commits are squashed into a single cohesive commit,
    
* then rebased from the up-to-date main branch.
    

This results in a cleaner commit history, grouping of changes, easier code reviews, and streamlined collaboration. *Of-course there are some drawbacks of this workflow, but that is beyond the scope of this post.*

---

### Git Squash (`gsq`):

```bash
gsq(){
    # Usage:
    ## gsq -b "base_branch_opt" -n "squash_count_opt" -m "msg" -d "desc" -D
    ##
    ##    -b: Base Branch Name (default: default origin branch)
    ##    -n: Squash Count (default: all until base branch)
    ##    -m: Commit Message, (default: latest commit message)
    ##        - Uses Auto Commit Command, if present.
    ##    -d: Commit Description, (default: null)
    ##    -D: if passed, the description is populated with all the previous commit msg and desc until sqaush_count

    # Get Base Branch -> Get Fork Point -> Get Squashable Count -> Squash!

    ## Even though this command seems big, but the flow is pretty simple.
    ## I use a very lite version of this code, there is alot of validation and
    ## edges cases here, which is not particularly I care about in my personal use.

    # Declare Local Var
    local base_branch_opt; local squash_count_opt; local commit_msg_opt;
    local commit_desc_opt; local add_full_desc_opt=false;

    # Get Optional Arguments!

    ## Reset the getopts state
    OPTIND=1
    ## Parse Through new opts.
    while getopts b:n:m:d:D flag
    do
        case "${flag}" in
            b) base_branch_opt=${OPTARG};;
            n) squash_count_opt=${OPTARG};;
            m) commit_msg_opt=${OPTARG};;
            d) commit_desc_opt=${OPTARG};;
            D) add_full_desc_opt=true;;
        esac
    done

    # Get Base Branch
    # ----------------

    ## Update base_branch_opt with default origin branch, if not passed!
    local current_branch=$(git rev-parse --abbrev-ref HEAD)
    if [[ $base_branch_opt == "" ]]; then
        base_branch_opt=$(git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@')
    fi

    # Get Fork Point Commit
    # ----------------

    # Check if given base branch is actually an ancestor or not.
    local fork_point_commit=""
    if [[ $( git branch --merged HEAD 2> /dev/null | sed  -e 's/* \(.*\)/ \1/' -e  's/^ *//g' | grep -w "$base_branch_opt" ) != "" ]]; then
        ### Get the Last commit by `base_branch_opt`
        fork_point_commit=$(git merge-base --fork-point "$base_branch_opt")
    else
        ### Throw Error if Base Branch is not found as an ancestor.
        echo "Invalid Base Branch! " && return 1
    fi

    # Get Squashable Count
    # ----------------
    ## Exit, if Squashable Count is 0!
    ## We are okay with squashing 1 commit, the reason for that is, this allows us
    ## to stage the changes and use `gsq` to commit those changes into squashed commit


    ## squashable_count: Number of commits that can be to be squashed
    local current_commit=$(git rev-parse HEAD)
    local squashable_count=$(git rev-list --count $fork_point_commit..$current_commit)

    ## Exit, if passed squash_count_opt is `0` or not a `number`!
    if [[ $squash_count_opt != "" && ( ! $squash_count_opt =~ ^[0-9]+$ || $squash_count_opt == 0 )]]; then
        echo "Invalid Squash Count $squash_count_opt! " && return 1
    elif [[ $squash_count_opt == ""  ]]; then
      squash_count_opt=$squashable_count
    fi

    ## Exit, if found `squashable_count` turns about to be `0` !
    if [[ $squashable_count == "0" ]]; then
        echo "Nothing to Squash! " && return 1
    fi

    ## Squash Count Passed cannot be more than Squashable Count found!
    if [[ $squashable_count < $squash_count_opts ]]; then
        echo "Cannot Squash more than $squashable_count! " && return 1
    fi

    # Squash!: Get Commit Message and Description.
    # ----------------

    ## Get Commit Message, (Default: title from latest commit)
    if [[ $commit_msg_opt != "" ]]; then
      local commit_message=$commit_msg_opt
    else
      local commit_message=$(git log --format="%s" -n1)
    fi

    ## Get Full Commit Description, (Default: raw body from last n commit)
    if [[ $add_full_desc_opt == true ]]; then
      if [[ $commit_desc_opt != "" ]]; then
        commit_desc_opt="$(echo $commit_desc_opt) $(git log --format='%B' -n$squash_count_opt)"
      else
        commit_desc_opt="$(git log --format='%B' -n$squash_count_opt)"
      fi
    fi

    ## create commit_desc_if_there if commit_desc_opt is populated
    local commit_desc_if_there=""
    if [[ $commit_desc_opt != "" ]]; then
      commit_desc_if_there=$(echo "-m '$commit_desc_opt'")
    fi

    # Squash!: Reset Commits Softly before commiting back.
    # ----------------

    ## Normally Squashing is done using rebase,
    ## but I could not automate its interactive shell.
    git reset --soft HEAD~$squash_count_opt

    # Squash!: Commit to complete squashing!
    # ----------------

    ## Use Auto Commit Command if There, Else use Default `git commit`
    if [[ $commit_msg_opt == "" && $(command -v gac) ]]; then
        gac $commit_desc_if_there
    else
        git commit -m "$commit_message" $commit_desc_if_there
    fi
}
```

---

### Git Auto Commit (`gac`):

```bash
gac() {
    ## The Idea is to create a commit message from branch name
    ## For Example:
    ##    if branch name is `feature/jira-123-this-is-issue-desc`
    ##    then commit would be `[Enhancement JIRA-123] This Is Issue Desc`

    # Exit, if there is nothing to commit!
    if [[ $(git diff --staged) == "" ]]; then
        echo "Nothing to Commit! " && return 1
    fi


    # Fetch Details
    # ----------------

    ## `feature/jira-123-this-is-issue-desc`, all in lower case
    local current_branch=$(git rev-parse --abbrev-ref HEAD | tr '[:upper:]' '[:lower:]')

    ## `feature`
    local issue_type=$(echo $current_branch | cut -d '/' -f 1)

    ## `jira-123`
    local issue_no=$(echo $current_branch | cut -d '/' -f 2- | cut -d '-' -f -2)

    ## `this-is-issue-desc`
    local issue_desc=$(echo $current_branch | cut -d '/' -f 2- | cut -d '-' -f 3- )


    # Format Details
    # ----------------

    ## We will use this map to get issue_type out of branch name
    declare -A branch_map; branch_map["feature"]="Enhancement";
    branch_map["bugfix"]="Patch"; branch_map["version"]="Upgrade";

    ## `feature` -> `Enhancement`; `random` -> `Random`
    issue_type=${branch_map[$issue_type]-${issue_type^}}

    ## `jira-123` -> `JIRA-123`
    issue_no=$(echo $issue_no | tr '[:lower:]' '[:upper:]')

    ## `this-is-issue-desc` -> `This Is Issue Desc`
    issue_desc=$(echo $issue_desc | sed 's/-/ /g' | awk '{for (i=1; i<=NF; i++) $i=toupper(substr($i,1,1)) tolower(substr($i,2))}1')

    ## `[Enhancement JIRA-123] This Is Issue Desc`
    local commit_msg="[$issue_type $issue_no] $issue_desc"

    # Commit!
    # ----------------

    ## Along with this message, I have added $@, So that all the
    ## flags and arguments of `git commit` can be passed in this command.
    git commit -m "$commit_msg" $@
}
```

---

### Git Fetch Rebase (`gfrb`):

```bash
gfrb() {
    ## If $1 is not passed, then it will fetch the default branch
    local base_branch=${1:-$(git symbolic-ref refs/remotes/origin/HEAD | sed 's@^refs/remotes/origin/@@')}
    git fetch origin "$base_branch":"$base_branch"

    ## Rebase requires stashing, but any file that has both
    ## staged and unstaged changes will lose all its
    ## unstaged changes! So uncomment at your own risk.

    # git stash --all

    git rebase "$base_branch"

    # git stash pop
}
```

---

### Git Upstream Push (`gup`):

```bash
gup() {
    ## Although `--force` is frequently used in this workflow.
    ## It still feels destructive to add here.
    ## Instead I have added $@, So that all the `git push` 
    ## flags and arguments can be passed in this command.
    local current_branch=$(git rev-parse --abbrev-ref HEAD)
    git push --set-upstream origin "$current_branch" $@
}
```

---

Update your `~/.[ba|z]shrc` with these productive commands and save those few minutes. Happy Commiting!

**You also might like these:**

* Check out my gist ([alias](https://gist.github.com/ra101/70ea4389ba04bf6d5a172d9a3000e5f7)[.sh](http://alias.sh)) for a big list of aliases for git, python, Django, and docker.
    
* Made a meme into reality - [New Bash Alias: Git Out](https://blog.ra101.dev/new-bash-alias-git-out)
    
* And Finally, a fun video - [Bash Script: Enjoy the Little Things!](https://www.youtube.com/watch?v=z0-Rw7guZDY)
    

**PS: Feel free to share your productivity Hack!**