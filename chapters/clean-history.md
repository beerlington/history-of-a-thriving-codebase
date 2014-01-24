# Clean History

There are countless benefits to using Git over an alternate version control system (VCS) like Subversion. However, the purpose of this book isn't to try and persuade you to switch Git. It assumes you've already either made the switch, or perhaps Git is the only VCS you've ever used (lucky you!). You are convinced of its powers and just want to be a better developer.

Throughout the remaining chapters we'll explore some of my favorite attributes of using Git, and how they're related to the history of your code. In this chapter, we will start by discussing the concept of code history, learn why it's important and examine the key differences between "clean" and "unclean" history.

## What is Code History?

Code history is a series of changes to an application over time. It is a story to future developers about how and why your codebase has evolved. Each change in that history represents a decision that was made by a developer to manipulate the code in some way. Some changes are significant, while others may be very minor.

Many times these changes contain comments embedded in the code. Some of you may have experienced the dreaded "out of date comment" where the comment says one thing, but the code is doing something completely different. One reason this happens is because developers are so focused on what is going on with the code itself that they forget to update the comments too. This is especially common in internal applications where the source code is not shared with external developers, and documentation tends to be neglected and undervalued.

Fortunately almost every version control system allows you to leave a comment when the change is made. These commit comments will never be out of date because any time the code changes, you *must* leave a new comment.

Every version control system is basically the same on the surface. You can simply use it for just making changes to files and pushing to a remote repository where those changes are shared across a team. There's nothing wrong with using it this way, but you're not getting the most out of it.

Every time you commit, you're creating a new snapshot of your codebase. You can think of a commit as a backup. Over time, your repository accrues many of these commits, each representing some sort of change to the code.

## What is a Commit?

With Git, a commit is literally a snapshot of your code, not just the changes. You can roll back in time to any previous snapshot without losing your work (more on this later). It's easy to think about a commit as just being the change to the *code*, however, it is really about the code change *and* the commit message.

Commits can be something as simple as fixing a typo or refactoring a chunk of code. It might be to update some documentation for existing code, without actually changing any code. Or even adding a new feature.

## What is Clean History?

This book has mentioned the term "clean history" a few times, but hasn't defined it yet.

I recently got a new device called a Fitbit Flex. You wear it around your wrist and it records how many steps you take, how far you walk, etc. One thing it does really well is automatically record and organize the data into a useful way. Each day you can see how close to your goals you were, and never have to think about formatting the data because it handles this for you.

Unfortunately, when developing software, we don't have something that can automatically record what we've done in a way that is as meaningful as the Fitbit does it. There isn't a magical tool that cleans up our poor commit messages and makes each commit useful. It is the responsibility of us as developers to put effort into recording the project's history.

### Why Record History?

When recording any kind of history, we want to be able to see what happened, to determine why, and then try and make better decisions about what to do in the future. The same goes for the history of our code.

I was recently working on a project that had been in development for a few years. I was looking at some funky code and couldn't figure out how it was supposed to work. Looking at the commit that introduced the code was not very useful because it didn't contain any tests and I couldn't figure out where it was actually used in the application.

Every commit I looked at said things like "fixing test", "fixing typo", "misc changes" and it took me an hour to track down how the code was supposed to work. If the developer had made an effort to maintain a clean history, it would have saved me a lot of time.

### What is Clean History?

What exactly is a clean history? It means that every commit is a solid piece of work, representing a milestone. Every change adds some sort of value and only includes information that may be useful in the future.

### Reducing Complexity

It's really about reducing the complexity of your history in the same way that adhering to principles like KISS, YAGNI and DRY help reduce complexity in the code itself. There's a theory discussed in the book the Pragmatic Programmer that a single broken window can lead to a sense of abandonment, which becomes reality. In terms of software, this causes "rot". I believe this concept also applies to the history of an application. By reducing the complexity of your history, you're demonstrating you care about your project.

### Why Clean History?

Why do we need this? Clean history can make it easier to track down why changes were made, which leads to faster bug fixes and a better development experience overall. It's even required on some open source projects, such as Rails. They won't merge pull requests until they've been cleaned up, and they are very strict about this.