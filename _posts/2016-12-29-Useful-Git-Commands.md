---
title: Useful Git Commands  
category: git  
tags: [git]  
layout: post  
lang: en
---

I love using git command to do everything, so I added some commands/alias to speed up some general git operations.

## Git alias

You can add your alias by opening gitconfig with command `git config --global -e`, then you can add a `[alias]` section into it.

Here is my `[alias]`section:

{% highlight sh %}
st = status
llg = log --oneline -n
p = pull --rebase
ci = commit -m
cia = commit -am
co = checkout
amend = commit --amend -a --no-edit
{% endhighlight %}

## Powershell commands
There are still other operations that need a combination of several git commands, I added them as powershell function into my powershell profile(As I'm using windows to do the development at the moment).

Use `notepad $profile` or any other editor you have to open the powershell profile. If it's not existed, create one.
Then I have the following functions in it:

{% highlight powershell %}

function ws(){
    cd c:\workspace
}
ws

#rebase latest master to current branch
function gr(){
    $currentBranch = git symbolic-ref --short HEAD
    write-host "Current branch: <$currentBranch>" -f green
    git co master
    write-host "Pull master latest code..." -f green
    git p
    write-host "Go back to <$currentBranch>." -f green
    git co $currentBranch

    write-host "Rebasing master into <$currentBranch>..." -f green
    git rebase master
}

#go to branch with a substring match
function gb($pattern){
    while($pattern -eq $null -or $pattern -eq ""){
        write-host "please give a sub string in the branch name, here are the branches:" -f red
        git branch

        $pattern = Read-Host "Switch to:"
    }

    $branch = git branch | ?{$_ -like "*$pattern*"} | Select-Object -first 1
    git co $branch.trim("*").trim()
}


{% endhighlight %}

## Merge commits

If you have several local commits, and you want to merge them into one when committing, you can use this command:

`git rebase -i` or  
`git rebase -i head~3` if you only want to handle the first 3 commits.

it will open a window like this:

{% highlight sh %}
....
# Rebase 598cf24..598cf24 onto 598cf24 (1 command(s))
#
# Commands:
# p, pick = use commit
# r, reword = use commit, but edit the commit message
# e, edit = use commit, but stop for amending
# s, squash = use commit, but meld into previous commit
# f, fixup = like "squash", but discard this commit's log message
# x, exec = run command (the rest of the line) using shell
# d, drop = remove commit
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
{% endhighlight %}

then you can change the commands to `squash`(if you want to keep the commit message) or `fixup`(discard the commit message)