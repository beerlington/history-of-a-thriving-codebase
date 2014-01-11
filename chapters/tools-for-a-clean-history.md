# Tools for a Clean History

Now that we've discussed *why* maintaining the history of your codebase is important, we're going to dig into some of the Git tools and techniques that you can incorporate into your daily development workflow.

A **workflow** is the process of incorporating new changes into an application. These changes may include things like features or bug fixes, but the process really dictates *how* they are added, and not necessarily what is added or why. Some refer to this process as a "Git workflow", but we prefer to call it a "development workflow" because Git is just one piece of it.  We want to encourage you to think about Git as a productivity tool like your editor or browser, and not as a separate process.

When a version control system such as Git is introduced into the development process, the resulting workflows can take on many forms. In this chapter we'll simplify things a bit as we introduce the basics, but in the following chapter we'll take an in-depth look at a few different types of development workflows.

Since the goal of this chapter is to teach you *how* to record a clean history, there are four general steps which are common among all the different workflows.

These steps include:

* Start with a fresh branch before writing any code
* Commit to this branch as you make progress
* Clean up your progress when you're ready to merge
* Merge your branch into the release branch (i.e. master)

## Branching

If you're already familiar with Git, chances are you know a thing or two about branching. Branching is the first step in every workflow because it gives you a fresh starting point and allows you to clean up your progress when you're done.  It's like having an infinite number of disposable "whiteboards" for your codebase. You can document any code ideas in a branch, and when you're done you can choose to either incorporate your ideas into the main application, or simply toss them out. 

Git's branching model is one of its biggest advantages over other version control systems (VCS). When you create a branch in a centralized VCS such as Subversion, you're literally copying the entire codebase. Depending on the size of the codebase and where the repository is hosted, creating a new branch can be a slow process. Git however, is a **distributed** VCS and branching is *always* fast. There's lots of fun technical stuff happening behind the scenes that we won't bore you with, but essentially Git just creates a very small, disposable file for each branch. If you're interested in learning more about Git's inner workings, be sure to checkout the [branching chapter](http://git-scm.com/book/en/Git-Branching-What-a-Branch-Is) in Scott Chacon's timeless classic "[Pro Git](http://git-scm.com/book)" (available free of cost).

To show how easy it is to create a branch in Git, let's look at some command line examples. You can either create the branch and then check it out, or do it both in a single step. Since it's not actually copying code or going over a network like it would be in Subversion, it happens almost instantly.

Let's first confirm we're on the master branch:

```
$ git status
```
```
# On branch master
nothing to commit, working directory clean
```

Next we create a new branch and check it out:

```
$ git branch new-feature
$ git checkout new-feature
```
```
Switched to branch 'new-feature'
```

It's rare that you'd need to create a new branch and not immediately to switch to it, so Git provides a single command to combine both steps into one:

```
$ git checkout -b new-feature
```
```
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

#### Clean up when you're done

This should probably go without saying, but don't hoard branches[^branch-hoarding]. Once they've been merged back into your release branch, there's no reason to keep them around. With GitHub's incorporation of branch management into the pull request interface, you can delete a branch as soon as it's merged.

[^branch-hoarding]: Pete says: I worked on a project in the past that would consistently have 70+ branches at any given time, and that was with only five or six developers. It was a nightmare to sort through and felt like a every time we deleted a branch, two more would show up to replace it. Many of the branches were in active development, but there were some that had been abandoned and around for so long, we were afraid to delete them. It's hard to play catch up when you already have a mess, so try and stay on top of it as much as possible.

## Cleaning Up (i.e. Rewriting History)

You may remember that on your feature branch we told you to commit often, and it doesn't really matter how well these progress commit messages are written. Hopefully you're thinking *"That's not clean history!"*, because it's not. It's the *opposite* of clean, and these are *exactly* the commits you don't want mucking up your history. Before merging your branches, you can clean up your commit history and make it look as though you did everything in one or more, well thought out commits.

Git provides us with some really powerful ways to rewrite the history of our branches, allowing us to clean them up before merging. When we discuss workflows in the next chapter you'll see how rewriting history fits into the bigger picture, but for now just try and follow along.

So what do we mean by rewriting history? We're literally talking about changing existing commits by removing them, amending them, rewriting messages, and even squashing them together. There are a number of different ways to rewrite the history of your codebase, depending on many different factors. They range from fixing a typo in your most recent commit message to resetting a branch to a specific point in time.

The next few sections will describe what we feel are the essential commands to have in your Git toolbox when cleaning up history.

### Rebasing

The first method we'll look at is rebasing. Rebasing is arguably one of the most misunderstood, yet powerful features of Git. If you're new to Git, or have had a bad experience with rebasing in the past, now would be a good time to take a short break. Exercising has been proven to assist with learning, so stand up, go outside, and take a brisk walk around your neighborhood or block before reading on.

In a nutshell, rebasing is just another form of merging, except rather than adding an undesired merge commit, your changes are applied cleanly at the end of the commit timeline.

Here are a few examples of what rebasing commands looks like, and we'll explain each one in detail in the following sections:

```
$ git rebase origin/master
$ git rebase -i origin/master
$ git pull --rebase origin master
```

#### Non-interactive rebase

```
$ git rebase origin/master
```

Let's pretend you have a feature in your own branch that you started working on a few days ago, and have a handful of progress commits. In the meantime, someone else has fixed a few critical bugs in the application, and their changes are on the master branch. You could ignore the changes, but it turns out one of the fixed bugs is having an impact on your development. You decide that you want to incorporate the fixes into your feature branch, but aren't really sure what the best approach is.

The situation described above is common in team-based development environments. In an ideal world you would never have to worry about this, but all software applications evolve over time. You always want to integrate changes to master as soon as possible because there might be merge conflicts, and the sooner you deal with them, the easier it will be. Prolonging the inevitable will only cause you more pain when you have multiple conflicts to resolve, and no one on the team can remember what has changed or how things are supposed to be.

Rebasing lets you replay your commits at a different point in time, as if you branched off master in its current state, after the bugs were fixed.

Let's walk through an illustrative example of the scenario described at the beginning of this section, and see how rebasing can help us get those bug fixes merged in. If you want to see a representation of your repo's timeline that's similar to the examples shown below, you can use the `git log` command with a bunch of formatting flags:

```
$ git log --oneline --graph --color --all --decorate
```

Note that in all of the examples, the arrows are pointing to the right, indicating that the commit timeline reads from left to right. In other Git resources, you may instead see the arrows pointing to the left to indicate that the commit on the right references the commit on the left. Since we're more concerned with what's happening *outside* of Git, this book uses left to right notation.

You start on your master branch, which happens to have two commits. 

```
C1 → C2 (master)
```

Then you create a branch off of master called 'my-feature', add three commits to it, and another developer adds their bug fixes to master:

```
C1 → C2 → C3 → C4 (master)
       ↘︎
         C1' → C2' → C2' (new-feature)
```

At this point, your branch is two commits *behind* master, so let's see what the rebase command does:

```
$ git fetch
$ git rebase orign/master
```
```
First, rewinding head to replay your work on top of it...
Fast-forwarded new-feature to master.
```

Note the terms "rewind" and "fast-forward" used in the above console output. One of Git's philosophies is to expose much of what is happening behind the scenes. While we do believe that understanding Git's internals can give developers an advantage in certain situations, the philosophy of this book is just the opposite.

Now our commit timeline looks like this:

```
C1 → C2 → C3 → C4 (master)
                 ↘︎
                   C1' → C2' → C2' (new-feature)
```

Does this belong in the merging section???:

```
C1 → C2 → C3 → C4 → C5 → C6* → C7 (master)
                 ↘︎
                  C1' → C2'* → C3' (my_branch)
```
In the illustration above, commits C6* AD C2'* have divergent changes to the same file, and Git cannot resolve the changes automatically, leading to what's known as a merge conflict. You have two options at this point: 1) Merge master into my_branch, and resolve the conflict, resulting in a merge commit, or 2) rebase so that your branch thinks that it branched off commit C7 instead of C4. You will still need to resolve the merge conflict in this case, however a merge commit will not be introduced. The following section discusses the ins and outs of merge conflicts, so don't worry if this concept is foreign to you.

You will be left with a structure that looks like this:

```
C1 → C2 → C3 → C4 → C5 → C6 → C7 (master)
                                ↘︎
                                 C1' → C2' → C3' (my_branch)
```

At this point, you can merge your branch back into master without needing to resolve the conflict again.

Most Git books would probably leave you here wondering when to merge and when to rebase. Since this is not most books, we're going to dive into that question a little more, but first let's discuss interactive rebasing (squashing).

#### Interactive Rebase (i.e. Squashing Commits)

```
$ git rebase --interactive origin/master
```

Next we're going to discuss a type of rebasing called interactive rebasing. Interactive rebasing is what gives us the ability to cleanup and squash our progress commits in our feature branch before merging it. It's a little different than just a regular rebase. You start off with the same scenario as before... but instead of keeping all the commits as you replay them, you can squash them all into a single commit.

let's take a look at how this is actually done.
* Understand why you're squashing - is it really just one commit?
* Fetch all the changes from the remote repository to make sure you're rebasing against the latest code.
* Run the interactive rebase.

* Imagine I have a branch called “convert-hashes” where I'm updating all the hashes in my Ruby code to use the newer syntax.
* Without debating the practicality of this, let's assume it took me 5 commits to get it right. I can use git log to see a list of all my commits.

Next, I issue the interactive rebase command. It's just using the -i option and then specifying the branch that I want to rebase against. This is assuming that I have a remote repo called origin that I want to integrate with. You see all my commits listed at the top with the word “pick” next to each one. Below that is a description of what you can do with that commit.
* This is vim by the way, but you can use whatever editor you want.

In my case, I usually want to have a single commit and squash all of them into this one. I use the "reword" command on the first commit, and "fixup" on the others.
* This is saying, let me change the commit message of the one that I keep and discard all the others.

once I save that, it takes me to another screen where I can change the message for the first commit.

since this is going to be a production commit, I am using the format that I described above for my message and then save the file again and let git take care of the magic behind the scenes.

if you look at the single commit after, and compare to the initial commits before, you can see how it has squashed a bunch of useless messages together into a single cohesive story about what you changed. From here, you can merge your branch back in.

#### Pull w/ Rebase

```
$ git pull --rebase origin master
```

Under the hood, the default behavior of `git pull` is a combination of `git fetch` and `git merge`. In some cases this is desired, but since we're trying to avoid unnecessary merge commits, we're not going to use it this way.

### Amending Commits

The final method we'll look for rewriting history is not rebasing, but instead is part of Git's "commit" command. Git allows you to amend an existing commit which is useful for fixing typos in code or commit messages, add files you forgot, or even modify the author.

The amend command looks something like this:

```
$ git commit --amend
```

According to Git's documentation, it is roughly equivalent to a soft reset:

```
$ git reset --soft HEAD^
... make changes ...
$ git commit -c ORIG_HEAD
```

Now let's look at a real use case where I've committed a change, but have a typo in my commit message.

```
commit 4d676c4ab363e75d0db269329f504e95de139f0d
Author: beerlington <pete@lette.us>
Date:   Fri Jan 17 08:38:14 2014 -0500

    Adding the cats fle

 cats | 0
```

As you can see above, I misspelled the word "file", which I'm really embarrassed about. As we learned in the previous section, I could fix the error by using `git rebase`, and rewording that commit, but `git commit --amend` requires much less thought.

If this is my latest commit, I can run the following command:

```
$ git commit --amend -m "Adding the cats file"
```

And when we look at the result of that amended commit in git log:

```
commit 989fe9f84283d145029d80a2c571a8bbaa01b7eb
Author: beerlington <pete@lette.us>
Date:   Fri Jan 17 08:38:14 2014 -0500

    Adding the cats file

 cats | 0
```
 
A few important things to note about the output above is that the commit SHA is different, but the Author and Date are exactly the same.

As with rebasing, you need to understand the implications of rewriting history.

### Protips for Rewriting History

Here are a few things I've learned about rebasing that can save you lots of headaches.

#### Always fetch before rebasing

This might seem like an obvious tip, but the reality is that it's an easy step to forget since it's usually the last step before merging all your hard work into master. If you do happen to forget to fetch before rebasing, it won't really cause any harm because you can repeat the rebase as many times as needed. However, it might lead to confusion when you discover that you still have conflicts between your remote master branch and your local branch.

If you're forgetful like me, you might want to setup a Git alias to use that performs a fetch before rebasing:

```
$ git config --global alias.squash '!git fetch && git rebase -i origin/master'
```

With the above alias in place, you would then issue the rebase command like so:

```
$ git squash
```

#### Never rebase a branch someone has based work off

#### Prolong squashing as long as possible

#### Don't be discouraged

#### Know when to use which tool

#### Rebasing is not as scary as it seems

I've been squashing commits for at least 2 years and have never unintentionally lost code that I cared about. Since branches are so cheap, you can always create a backup copy before squashing so that you can switch over to it in the event that something goes very wrong. We'll also be looking at the reflog in a bit which we can use to recover lost commits. So it can cause a headache, but nothing you can't recover from.

## Merging

### Basics of merging


```
$ git checkout master
$ git merge my_branch --no-ff
$ git branch -d my_branch
```

### Resolving Merge Conflicts