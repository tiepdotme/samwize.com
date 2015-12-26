---
layout: post
title: "My Vim Setup"
date: 2015-12-21T12:51:26+08:00
categories: [vim]
published: false 
---

## Install vim with +clipboard

The vim that comes with Mac OS was not built with clipboard support. That means you have no access to system clipboard in their vim!

Therefore, the first step is to install a better version of vim.

	brew install vim


## Vundle to manage vim plugins

[Vundle](https://github.com/VundleVim/Vundle.vim) is a plugin manager for vim.

Firstly, `git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim`.

Since I use [dotfiles](http://samwize.com/2014/01/12/getting-started-with-dotfiles/) and [oh-my-zsh](http://samwize.com/2014/01/15/getting-started-with-zsh/), there will be some setup to them. With oh-my-zsh, enable vundle as one of the plugins in `.zshrc`.

Then in `.vimrc`, add the following: 

```
filetype off                  " required
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
Plugin 'ctrlpvim/ctrlp.vim'
call vundle#end()            " required
filetype plugin indent on    " required
```

In the example, I have only added [ctrlp](https://github.com/ctrlpvim/ctrlp.vim) for illustration of a plugin to install. Add more as needed.


You can now install with `:PluginInstall`.


## Symlink dotfiles

A convenient way is to symlink `.vimrc` so that you only edit in one place (dotfile):

```
ln -s ~/dotfiles/.vimrc ~/.vimrc
```


## Solarized Theme

We can't not mention the most popluar theme - solarized.

Add the plugin to `.zshrc`, and install with `:PluginInstall`.

    Plugin 'altercation/vim-colors-solarized'

Remember to also change the [theme for iTerm2](https://github.com/altercation/solarized/tree/master/iterm2-colors-solarized).


## Agnoster/Powerline zsh theme

While we are at theme, you might as well change your zsh theme to [Agnoster](https://gist.github.com/agnoster/3712874), a theme that is compatible with Solarized and good if you use git. Change your `.zshrc` theme to `agnoster`.

You will need to install [powerline fonts](https://github.com/powerline/fonts), then change your iTerm non-ASCII font to eg Menso for Powerline.

