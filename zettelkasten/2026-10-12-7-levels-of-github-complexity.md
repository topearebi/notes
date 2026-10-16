---
title: 7 levels of GitHub complexity
date: 2026-10-12 13:14 +0100
source: "What’s a gist? 🤷🏼‍♀️ : r/github"
tags: []
---

> The simplest variation of what GitHub offers is a Gist. You make an account, upload some working code (or a cake recipe, or a movie pitch, any single text document), and now it's available online. Usually, if you're the one posting the Gist, it's specific to you- The world doesn't need another Java Hello World, but it might be handy to have access to your personalized IDE config.
>
> So level one is text files, available online, tied to your login.
>
> But over time, your tastes change, and you update your config to have different settings (or the recipe is now sugar free, or the movie makes more sense as a musical). You can edit your gist, and not only will the latest version be available, you'll be able to see the versions before, the differences, the dates each one was posted, that sort of thing.
>
> So level two adds version history and meta data.
>
> This has been mighty helpful, now. You've been using the features, sharing your file with friends, making edits as needed. But you decide that just one file isn't going to cut it. You want to store config files for your entire tool chain (or a cookbook's worth of recipes, or a collection of movie pitches). Now you need a git. You start off by uploading the document you already have as a gist, but you can add other files. Those will also be available and have version history. More important, the folder itself will have version history; On this day you added this file, on that day you removed that file, today you're uploading a folder with folders in it that each have files in them.
>
> So level three adds a directory.
>
> Well now your files are getting popular. Your friends, the friends of your friends, even total strangers are finding and using your configs (cookbook, pitches). You start to take advantage of GitHub features, like Issue tracking, where users document where your file had an error. You can interact with them, indicate that this new version fixed that issue, or that what they're describing is not, in your opinion, an issue.
>
> So level four adds social interaction.
>
> Since this is all publicly available, the person with the issue you didn't care about decided to copy your entire git, and then fix that issue in a new version. This is called a fork, because if you look at the version history, there's a split between your versions and theirs. Your's is still more popular by far, so they send a Pull Request, which would update your git to their version. If you haven't made any changes, no problem! You can just add their version as the latest version. If you've made changes that don't interfere, like you changed only the first lines of each file and they changed only the last lines, then you can 'Merge', which is the opposite of a fork. The version history of this file will show both yours and theirs coming together. Sometimes you'll edit the same line, so the merge isn't automatic. Git will take you line by line, saying here's yours, there's his, which would you like? This is called resolving merge conflicts.
>
> So level five adds forks, merges, pull requests (which should honestly be called merge requests), merge conflicts, and resolving conflicts.
>
> Well now there are tons of people sending pull requests, making changes here or there. Some changes seem promising, but are currently broken. You can go into the version history and tag the last working version 'master'- this is the version newcomers will see and use. Those promising changes are tagged 'development', and are only available if the user looks for that tag.
>
> So level six adds branching. Each branch shows the current version that fits the description, as well as the history of that branch.
>
> Well now you have a master branch that for sure works, but there's all sorts of development branches that... Kinda work? Some fearless users swear by branch development/prettierErrorMessages, because it mostly works and the parts that don't are at least pretty. We can quantify precisely what does and does not work by setting up a testing suite. It's not as bad as it sounds. The test might be written in Python, for example, and do static analysis of your config files. Does the number of open curly braces match the number of closed curly braces? It's up to you to write tests that matter. If you put the same tests on each branch, then the ReadMe for that branch can automatically say "4/10 tests pass, this branch is probably broken".
>
> So level seven adds unit tests and continuous integration.
>
> That should provide an overview of not just what you'll use, but what you might find.
>
> — What’s a gist? 🤷🏼‍♀️ : r/github

This is such a good beginner level explanation of what GitHub is and how it works

## Related

- [[2026-10-12-managing-github-repos-in-crostini|Managing github repos in crostini]]
