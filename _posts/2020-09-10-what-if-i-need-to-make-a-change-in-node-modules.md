---
layout: post
title: What if I need to make a change in node_modules?
tags: [open-source, github, node, electron, npm]
---

<p align="center">
  <img src="https://res.cloudinary.com/practicaldev/image/fetch/s--GNAvtrVf--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/futo9whmnw7or9e26g6k.png"/>
</p>

I have an application build on top of node/electron, and recently, because of an update in the electron, some dependencies have broken down, preventing me to upgrade to the newer versions while keeping all functionalities.

In such cases, the correct behavior would be to reach out to the dependency, and open an issue, or even submit a Pull Request, but what if your dependency is not maintained anymore, or the development process on such repository is stalled or slow, and you really need to move on with your development?

That was the scenario I was facing. The change I needed was very localized, but I could not, in a million tries, get to make it work on my fork of the dependency. To make things worst, the last commit on the repository was over 6 months ago, and no sign of a reply on the issue I opened in a week.

That's when I found [patch-package](https://github.com/ds300/patch-package), a package that lets app authors instantly make and keep fixes to npm dependencies.

<p align="center">
  <img src="https://camo.githubusercontent.com/9a1a8bbacbc7af6193cd66a89f40160b71419e34/68747470733a2f2f64733330302e6769746875622e696f2f70617463682d7061636b6167652f70617463682d7061636b6167652e737667"/>
</p>

The [README](https://github.com/ds300/patch-package/blob/master/README.md) has all the information you'll need, but I'll summarize here the idea:

1. Install patch-package (using npm or yarn)
2. Update the `scripts` rule on your `package.json` to include a call for patch-package:

    ```
     "scripts": {
    +  "postinstall": "patch-package"
     }
    ```

3. Do the change you need on the dependency, directly on node_modules
4. Call patch-package specifying the package you modified:

    ```
    (npx | yarn) patch-package package-name
    ```

    The last command will create a patch file (a diff) in `patches/`, which is a diff between the public dependency and your changed version.

5. Commit the changes, including the new patch file and everything will be working as expected.

Now, whenever you (or someone) calls install on your repository, patch-package will be called, applying that change on it.

<p align="center">
  <img src="https://media.giphy.com/media/5oGIdt1xapQ76/giphy.gif"/>
</p>

Problem solved!