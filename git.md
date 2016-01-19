###Best Practices
- **Working on feature Branch**
  1. Merge master into feature branch often (atleast once in a day). 
     - Many times you may depend on dev(SNAPSHOT) versions of same/different projects in same repo being worked out in master. You may get various compile and run-time issues as your feature branch build may not be up to date.
  

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

 
