# Commits

Commits are the ground work of your codebase. A commit represents one step
forward in the history of your codebase. It contains a snapshot of your codebase
at the time when the commit was made. Commits are what allows Git to be such a
powerful tool. There are many different kinds of commits, and there are
definitely great commits and not-so-great commits. You want to take the time to
make wonderful commits.

The difference between a poor commit and a great commit makes the world of
difference in your codebase. Wonderful commits help you easily find where a
problem was introduced and what drove the introduction of that problem. It can
help pinpoint who introduced the error and under what circumstances it was
introduced. A poor commit can mask where an issue was introduced, making it
difficult to use commands like `git bisect`.

## What Makes a Commit

You must work with Git to create a great commit. Git gets your started by
generating a unique hash identifier for your commit. Git also adds in the author
and date of the commit if you have that configured. What Git does not handle,
arguably the two key parts of the commit, is the commit message and the changes
to the code base that are committed.

Commits should be concise but also have substance. A commit may composed of many
different code changes, but only the changes related to each other should be in
a single commit. The first part to creating a great commit is committing the
right files. The second part is writing a wonderful commit message.

### What Changed

Understanding what to add to a commit can be challenging. It depends on what
type of commit you are making, but the general rule of thumb is to only commit
the changes that are relevant to what you are working on. If you notice that
there is some missing documentation for a method in class that is not related to
what you are working on, do not add that to the commit. Once you have finished
your working commit, go ahead and create a new commit with that documentation
addition.

A useful command for manually selecting what changes to add is `git add --patch`
or `git add -p` for short. `git add -p` lets you choose what hunks of the patch
to add to your index. `git add -p` is also a great way to review your code and
what is going to be committed before committing it. It progresses through each
hunk, presenting you with handful of options for that hunk.

``` sh
Stage this hunk [y,n,q,a,d,/,j,J,g,s,e,?]? ?
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk nor any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk nor any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
? - print help
```

`y` and `n` are fairly obvious, but it is helpful to call out the `s` and `e`
answers. `s` splits a hunk up into smaller hunks. If two unrelated changes are
close to each other, Git may lump them into one hunk. The `e` command opens up
your default editor in Git to edit that hunk. Notice a typo when adding hunks?
`e` allows you to quickly change it and hop back to your `git add -p`.

Note: `git add -p` does not add new files to your index to be committed. You
will need to add those manually.

Once your files are staged to be committed, it is time to write the explanation
of the commit.

### Writing Wonderful Commit Messages

I often view commit messages as a detailed journal of what your codebase has
been through. While code may speak for itself, without context, it can be
confusing why something was changed or added. The solution to this issue is the
commit message. You need to think of yourself as a code historian. You want
everyone else hanging out in the museum of your codebase to know what happened
and why. This is accomplished by writing wonderful commit messages.

A commit message contains two parts:

1. Summary of the commit.

  This is the first line of the commit message. It should be no longer than 50
  chars. Summaries longer than 50 characters get cuff off will get cut off by
  most interfaces that display commits. Your summary should be concise and
  explain what significance your commit has.

2. Details of the commit.

  This is what follows the summary and is separated by a blank line below the
  commit summary. There is no length limit on the commit details. Be as
  descriptive and informative as needed. If you were viewing the
  commit two years in the future, would the details you are writing effectively
  inform your future self what you were doing and why?

If you are working on a code base with an issue tracker, it can be helpful to
put the ID of issue in the tracker into your commit summary or details.

`git commit --verbose` or `git commit -v` is a super useful command to open up
your default Git editor to author the commit messages while viewing what has
changed (known as the unified diff). This lets you explicitly see what you are
committing. It helps write more descriptive commit messages because the diff is
right there.

On the other hand, there is a commonly used flag that encourages the opposite of
writing wonderful commit messages. That is the `-m` flag. `-m` allows you to
specify the commit message in that command.

```
git commit -m "Change the file."
```

Under no circumstances should `-m` be used. Take the time to view your diff,
think about what the commit accomplishes and write a descriptive message.

#### Examples of Wonderful Commit Messages

``` text
Don't allow users to join CCLA twice

This resolves an issue where users were able to
join the same CCLA multiple times. This is unexpected
behavior so we're resolving it with a little validation
on Contributor and a alert message if the user attempts this
behavior.
```

Source: [opscode/supermarket 5f43234ae421d7bb0e66568be65311d1ba6b4c33](https://github.com/opscode/supermarket/commit/5f43234ae421d7bb0e66568be65311d1ba6b4c33)

The summary of the commit details what the commit accomplishes. At first glance,
it is clear what happens. If you are interested in the specifics, there is more
information surrounding the commit.

#### Examples of Not-So-Wonderful Commit Messages


``` text
Changes stuff
```

``` text
Get tests passing
```

``` text
Fix typo
```

## Types of Commits

Commits can be classified into three different groups:

* Progress commits
* Merge commits
* Anchor commits

There is no technical different between the three different types of commits,
but what they accomplish are all different. They are all crucial in the creation
of a codebase with a solid history.

### Progress Commits

First up are progress commits. Progress commits are your standard 'I did this'
commits. They are exactly what they sound like - commits denoting progress
towards a feature. These commits should be squashed down into relevant anchor
commits, which will be covered below. Think of progress commits as detailing
your approach to completing your feature.

It is important to create progress commits in a logical way. If you are working
on a feature that has three distinct parts, let's say a background worker, a
view and a model that handles your logic, you will want to commit files that are
directly related to each other. I often think of the [Law of
Demeter](http://c2.com/cgi/wiki?LawOfDemeter) when selecting what files to add
to a commit. Only add the files that friends, the ones that are directly related
to each other. This will make squashing your progress commits into anchor
commits much less painful.

One of the most common and frustrating part of progress commits is when you
notice something wrong (and fix it) that is not related to what you are working
on. You then do `git add --all` and maybe note that there is hunk committed that
is not directly related. This can be misleading, and when squashing your
commits, it can make it difficult to properly squash commits. If you find
something wrong that is unrelated, fix it, but do not add it to the commit of
the actual code you are working on.

### Anchor Commits

Anchor commits are commits that are complete. They represent a change or
addition to the codebase in its entirety. An anchor commit may be as large as a
week of feature development or as small as a simple bug fix. Anchor commits are
what you will look back on in three years and have a solid understanding of what
changes and why.

### Merge Commits

Merge commits represent the branch topology of your codebase. They are clear
indicators of the topic branches.
