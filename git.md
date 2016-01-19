###Best Practices
- **Working on feature Branch**
  1. Merge master into feature branch often (atleast once in a day). 
     - Many times you may depend on dev(SNAPSHOT) versions of same/different projects in same repo being worked out in master. You may get various compile and run-time issues as your feature branch build may not be up to date.
  
###Rebase vs Merge

**Merge**

1. **What**: Merging two branches `(branch a)$ git merge b`  (both local or a local and a remote ) creates a new commit which marks the merge of two branches. Only corner case here is if the branch on which you are merging you changes hasn't changed since you branched out of it then git by default does a fast-forward merge which effectively put all commits of b infront of a to give a linear history. All other cases (non-fast forward) cases are referred as **true merge**.
2. **When** (true merge): Doing a merge makes sense when you want to preserve history of branch being merged.
    - When merging a completed feature branch to a public branch (say master). Always do a true merge `(master)$ git merge feature1 --no-ff `. Where --no-ff represents no fast forward.

**Rebase**

1. **What**: Rebasing two branches `(branch a)$ git rebase b` or `$ git rebase b a`  (both local or a local and a remote) as the name suggests rebases the origin of branch 'a' to the head of branch b.
2. **When**:
    - Pulling remote branch to your local branch on top of your local commits.(There is no need to create un-necessary merge loops which have no meaning as it is just difference in timing of commits made by different developers on the same branch)
       - **How**
       - Do a `$ git pull --rebase` 
       - This can be made default by setting `$ git config branch.autosetuprebase remote`. For existing branches do `$ git config branch.<branch-name>.rebase true` 
    - Rebasing master to a stale feature branch **TODO: what **
    - Another use case is not about rebasing itself but **cleaning a series of LOCAL commits**
       -  Poor commits can mean a lot of things
          1. Single commit involving non-related changes 
          2. Having muliple commits for a single issue/mini task.
          3. It may also involve typos in commits. 
          4. Multiple reverts in commits
       - One can clean up this history by `**$ git rebase -i**` (i.e interactive rebase)
       - Ref: https://medium.com/@porteneuve/getting-solid-at-git-rebase-vs-merge-4fa1a48c53aa#.lod2ibw9t
  

**NOTE**:  If you've made a series of commits that all depend on what was in the repo before someone else changed and pushed, your rebase (on top of Bob's changes) may force you to fix up all those commits, when it might have been easier to fix up only the final merge commit.

Ref: https://www.atlassian.com/git/articles/git-team-workflows-merge-or-rebase/


###Setup

Update ~/.gitconfig for global changes or repo/.git/config for local repo level config

```
[user]
        name = Harshit Aggarwal
        email = harshit.devhub@gmail.com
[push]
        default = tracking
[branch]
        autosetuprebase = remote
[core]
        editor = vim
[alias]
        s = status
        d = diff
        ds = diff --staged
        dh = diff HEAD
        a = add
        b = branch
        ci = commit
        cia = commit --ammend
        co = checkout
        st = stash
        l = log --graph --all --pretty=format:'%C(yellow)%h%C(green)%d%Creset %s %C(white)- %an, %ar%Creset'
        ll = log --stat --abbrev-commit
        lame = log --author=harshit-cs
        rh = reset HEAD
        cl = clean -df
        m = merge
        mm = merge master
```

 
