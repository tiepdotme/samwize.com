---
layout: post
title: "Load dotfiles .aliases in zsh"
date: 2014-01-19 12:10:37 +0800
comments: true
categories: Mac
---

In the last 2 posts, I setup [dotfiles](/2014/01/12/getting-started-with-dotfiles/) and [zsh](/2014/01/15/getting-started-with-zsh/).

Right after setting up zsh, all the aliases in dotfiles no longer works.

<!-- more -->

To fix, you can add this in `~/.oh-my-zsh/lib/aliases.zsh`:

```bash
source $HOME/.aliases
```

Or simply fork [my zsh version](https://github.com/samwize/oh-my-zsh/).

## Reload Dotfile

You will have to go to your dotfile folder, then `source bootstrap.sh`.

But you will like have this [error](https://github.com/mathiasbynens/dotfiles/issues/230):

    bootstrap.sh:9: = not found

Since we are using zsh, you need to change to `sh bootstrap.sh`.
