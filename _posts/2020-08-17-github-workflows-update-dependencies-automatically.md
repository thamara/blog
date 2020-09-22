---
layout: post
title: GitHub Workflows: Update dependencies automatically
tags: [open-source, github]
---

<p align="center">
  <img src="https://res.cloudinary.com/practicaldev/image/fetch/s--iT4C0UDo--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/w0j6bsx5v4g9b2pytr5v.png"/>
</p>

In this new series, I'll be sharing what are the Workflows I use that help me maintain my projects. Not only I'll be sharing what actions I use and how they work, but I'll also give tips on how to trigger them, as well as the advantages and the drawbacks they have. Maybe some of them will not be ready to use in your project, but they might have some cool ideas for you to reproduce in your environment.

----

- [**Update dependencies**](#Update-dependencies)
  * [Dependabot: What GitHub gives you for free](#Dependabot-What-GitHub-gives-you-for-free)
  * [Not-so-automatic solution: Npm Check Updates](#Not-so-automatic-solution-Npm-Check-Updates)
  * [(Almost) The one: Action package update](#Almost-The-one-Action-package-update)
  * [The ultimate combination: taichi/actions-package-update + GitHub Apps](#The-ultimate-combination-taichiactions-package-update-GitHub-Apps)
  * [The final workflow](#The-final-workflow)
  * [Final thoughts](#Final-thoughts)

---

# **Update dependencies** <a name="Update-dependencies"></a>
In this first article, I'll be talking about the workflow for automatically updating the dependencies in a node-based project. I'm confident it can be useful for different stacks too.
I think we all can agree that it's important to keep dependencies updated, right? The updated version of the libraries includes security and bug fixes, as well as some really good enhancements, and we want to take advantage of that all. If you are one of the few that doesn't believe that, I suggest reading this [page from Dependabot](https://dependabot.com/blog/why-bother/) on why bother keeping dependencies up-to-date before continuing.

## Dependabot: What GitHub gives you for free <a name="Dependabot-What-GitHub-gives-you-for-free"></a>
Now that we are all on the same page on why updating the dependencies is important, let's talk about how.
[GitHub has acquired Dependabot](https://dependabot.com/blog/hello-github/), a tool that does automated dependency updates, in 2019, and made it free for all public open-source projects in the platform.
If you have a project that fits the criteria, most likely you have received a PR at some point from Dependabot, probably regarding an updated that fixes a security vulnerability, something that looked like:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/gefmcg08yyg90mkgng3n.jpg)

###### From [thamara/time-to-leave/pull-320](https://github.com/thamara/time-to-leave/pull/320)


**Drawbacks**:
- The focus are updates that improve security
- Sometimes it was [trying to update some dependencies that were not directly the ones I had](https://github.com/thamara/time-to-leave/pull/320) in my package.json, but instead, dependencies of my dependencies, which is not the correct flow.

## Not-so-automatic solution: Npm Check Updates <a name="Not-so-automatic-solution-Npm-Check-Updates"></a>
If you use node, it's possible you know about the package that reads your dependencies and searches for updates, optionally changing your package.json directly. That's [npm-check-updates](https://www.npmjs.com/package/npm-check-updates).
It's extremely easy to use, and as far as I can tell, pretty reliable too. You just install it, call `ncu -u`, and you'll see what are the changes available (with colors), and it will update your package file accordingly automatically.

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/nbt3tdi49znopuxy7ko0.jpg)

**Drawbacks**: You have to run it and commit the changes manually.

## (Almost) The one: Action package update <a name="Almost-The-one-Action-package-update"></a>

I wanted something that would combine `ncu` with a workflow, and that's when I found [taichi/actions-package-update](https://github.com/taichi/actions-package-update). To summarize, it's an action that runs `ncu`, and, when determining there's an update, will automatically create a PR on your repository.
The way they suggest to use (and also the way I use) is to have it as a workflow that runs on a schedule, which is defined using [cron's notation](http://www.nncron.ru/help/EN/working/cron-format.htm). (Check out the complete code for the workflow at the end of the article). If there's any update, you'll get a PR that will look like:

![Alt Text](https://dev-to-uploads.s3.amazonaws.com/i/3ubu00ao0a37aoljq816.jpg)

###### From [thamara/time-to-leave/pull-341](https://github.com/thamara/time-to-leave/pull/341)

You can follow the README on the [action's repository](https://github.com/taichi/actions-package-update) and configure it as suggested, but continue reading for some observations and more details on the setup I use.

**Drawbacks:**
- Sometimes it creates an ["empty" PR](https://github.com/thamara/time-to-leave/pull/339), without real modifications
- Following the suggested configuration, it will not trigger other workflows you have set up for PRs, for example, running some tests. And this for me was a show stopper.
  - This is not specific of this action, it happens to any action that will open PRs in your repository, using the default GITHUB_TOKEN.

## The ultimate combination: taichi/actions-package-update + GitHub Apps <a name="The-ultimate-combination-taichiactions-package-update-GitHub-Apps"></a>

After some research, I found out how to allow the PRs created by actions-package-update to trigger my PR checks. Basically I needed to combine the package update action with a GitHub App, which may sound difficult, but it's quite simple in practice. What you need to do is to create an alternative token, that will have some permissions to your project, thus being able to create a PR. Because it will come with a different user, it will trigger all other workflows you have for your PRs.

To setup this GitHub App, follow the instructions available [here](https://github.com/peter-evans/create-pull-request/blob/master/docs/concepts-guidelines.md#authenticating-with-github-app-generated-tokens). If you followed the instructions, you'll be left with two new secrets on your project: `APP_ID` and `APP_PRIVATE_KEY`, which can be used in the actions-package-update.

## The final workflow <a name="The-final-workflow"></a>

So, let's look at how the final workflow looks like:

```
on:
  schedule:
    - cron: '0 0 * * *'
name: Update package dependencies
jobs:
  package-update:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - uses: tibdex/github-app-token@v1
      id: generate-token
      with:
        app_id: ${{ secrets.APP_ID }}
        private_key: ${{ secrets.APP_PRIVATE_KEY }}
    - name: set remote url
      run: |
        git remote set-url --push origin https://$GITHUB_ACTOR:${{ secrets.GITHUB_TOKEN }}@github.com/$GITHUB_REPOSITORY
        npm ci
    - name: package-update
      uses: taichi/actions-package-update@master
      env:
        AUTHOR_EMAIL: tkcandrade@gmail.com
        AUTHOR_NAME: Thamara Andrade
        EXECUTE: "true"
        GITHUB_TOKEN: ${{ steps.generate-token.outputs.token }}
        LOG_LEVEL: debug
      with:
        args: -u --packageFile package.json --loglevel verbose
```
###### From [thamara/time-to-leave/.../UpdateDependencies.yml](https://github.com/thamara/time-to-leave/blob/main/.github/workflows/UpdateDependencies.yml)

- On lines 1-3 we see when the Workflow is run: every 0h of every day
- On line 7 we see that the job is run on an ubuntu machine
- Steps:
  - Lines 10-14 sets up the token to be used when creating the PR
  - Line 15-18 sets the repository remote, which will be used to create the PR and also install my application, a step that is necessary in order for ncu to run correctly
  - Line 19-28 configures the action-package-update, setting the author name and email, execute = true (true means I want a PR for this, otherwise it will just run and do nothing), the token is retrieved from the previous setup token step, and some other arguments as specified (for debug purposes).

## Final thoughts <a name="Final-thoughts"></a>
- I was able to complete my goal of having an automatic flow that checks for updates, and when there is any available create a PR for me to just merge it or not. Because the update comes as a PR, and I have some other workflows that run tests, I can have the guarantee that no update gets checkout if any test fails.
- It's possible that Dependabot does all of this, but I came up with this solution first and it works great!
- Sometimes the action still tries to create some empty PRs, but I don't think this is a big issue. On such occasions, I just drop the PR.
- The screenshots where you see GitHub in a dark theme is because of this [Chrome Extension](https://chrome.google.com/webstore/detail/odkdlljoangmamjilkamahebpkgpeacp)


Let me know what are your thoughts on GitHub workflows and what are the ones you cannot live without.
