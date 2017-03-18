### What is Git ?
Git is an Open Source Distributed Version Control System. 
* Git source code freely available for use, distribution and modification under GNU Public License 2.0. (Not a proprietary software)
* VCS: Record changes to file(s), to be able to recall specific versions later.
* Client-server model: (Not a local VCS) A group of people can collaborate. 
* Distributed => (Not centralized) one where Clients to get the entire repository and not just the latest snapshot of files.
  * Almost all operations are local => No performance bottleneck
  * No single point of failure

### Architecture
Working Directory <--> Index(Staging area) <--> Local Repository <--> Remote repository

### Commands

#### Cloning existing / Initialize 

#### Working directory <->Index
Working directory -> Index
```
$$ git add . ## add all unstaged data to index
$$ git add  [file paths with spaces] ## add selective files to index
$$ git add -p [file paths with spaces] ## interactively add certain patches of input files to index
```
Working directory <- Index
```
$$ git reset HEAD ### Update entire Index (not working directory) to state of HEAD. All staged changes get unstaged
$$ git reset HEAD -- [file paths with spaces]
```
#### Commiting changes
Local Changes
1. Index -> Local Repo
```
$$ git commit
```
2. Working directory -> Local Repo (Direct without going though staging)
```
$$ git commit -- [file paths of unstaged ]
$$ git commit -a 
```
3. Updating last local only commit (if any)
```
$$ git commit --amend
```



#### Stashing changes
Temporarily stash contents of working directory and/or index 
```
$$ git stash ## stash contents of working directory AND index 
$$ git stash save “one liner for stash content” ## annotated stash, very helpful
$$ git stash -k ## stash only the contents of working directory and NOT index. Useful for testing staged items in isolation, before commit.
```
View stashed items
```
$$ git stash list ## view list of stashes
$$ git stash show stash@{1} ## to see listing of files deleted/changed/created in particular stash
$$ git stash show -p stash@{1} ## to see code diff of updates in particular stash
```
Apply stash back
```
$$ Git stash pop ## top stash item brought to working directory and removed from stash
$$ git stash apply [stash@{n}] ## applies nth (default 0) stash entry to Working directory
### pop should be your preferred choice as it drops (delete) the stash as well.
```

### Best Practices
- **Working on feature Branch**
  1. Merge master into feature branch often (atleast once in a day). 
     - Many times you may depend on dev(SNAPSHOT) versions of same/different projects in same repo being worked out in master. You may get various compile and run-time issues as your feature branch build may not be up to date.
  
### Rebase vs Merge

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


### Setup

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
### Under the hood

How does it work ?
1. Cloning or creating a repo creates a .git folder in the top directory (current dir)
2. Changes to file are detected via SHA1 checksums (40 char hexadecimal string) 
3. Git has 3 basic objects(basically files) for data storage 
   * Blob: A compressed form of entire file (not delta change)## that are tracked
   * Tree: Like a file-system directory can refer to blobs and other trees. One tree per-directory in codebase
   * Commit: Stores the tree object corresponding to code repo, reference to previous commit object  and  of who made commit 
4. Changes(addition/modification) are tracked using git add. Doing so a new blob object is created for entire file, index is updated and a new tree object is created. Index data info same as the latest tree object for repo. 
   * Check using $ git ls-files --stage # and compare with latest tree object hash i.e 
            `$ git cat-file -p <hash of latest tree> # can find using ls -ltr on ./objects folder after add  `
5. On doing a commit (if index state is different from tree pointed out by HEAD) a new commit object is created 
```
harshit@localhost:git_test> git cat-file -p g6138ffe7aa1bdc73c56793671c1771abf382947
tree rfb8bb102afeca86037d5b5dd89ceeb0090eae9d
author Harshit <myid@abc.com> 1420306882 -0500
committer Harshit <myid@abc.com> 1420306882 -0500
harshit@localhost:git_test> git cat-file -p rfb8bb102afeca86037d5b5dd89ceeb0090eae9d
100644 blob ef18e512dba79e4c8300dd08aeb37f8e728b8dad	test.txt
harshit@localhost:git_test> git cat-file -p ef18e512dba79e4c8300dd08aeb37f8e728b8dad
                                                 hello world
```

 6. It uses HEAD to point to the current branch (stores location of branch like refs/heads/featureBranch)
 7. Each of refs/heads/<branchname> points to the latest commit object in that branch present in objects/\<First two chars of has>/\<remaining chars of hash>. 
 8. These are the basic building blocks (storage and tracking). Rest all features use these and reference mechanism to achieve their goals

 
