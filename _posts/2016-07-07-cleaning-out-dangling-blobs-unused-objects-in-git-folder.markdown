---
published: true
title: Cleaning out dangling blobs (unused objects) in .git folder
layout: post
---
Quick note here. Today while I was training a neural network model for my project, I was messing around with the [git-plus](https://github.com/akonwi/git-plus) package of the Atom editor and thinking about adding a command or two I'd like to use (or just `git add -u`). I was reading the scripts and looking up some git commands, and with my local repo open, I decided wisely to try out `git add .` in there, completely not paying attention to the fact that I have 10+ Gigs of generated binary files unstaged (yes I know `.gitignore` is a thing). After waiting a few minutes for it to respond, I realized there was some kind of caching going on, and Ctrl-C'ed out of the garbage generation. Good news is there's nothing funny with the git status - everything was not added just yet; bad news is my .git folder grew to like 2+ Gigs.

Fortunately the [remedy](http://stackoverflow.com/questions/5277467/how-can-i-clean-my-git-folder-cleaned-up-my-project-directory-but-git-is-sti) was not hard to find. Since I hadn't committed this bad add, I just used `git fsck` to list unreachable blobs and `git prune` to delete.

The thread above also explains what you can do if things go up further with commit and push. I've never actually checked in Gigs of garbage to the remote repo, but I did see somebody else with fairly large chunks of data online, and back in the day I used to put pdf files along with tex before someone pointed out to me this was not what git was for, so yeah the cleaning thing may come in handy.