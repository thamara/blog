---
layout: post
title: Contributing to Open-Source, a brief how-to
tags: [open-source, github, git]
---

October is here, so it is [Hacktoberfest](http://hacktoberfest.digitalocean.com/), an event by DigitalOcean in partnership with Intel and Dev to celebrate Open Source and get more people involved with it.

As someone who found in open source a lot of fun and knowledge, I'm always talking to others to join this party. Still, usually, I get the following answers:

- I don't know Git and Github...
- I don't know how to program...
- Where do I even find a project to start with?

To answer all of that, and much more, I got together with Tulio Leão, a dear friend that shares all my excitement regarding open-source, and we hosted a virtual Meetup on Youtube to answer all of that.
If you are a Portuguese speaker, you can check the recording [in here](https://www.youtube.com/watch?v=J_HAsKN_m14&ab_channel=ThamaraAndrade).

In this post, I will cover a summary of the event, hoping it can be useful for someone who wants to start contributing to open source.

## Git

Git is the kind of skill that, doesn't matter what you do, mastering it should be useful. If you are a developer, you probably already know Git or any other form of source control, and that's great! The hardest part of learning git is understanding the concepts; if you already know something, it should be a matter of adapting to the commands.

Explaining Git from the beginning should take more than one post, so I leave this to you. I suggest you start and my new favorite game-ish site [Learn Git Branching](https://learngitbranching.js.org/), where it will explain you the concepts of Git, step by step, and you can practice directly on the site (no need to worry of messing something up). [Git's](https://git-scm.com/book/en/v2) own page is also a great resource, with a completely free book on git commands.

## GitHub

As you would expect, GitHub is related to Git, in a way that it's a hosting platform for version control (usually Git) and collaboration. Basically, it's a source of repositories where multiple people can work simultaneously. Besides this, it also adds lots of functionalities to improve project management, such as an issue tracker, continuous integration, wikis, and much more.

The best way to learn how to use GitHub is by actually using it. So let me walk you through what a usual work is using GitHub:

### GitHub Workflow

1. First of all, we need to find a project (there will be more tips at the end on how to find those)
2. When you find one project you want, you can browse around, check the code, the issues, what people are doing, until you find something you feel comfortable taking
3. You fork the repository to your account, kind of creating your own copy of it for you to clone and modify
4. In your fork, you start a new branch for your modification
5. Now it's time you start coding or doing whatever was the task you meant to do; this will become commits in your own fork of the project
6. With your changes done, you push the commits to your fork
7. From your fork, you open a Pull Request (or PR for simplification); this is saying to the maintainers of the project that you did something and is ready to share with them
8. The maintainers (or actually anyone interested) will review your changes and point to some things you need to change. This step might take some time and some iterations until it's all good.
9. With the change approved, now it's up to the project's maintainers to merge your PR, thus making it available in the standard branch of the project, and guess what?
10. You are done!

The flow can change from project to project, but this is considered the typical case. All of this is beautifully illustrated and explained in [GitHub's Understanding the GitHub flow](https://guides.github.com/introduction/flow/).
In my presentation, I illustrated this scenario; you can check it [here](https://github.com/codiqueijo/recursos/blob/main/2020-10-01%20-%20Uaiktoberfest/02%20-%20Uaiktoberfest%20-%20Git%2C%20GitHub%20e%20Hacktoberfest.pdf).

## Coding is not the only skill required to contribute

When we think about open-source, usually we imagine a developer or someone who knows to program. If you don't know, you may think open-source is not for you. Well, this is entirely incorrect. There are many different roles in projects, and for sure you can find something you can help.

Here are a few examples (there are many more) of roles you can assume to contribute to open source:

- As a user: Yes! Open source projects need users, especially those that are willing to give feedback on the project. This can be opening issues, proposing enhancements, and a lot more. As a user, you are not required to understand a single line of the code or the project's infrastructure.
- As a manager: Most projects have some sort of a software development process established in terms of releases, task organization, features prioritization, and more. You can help managing it, with a higher-level view of deliveries and again, without actually needing to code a thing.
As a translator: Lots of projects have a way of localizing its content. You can help translate it to the languages you are familiar with or even just reviewing previous translations.
- As an artist: At some point in my life, I heard that developers don't know how to draw. Although I disagree this is true, many projects out there will benefit from someone with a more artistic view. This means that there are tasks for you to work on the branding or on specific components that need an artsy touch.
- As a documentarian: if there is something that any project is missing at some level, it is documentation, not only on the code itself. We are talking about the documentation of the tool or whatever is the product of the project.
- As a reviewer: Code reviews are essential, and on the open-source, I think it's vital. But many times, the maintainers of the project cannot undertake all the work, and you can help them by doing reviews!
- As a developer: that's the obvious one, but it's not apparent that you don't need lots of experience in a language/technology to contribute. You can even use it as a way for you to improve your skills.


## Where to start

Finding a project to start is maybe the most challenging step when we talk about open source. My first suggestion is to check the products you already use; chances are that some of those are open source. Because you already use them, it will be easier to understand the project's scope and intent. Sometimes this can be a little challenging, so there are other paths.

Under a project on GitHub, you'll find that the issues have different labels, some of those with great names such as "good-first-issue" or "first-contributors", and more, indicating those that are more straightforward tasks that you can work on to get you more comfortable with the project and the flow.

You can search for these labels directly on GitHub, or you can check the pages below:

- [Up for grabs](https://up-for-grabs.net/#/)
- [Good First Issues](https://goodfirstissues.com/index.html)
- [Awesome for Beginners](https://github.com/MunGell/awesome-for-beginners)

As this is my post, I can do some advertising some of my own projects too, right?!

- [Time to Leave](https://github.com/thamara/time-to-leave): A working hours tracker that notifies you when it's time to leave. It's based on Electron and written in JavaScript. You can help with development, translation, testing, and whatever you feel like, really.
- [Code Annotation, a VSCode exntension](https://github.com/thamara/vscode-code-annotation): A VSCode extension that allows you to create annotations (like todos) on your code, without actually committing the changes. As this is something, I have started recently, any kind of help is welcome!

## Final thoughts

I hope this has inspired you a little bit more to contribute to open source. I've been actively contributing to several projects, including creating my own for more than a year now, and I can't even measure how much I learned during this time. Especially over the past months, due to Corona and such, this has been the hobby that has helped me keep my sanity.

If you have any questions, feel free to get in touch on Twitter over [@thamyk](https://twitter.com/thamyk). I would be glad to help you on this journey!