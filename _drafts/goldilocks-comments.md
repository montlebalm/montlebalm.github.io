---
title: "// Goldilocks"
layout: single
summary: ""
---
Every programmer knows a bad commenter. Anyone who has collaborated on a codebase knows someone whose lack of code comments is officially a "situation". If you work on a team and you don't know anyone that fits this description, then it's probably you. A lack of comments can make navigating code feel like a game of Where's Waldo on a Chinese beach. You have to scrutinize every line just to get a feel for what's happening.

If you're a conscientious developer, chances are you've read a dozen articles urging you to properly comment your code. We've all taken the time to go back and leave breadcrumbs for the man or woman following behind so that we wouldn't be <em>that guy</em>. Unfortunately, sometimes we go too far and guess what? Writing too many comments is worse.

Copious comments remind me of the fresh blanket of power after a nights snowfall. I'm referring to scrolling down the page and seeing a light layer of comments on top of every line of code. This practice comes from a good place, but too much can be just as confusing as too little.

<h2>Too Cold</h2>

In code documentation, as in creative writing, there are many audiences to which you can direct your narrative. If you're writing an open source project to be consumed by developers of all skill levels, you might err on the side of verbosity. For the sake of this article, I'm going to assume you're on a private codebase with a team of regular colleagues. In this case, write comments as if a new hire is going to start next week and they're getting up to speed by reviewing your work. Assume this person has years of programming experience in the relevant languages and doesn't need to be taught Computer Science 101.

How many comments are too few? Put simply, your codebase is insufficiently commented if the new hire has trouble understanding your team's work. If you're not sure about whether to leave a comment, consider documenting these scenarios:

<ul><li>Using a custom object from your team's private framework</li>
<li>That fancy LINQ block you wrote that saves 20 lines of code</li>
<li>Something that doesn't serve any purpose other than setting up a future block of code in another document</li></ul>

<h2>Too Hot</h2>

Look back on the code you're working on right now. Are you documenting the behavior of built in functions and properties? If so, you're writing too many comments. Don't write anything that could be better explained in the language's manual.

If done properly, comments often signal areas of high value to a third party. By value I mean blocks of code that do enough non-obvious logic that they need to be signaled out and explained. These areas are often good places to start if you're reviewing another developer's code. In addition to obscuring value, unnecessary comments can get out of date as the underlying code changes. It's hard to regain the confidence lost when someone sees expired comments in your work.

<h2>Just Right</h2>

You'll never know for sure if you're commenting your code properly. Over time you just get a feel for what is too much or too little. If your team participates in code reviews, ask a colleague if your writing was easy to understand.

Add comments that describe blocks of code instead of individual lines. Also, don't focus on adding comments as you're actively developing code. Markup often changes substantially and it's cumbersome to edit the code as well as the underlying comment. When you get to a stopping point, go back over what you've done and identify the