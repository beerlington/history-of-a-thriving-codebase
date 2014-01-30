# Tools for a Clean History

Now that we've discussed *why* maintaining the history of your codebase is important, we're going to dig into some of the Git tools and techniques that you can incorporate into your daily development workflow.

A **workflow** is the process of incorporating new changes into an application. These changes may include things like features or bug fixes, but the process really dictates *how* they are added, and not necessarily what is added or why. Some refer to this process as a "Git workflow", but we prefer to call it a "development workflow" because Git is just one piece of it.  We want to encourage you to think about Git as a productivity tool like your editor or browser, and not as a separate process.

When a version control system such as Git is brought into the development process, the resulting workflows can take on many forms. In this chapter we'll simplify things a bit as we introduce the basics, but in the following chapter we'll take an in-depth look at a few different types of development workflows.

Since the goal of this chapter is to teach you *how* to record a clean history, there are four general steps which are common among all the different workflows. These steps include: start with a fresh branch, commit your changes, clean up commits, and finally merge your branch.

## Branching

If you're already familiar with Git, chances are you know a thing or two about branching. Branching is the first step in every workflow because it gives you a fresh starting point and allows you to clean up your progress when you're done.  It's like having an infinite number of disposable "whiteboards" for your codebase. You can document any code ideas in a branch, and when you're done you can choose to either incorporate your ideas into the main application, or simply toss them out. 

Git's branching model is one of its biggest advantages over other version control systems (VCS). When you create a branch in a centralized VCS such as Subversion, you're literally copying the entire codebase. Depending on the size of the codebase and where the repository is hosted, creating a new branch can be a slow process. Git however, is a **distributed** VCS and branching is *always* fast. There's lots of fun technical stuff happening behind the scenes that we won't bore you with, but essentially Git just creates a very small, disposable file for each branch. If you're interested in learning more about Git's inner workings, be sure to checkout the [branching chapter](http://git-scm.com/book/en/Git-Branching-What-a-Branch-Is) in Scott Chacon's timeless classic "[Pro Git](http://git-scm.com/book)" (available free of cost).

To show how easy it is to create a branch in Git, let's look at some command line examples. You can either create the branch and then check it out, or do it both in a single step. Since it's not actually copying code or going over a network like it would be in Subversion, it happens almost instantly.

Let's first confirm with `git status` that we're on the master branch:

```console
$ git status

# On branch master
nothing to commit, working directory clean
```

Next we create a new branch and check it out:

```console
$ git branch new-feature
$ git checkout new-feature

Switched to branch 'new-feature'
```

It's rare that you'd need to create a new branch and not immediately to switch to it, so Git provides a single command to combine both steps into one:

```console
$ git checkout -b new-feature

Switched to branch 'new-feature'
```

Now that you have a fresh branch, you can write your code, making progress commits any time you change something and you don't want to lose the work. As this point you may be wondering, "How do I know when to commit?". Unfortunately there aren't any hard and fast rules, and it ultimately comes down to personal taste. The key is to find a good balance between development speed and confidence. If you're practicing test-driven development, you may want to commit any time your tests are passing.

Now that you're done and ready to integrate your changes, you'll want to clean up all of your progress commits. The next section walks through this process, but before we get there, let's go over a few branching tips.

### Branching Protips 

Branching is one of the simplest and most straightforward commands that Git provides. Even so, there are a few guidelines you can follow that will greatly improve your experience.

#### Always Be Branching

Branches are so easy to create that there's really no excuse not to use them. They're incredibly fast, don't take up much disk space, and give you breathing room to code in isolation. The only exception you might make to this rule is for fixing documentation typos.

#### Use a descriptive name

It's important to use a branch name that briefly summarizes its intent. The name shows up in your history when you have a merge commit, and makes it easier to know what the purpose of the branch is without having to look at the commit messages or code. 

Some teams may elect to include the ticket number issued by a project/feature/bug tracking system. Another practice we've used with bigger teams is to prefix branch names with the developers initials. Slashes are legal, so you can do things like `pb/feature-branch`. This makes it easy to know who's been working on which features just by looking at the branches, providing an implicit sense of code ownership[^initial-prefix].

[^initial-prefix]: Pete says: I prefer to avoid the concept of "code ownership" as it can foster territorial thinking and lead to problematic team dynamics. Usually the underlying cause is a much deeper issue, but nonetheless, it's something to be cognizant of.

Here are some examples of branch types with a comparison of good and bad names:

| **Branch Type** | **Bad Names** | **Good Names** |
|-:|-|-|
| Bug Fix: | bug-fix | fix-turbolinks-1234 |
| Feature: |  admin-stuff | pb/admin-ui-5678 |
| Refactor: |  code-fixes | refactor-user-roles |
| Experimental: |  experiment | faster-tests-experiment |

No matter what branch naming conventions you choose, the important thing is that you're consistent with it and everyone on your team follows the same conventions.

#### Reduce longevity of branches

In the epic programming guide [Code Complete](http://www.cc2e.com/Default.aspx), Steven C. McConnell introduces a concept called variable "live time". Not to be confused with a variable's language-dependent "scope", "live time" is the number of statements between when a variable is first referenced to when it is last referenced. The longer a variable is "live", the more likely it is to be unintentionally modified or used; thereby introducing bugs.

With branching, the concept of "live time" describes the amount of time from when the branch is first created to when it is merged or deleted. In an active codebase with multiple developers, this can mean many changes to master. The longer a branch is around, the more likely there will be merge conflicts, dependency issues, and general confusion about why it exists. Resolving these problems can be frustrating and time consuming, so it's best to try and avoid them altogether.

#### Keep changes to a minimum

We recognize that it's not always feasible to integrate every change as soon as possible. Depending on the team and workflow, there might code reviews, QA, management decisions, and customer expectations that, for better or worse, will slow things down. One thing we've observed time after time, is that the smaller these changes are, the less resistance there will be, and the faster they'll be released. This may seem like an obvious statement, but you'd be amazed at how much time we've seen wasted by someone trying to sneak "one little refactor" or "just a tiny change" into an unrelated branch.

Keep your changes to a minimum. If you need to refactor, don't do it in the same branch as a bug fix unless it's critical to that fix. If you discover an existing bug while tackling a feature, resist the urge to fix the bug in the branch you're on.

#### Delete branches when you're done

This should probably go without saying, but don't hoard branches[^branch-hoarding]. Once they've been merged back into your release branch, there's no reason to keep them around. With GitHub's incorporation of branch management into the pull request interface, you can delete a branch as soon as it's merged.

[^branch-hoarding]: Pete says: I worked on a project in the past that would consistently have 70+ branches at any given time, and that was with only five or six developers. It was a nightmare to sort through and felt like a every time we deleted a branch, two more would show up to replace it. Many of the branches were in active development, but there were some that had been abandoned and around for so long, we were afraid to delete them. It's hard to play catch up when you already have a mess, so try and stay on top of it as much as possible.

## Committing Changes

This is just a placeholder. It felt like the natural progression was to go from branch to commit to cleanup to merge.

## Clean Up

You may remember that on your feature branch we told you to commit often, and it doesn't really matter how well these progress commit messages are written. Hopefully you're thinking *"That's not clean history!"*, because it's not. It's the *opposite* of clean, and these are *exactly* the commits you don't want mucking up your history. 

Before merging a branch, you can clean up your commit history to make it look as though you did everything in one or more well intentioned commits. This clean up process, often referred to as "rewriting" or "squashing" your commit history, is something that Git is very good at.

So what do we mean by rewriting history? We're literally talking about changing existing commits; removing them, amending them, rewriting messages, and even squashing them together. Git provides several different methods to rewrite the history of your branch. Which one you use depends on the situation you're in. These methods range from simply fixing a typo in your most recent commit message to melding dozens of commits into one.

The next few sections will describe what we feel are the essential commands to have in your Git toolbox when cleaning up history.

### Rebasing

The first topic we'll look at is rebasing, which actually encompasses multiple clean up methods. Rebasing is arguably one of the most misunderstood, yet powerful features of Git. If you've had a bad experienced with rebasing in the past, we're hoping this book gives you a new perspective on it. For those of you who are new to rebasing, we're honored to have the opportunity to introduce you[^rebase-experience]!

[^rebase-experience]: Pete says: I once worked on a team where even broaching the subject of rebasing had a polarizing effect on the developers. Most of us (myself included) regarded it as just another necessary step like branching and merging. There were a few though who shunned it. Looking back, the ones who eschewed rebasing never had a great experience with it, and weren't willing to take the time to learn it.

In a nutshell, rebasing is just another way of getting the latest changes from a branch (typically master) into your branch; except rather than adding an undesired merge commit, your changes are replayed cleanly at the end of the commit timeline.

As mentioned, the term "rebasing" encompasses multiple Git commands and options. We're going to introduce them to you here one at a time, and talk a little about how and when to use each one. If at the end of this section you're left wondering how they fit into the bigger picture, don't worry because the next chapter on development workflows is intended to fill in that gap.

#### Standard rebase
\label{sec:tools-standard-rebase}

The first rebase technique we'll talk about is called standard or non-interactive rebasing. This form of rebasing does not actually offer you a way to clean anything up by itself. What it does for you though is give you a clean starting point, as if you had just branched off master. It is also the foundation of the interactive rebase; a tool that you will soon find yourself using quite frequently.

A standard rebase is performed using `git rebase` and will resemble the following command:

```console
$ git rebase origin/master
```

Let's pretend you have a feature in your own branch containing a handful of progress commits that you started working on a few days ago. Since creating your branch, someone else has fixed a few critical bugs in the application, and their changes are on the master branch. You could ignore the changes, but it turns out one of the fixed bugs is having an impact on your development. You decide that you want to incorporate the fixes into your feature branch, but aren't really sure what the best approach is.

This situation is common in team-based development environments. In an ideal world you would never have to worry about this, but all software applications evolve over time. You always want to integrate changes in master as soon as possible because there might be merge conflicts, and the sooner you deal with them, the easier it will be. Prolonging the inevitable will only cause you more pain when you have multiple conflicts to resolve, and no one on the team can remember what has changed or how things are supposed to be.

Let's walk through an illustrative example of this scenario, and see how rebasing can help us get those bug fixes merged in.

You start on your master branch, which happens to have two commits:

```text
C1 → C2 (master)
```

Note that in all of these illustrations, the arrows are pointing to the right, indicating that the commit timeline reads from left to right. In other Git resources, you may instead see the arrows pointing to the left to indicate that the commit on the right references the commit on the left. Since we're more concerned with what's happening *outside* of Git rather than what's under the hood, this book uses left to right notation.

Then you create a branch off of master called 'new-feature' and add three commits to it:

```text
C1 → C2 (master)
       ↘︎
         C1' → C2' → C2' (new-feature)
```

In the meantime, one of your coworkers has added their bug fixes to master:

```text
C1 → C2 → C3 → C4 (master)
       ↘︎
         C1' → C2' → C2' (new-feature)
```

At this point, your branch is two commits *behind* master and you want to get those bug fixes. Let's use the standard rebase command (first fetching the changes) and see what happens:

```console
$ git fetch
$ git rebase orign/master

First, rewinding head to replay your work on top of it...
Fast-forwarded new-feature to master.
```

Note the terms "rewind", "replay", and "fast-forward" used in the above console output. One of Git's philosophies is to expose much of what is happening behind the scenes. The goal of this book is to explain what is happening *outside* of Git, but in this case the two worlds are slightly overlapping. Understanding that each of your commits is replayed one by one is important, especially when we get into resolving merge conflicts.

Now our commit timeline looks like this:

```text
C1 → C2 → C3 → C4 (master)
                 ↘︎
                   C1' → C2' → C2' (new-feature)
```

You have now successfully integrated the bug fixes from master into your branch. Since you used the standard rebase command, it's as if you just branched off master and started your work with a clean slate. You still have three commits though that are all related to the one feature, so let's jump to the next section and see how to clean them up.

#### Interactive Rebase (i.e. Squashing Commits)

Interactive rebasing is what gives us the ability to clean up the progress commits in our feature branch before merging it.

An interactive rebase is performed using `git rebase --interactive` or `git rebase -i`, and will resemble the following command:

```console
$ git rebase -i origin/master
```

Let's pick up where we left off with standard rebasing. We have a "new-feature" branch that is three commits *ahead* of master:

```text
C1 → C2 → C3 → C4 (master)
                 ↘︎
                   C1' → C2' → C2' (new-feature)
```

Let's pretend for the sake of this example that these three commits look like the following:

```console
$ git log --oneline master...new-feature

6429742 fix a typo
b7b85b9 Fixing specs
f23683b Adds new feature that customers have demanded
```

It was a small feature, and our first commit (`f23683b`) contained all the functionality we needed. After our test suite finished running though, we had to fix one of the tests. We also did a code review with another developer who found a typo. This leaves us with two extra commits containing changes that should have been done in the first commit.

Now let's start the interactive rebase command and see how we can clean this up and get a single commit:

```console
$ git rebase -i orign/master
```

\begin{codelisting}
\codecaption{}
\label{code:git-rebase-i}
```text
pick f23683b Adds new feature that customers have demanded
pick b7b85b9 Fixing specs
pick 6429742 fix a typo

# Rebase fd2bb54..6429742 onto fd2bb54
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```
\end{codelisting}

You can see all the commits listed at the top with the word “pick” next to each one. Below that is a description of what you can do with that commit.

\begin{codelisting}
\codecaption{}
\label{code:git-rebase-i-commands}
```text
reword f23683b Adds new feature that customers have demanded
fixup b7b85b9 Fixing specs
fixup 6429742 fix a typo

# Rebase fd2bb54..6429742 onto fd2bb54
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```
\end{codelisting}

We want to end up with a single commit that combines the work from the three separate commits, so we use the "reword" command on the first line, and "fixup" on the others. This lets us change the message of the first commit and discard the other two messages, but still retain their changes.

Once we save and close the window, we're taken to another screen where we can edit the message for the first commit.

\begin{codelisting}
\codecaption{}
\label{code:git-rebase-i-reword}
```text
Adds new feature that customers have demanded

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# HEAD detached from fd2bb54
# You are currently editing a commit while rebasing branch 'new-feature' on 'fd2bb54'.
#
# Changes to be committed:
#   (use "git reset HEAD^1 <file>..." to unstage)
#
#       modified:   README.txt
#
```
\end{codelisting}

Since this is going to be a production commit, we're using the recommended message format described earlier.

\begin{codelisting}
\codecaption{}
\label{code:rebase-i-new-message}
```text
Adds new feature that customers have demanded

We're adding this because our customers have been demanding it for six 
months and our sales team thinks it will be a huge win for the company. 
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# HEAD detached from fd2bb54
# You are currently editing a commit while rebasing branch 'new-feature' on 'fd2bb54'.
#
# Changes to be committed:
#   (use "git reset HEAD^1 <file>..." to unstage)
#
#       modified:   README.txt
#
```
\end{codelisting}

When we're done with our final commit message, we save and close the window once again and let git take care of the clean up magic behind the scenes, letting you know when the rebase is complete.

```console
[detached HEAD 56905f5] Adds new feature that customers have demanded
 1 file changed, 2 insertions(+), 1 deletion(-)
[detached HEAD 6433214] Adds new feature that customers have demanded
 1 file changed, 4 insertions(+), 1 deletion(-)
Successfully rebased and updated refs/heads/new-feature.
```

If you look at the single commit after, and compare to the initial commits before, you can see how it has squashed a bunch of useless messages together into a single cohesive story about what you changed.

```console
$ git log --oneline master...new-feature

6433214 Adds new feature that customers have demanded
```

#### Git Pull with Rebase

The last rebasing technique we're going to quickly go over is a handy shortcut for the standard rebase method from Section~\ref{sec:tools-standard-rebase}. If you recall, a standard rebase requires that you first run `git fetch` to ensure you have the latest code from the remote repository. It turns out that these two commands, fetch and rebase, can be combined into a single command using `git pull`:

```console
$ git pull --rebase origin master
```

The above command is roughly equivalent to the standard rebase method:

```console
$ git fetch
$ git rebase origin/master
```

Under the hood, the default behavior of `git pull` is a combination of `git fetch` and `git merge`. In some cases this is desired, such as when you're on the master branch and want to sync with the remote master branch, but since we're trying to avoid unnecessary merge commits, we're not going to use it this way. When you specify the `--rebase` flag, Git uses `git rebase` instead of `git merge` to integrate the branches. Besides eliminating an unwanted merge commit, rebasing in favor of merging also makes tracking down bugs with `git bisect` a much easier process when they're introduced by combining changes.

One thing to note before we finish this section is the difference between how the remote repository is specified in `git pull` vs `git rebase`. With `git pull`, there is a space between "origin" and "master", whereas they are separated by a slash (`/`) when using `git rebase`. This can be tricky to remember, but it helps if you think that when you're pulling, it's coming from a remote repository (origin) to a local branch (master), but when you're rebasing, it's against a local branch that references the remote repository (origin/master).

### Amending Commits

The final method we'll look at for history cleanup is not a rebasing technique, but instead is part of Git's `commit` command. Git allows you to amend (modify) an existing commit, which is useful for fixing typos in commit messages, adding files you forgot, or even modifying the author.

The amend command looks like this:

```console
$ git commit --amend
```

According to Git's documentation, it is roughly equivalent to a soft reset:

```console
$ git reset --soft HEAD^
# ... make changes ...
$ git commit -c ORIG_HEAD
```

Now let's look at a real use case where we've committed a change, but have a typo in the commit message.

\begin{codelisting}
\codecaption{\texttt{\$ git log}}
\label{code:amend-typo}
```text
commit 4d676c4ab363e75d0db269329f504e95de139f0d
Author: Peter Brown <github@lette.us>
Date:   Fri Jan 17 08:38:14 2014 -0500

    Adds new future that customers have demanded
    
    We're adding this because our customers have been demanding it for six
    months and our sales team thinks it will be a huge win for the company.
```
\end{codelisting}

As you can see in the commit message above, the word "feature" is misspelled as "future". This is slightly embarrassing and a little confusing, so we'd prefer not to have this in the codebase's permanent history. As we learned in the previous section, we *could* fix the error by using `git rebase` and rewording this commit, but a better approach is to use `git commit --amend`.

Since this was our most recent commit, we can run the following command and change the commit message in our editor:

```console
$ git commit --amend
```

After saving the file, when we look at the message of that amended commit using `git log`, we can see that our typo has been fixed:

\begin{codelisting}
\codecaption{\texttt{\$ git log}}
\label{code:amend-typo-fixed}
```text
commit 989fe9f84283d145029d80a2c571a8bbaa01b7eb
Author: Peter Brown <github@lette.us>
Date:   Fri Jan 17 08:38:14 2014 -0500

    Adds new feature that customers have demanded
    
    We're adding this because our customers have been demanding it for six
    months and our sales team thinks it will be a huge win for the company.
```
\end{codelisting}
 
There is one important thing to note about the output above, which is that the commit SHA is different before and after we amended the commit. What this means is that while the author, date, and code changes are exactly the same, it is in fact a new commit. As with rebasing, any time there's a new commit, the history has effectively been rewritten. We discuss the implications of this in following tip section.

### Protips for Clean Up

Here are a few things we've learned about cleaning up history that can save you lots of headaches.

#### Always fetch before cleaning up

This isn't so much of a tip as it is a necessity, but the reality is that it's an easy step to forget since clean up is usually the last step before merging in all your hard work. If you do forget to fetch before an interactive rebase, it won't really cause any harm because you can repeat the rebase as many times as needed. However, it might lead to confusion when you discover that you still have conflicts between your remote master branch and your local branch. 

If you're the forgetful type, you might want to setup a Git alias for `git squash` that performs a fetch before rebasing:

```text
$ git config --global alias.squash '!git fetch && git rebase -i origin/master'
```

With the above alias in place, you would then issue the interactive rebase command like so:

```text
$ git squash
```

#### Never rebase/amend on a branch someone has based work off

If you take away one thing from this entire chapter, it is that you should never, ever, ever rebase or amend on a branch that someone else has based work off. When you rebase or amend, you are replacing the existing commits with ones that resemble them, but are not quite the same. This leads to frustrating issues when trying to sync work with other developers. Scott Chacon's Git book has a section called [The Perils of Rebasing](http://git-scm.com/book/ch3-6.html#The-Perils-of-Rebasing) which gives a detailed description of exactly why it causes problems.

There are a two simple rules you can follow to help avoid this issue. First, never rebase or amend on the master branch. Second, never rebase or amend on a branch that has been pushed to a remote repository like GitHub, unless you're the only developer working on that branch, and you're developing on a single computer.

#### Prolong squashing as long as possible

Another way to avoid some of the common problems associated with interactive rebasing is to prolong the clean up process as long as possible. When we talk about code reviews in the next chapter, we'll explain why this is important. For now though, just avoid squashing your branch until you are ready to merge it.

#### Rebasing is not as scary as it seems

Even seasoned professionals get rebasing wrong on occasion, yet despite how scary this seems, we still do it. Why? Well it turns out that it's actually pretty hard to mess things up so badly that you actually lose your work. Later in the book we'll look at some of the ways to recover when things go wrong, but for now, just accept that mistakes *can* cause a headache, but you can almost always recover from them[^rebasing-reflog].

[^rebasing-reflog]: Pete says: I've been squashing commits for at least 2 years and have never unintentionally lost code that I cared about.

## Merging

Merging is the final step of every development workflow. It's where all your hard work comes to an end and you're ready for your work to be seen by the world (or your coworkers).

Before you merge your branch, you need to decide whether you want to introduce a merge commit or not. There are various arguments in favor of both sides, so we're going to discuss how they relate to a clean history to help you decide.

Merge commits offer two main benefits with respect to having a clean history. The first is that they allow you to revert multiple branch commits at once. The second is that they provide an exact point in time where the branch was merged into master.

### Merge Commit

If you merge a branch using GitHub's pull request feature, you don't have a choice about whether a merge commit is added since they are *always* created. The argument in favor of this behavior is that the merge commit links to the pull request, and code review discussions become first class citizens in your process. These discussions can act as additional documentation about why things were done a certain way, and give insight into what the developers were thinking at the time it was reviewed and merged.

If you're not using GitHub for merging branches, you'll lose the benefit of the code review discussions, but can still create a merge commit when desired. In order to manually create a merge commit, you need to specify the `--no-ff` option when merging.

Say we have a branch called "new-feature" that is ready to merge into master....

```text
$ git checkout master
$ git pull
$ git merge new-feature --no-ff
```

After the merge command is entered, your text editor will open, allowing you to specify a custom message for the merge commit. Unless you have a specific reason for the merge commit, using the default message is fine.

\begin{codelisting}
\codecaption{}
\label{code:merge-commit-message}
```text
Merge branch 'new-feature'

# Please enter a commit message to explain why this merge is necessary,
# especially if it merges an updated upstream into a topic branch.
#
# Lines starting with '#' will be ignored, and an empty message aborts
# the commit.
```
\end{codelisting}

After saving and closing the window, Git will report some info about the commit:

```text
Merge made by the 'recursive' strategy.
 README.txt | 5 ++++-
 1 file changed, 4 insertions(+), 1 deletion(-)
```