# Why Version Control

Throughout this book I'll be discussing why your project's history is important, and how you, the eager web developer, can get the most out of a version control system like Git. Before I do though, I thought it would be useful to take a holistic look at what version control is, because it's more than just a system for versioning your code as the name implies.

Developers often view version control as a necessary evil - or in some cases, just evil. I've spoken with developers who have described situations where they've been forced to use systems like Git and have dreaded every minute of it. They learn the basics of how to commit and branch, but are often left frustrated by their experience. They'll say things like "I hate Git!", putting blaming the tool itself, rather than stepping back and examining what it is about the experience they truly despise.

One of the biggest issues I've seen is that teams don't give version control the same "respect" that they give to their code. That is to say, they don't follow any sort of best practices and don't treat VC as part of their craft.

If used properly, version control can be the long-term memory of your code base, recalling things in a moment's notice. As we'll get into later, certain practices can make this experience less painful, and considerably more productive. It can be a powerful tool to help us collaborate, communicate, experiment and gain confidence as developers. Like any habit though, we need to make a conscious effort and a long term commitment to these behaviors.

I've occasionally heard complaints that tools like Git slow down the development process, and following guidelines and best practices makes this process even slower. Except for during a hackathon, software development should not a race to write the most code you can write in the shortest amount of timea. Good development is about creating value not just today, but during the entire life span of the application. This almost always means doing it in a sustainable way because often times whoever did the original code is not who will be maintaining it six months down the road.

What follows is a brief overview of some of the benefits of version control that aren't normally discussed, but are just as valuable any others.

## Collaborate

Many of us use version control as a means to collaborate with other developers. We may be working on a project or want other people to collaborate on our code such as the case with open source framework like Ruby on Rails. Services like Github are even built around the concept of collaboration.

It's a tool that allows us to work asynchronously and reduce the amount of friction when working in teams. Some teams believe that pair programming 100% of the time produces the best possible software. This may be true in certain circumstances, but I believe we need reflection time, especially introverts. Pair programming can overstimulate us sometimes and cause us to wear down more easily. Collaborating asynchronously via code reviews gives us this reflection time that can bring new perspectives that we wouldn't have otherwise gotten.

We can propose changes or new features without long term commitment. We can get feedback from developers using GitHub's pull request feature doing code reviews. We'll be discussing these concepts later on when we get into branching and workflow strategies.

## Communicate

Poor communication is often cited as a leading cause (sometimes *the* leading cause) of project failure. Everything from high level requirements down to who's writing which new feature depend on proper communication. Reducing the effort it takes for teams to share ideas and build features can reduce the likelihood that a project will fail.

Git helps facilitate communication in a number of different ways.

As discussed above with regards to collaboration, online code reviews allow developers to communicate in an asynchronous way that was not possible in the past. The issue with depending entirely on code reviews is that they are only performed once, when code and intent are fresh in the developer's mind. It only takes a few weeks for 

## Experiment

Experimentation is the foundation for new discoveries and breakthroughs in science. Without experiments, science would never progress. Experiments in software development (even without structure) can have substantial impact on the final outcome of an application. However, if developers are afraid to because they might break something or introduce a feature that might not be needed, they'll be less likely to want to experiment.

Git provides the freedom to experiment via branches, which we'll discuss in detail later in the book. Experiments can range from something as simple as a few lines of throwaway code (a "code spike"), to a series of different approaches to solving the same problem. They may also uncover new functionality that may go on to be a core feature.

## Gain Confidence

Our job as developers is to write code that solves problems in an effective way. Effective meaning within a budget or timeframe, and in a way that addresses real world needs. We must constantly move quickly and adapt to changing requirements, and in order to do so, we need to have confidence that things are going to work tomorrow, next week, or even 5 years from now. We need to be confident that everything will work when you leave the project and pass it off to other developers.

These concepts do not just apply to the code itself. Git provides  us with the tools to write a project's history in ways to feel more confident.

## Safety Net

One of the ways it builds confidence is by acting as a safety net as we are coding. Sometimes we introduce a bug and need to revert it. Or we introduce a change that we just don't like. We lose code, make mistakes, want to try new things without taking a risk and breaking things. We’re human. Git makes it easy to recover from issues without getting in the way.