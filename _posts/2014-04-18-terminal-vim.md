---
layout: post 
title: Terminal & vim with solarized 
keywords: tmux, oh-my-zsh, vim, solarized 
tags: tmux vim
description: tmux is a terminal multiplexer,Vim is a highly configurable text editor.
---
<h3>Getting Started</h3>
<p><b>Step 1:</b> install oh-my-zsh</p>

<pre>
$ sudo apt-get install zsh 
$ wget --no-check-certificate http://install.ohmyz.sh -O - | sh

#change your shell to zsh
$ chsh -s `which zsh`
</pre>

<p><b>Step 2:</b> install and config tmux</p>

<pre>
$ sudo apt-get install tmux 
$ vim .tmux.conf

#.tmux.conf
unbind C-b
set -g prefix C-a
setw -g mode-keys vi

# split window like vim
# vim's defination of a horizontal/vertical split is revised from tumx's
bind s split-window -h
bind v split-window -v
# move arount panes with hjkl, as one would in vim after C-w
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# resize panes like vim
# feel free to change the "1" to however many lines you want to resize by,
# only one at a time can be slow
bind < resize-pane -L 10
bind > resize-pane -R 10
bind - resize-pane -D 10
bind + resize-pane -U 10

# bind : to command-prompt like vim
# this is the default in tmux already
bind : command-prompt
</pre>

<p><b>Step 3:</b> setup terminal and vim color: solarized</p>

<pre>
## setup terminal dircolors 
$ git clone git://github.com/seebi/dircolors-solarized.git
$ cp ~/dircolors-solarized/dircolors.256dark ~/.dircolors
$ eval 'dircolors .dircolors'
$ source .bashrc

## setup terminal color:solarized 
$ git clone git://github.com/sigurdga/gnome-terminal-colors-solarized.git
$ cd gnome-terminal-colors-solarized
$ ./set_dark.sh

## config vim
# install vundle
$ git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
# copy my_vimrc to /etc/vim/vimrc
# my_vimrc: https://github.com/zmdyiwei/my-config-file/blob/master/my_vimrc
# move solarized.vim to /.vim/colors/
# solarized.vim: https://github.com/zmdyiwei/my-config-file/blob/master/solarized.vim

## config vim in tmux color: solarized
# add line to .zshrc
alias tmux="TERM=screen-256color-bce tmux"
# add line to .tmux.conf
set -g default-terminal "screen-256color"
</pre>

<p><b>Step 4:</b> config neocomplcache work with "sudo vim"</p>

<pre>
# comment these line in: .vim/bundle/neocomplcache/plugin/neocomplcache.vim
if v:version < 702  
  echohl Error  
  echomsg 'neocomplcache does not work this version of Vim (' . v:version . ').'  
 #echohl None  
 #finish  
 #eseif $SUDO_USER != '' && $USER !=# $SUDO_USER  
 #    \ && $HOME !=# expand('~'.$USER)  
 #    \ && $HOME ==# expand('~'.$SUDO_USER)  
 #echohl Error  
 #echomsg 'neocomplcache disabled: "sudo vim" is detected and $HOME is set to '  
 #      \.'your user''s home. '  
 #      \.'You may want to use the sudo.vim plugin, the "-H" option '  
 #      \.'with "sudo" or set always_set_home in /etc/sudoers instead.'  
  echohl None  
  finish  
endif  
</pre>

<p>Reference:</p> 

<pre>
<a href="http://blog.csdn.net/angle_birds/article/details/11694325">http://blog.csdn.net/angle_birds/article/details/11694325</a>
<a href="http://happycasts.net/episodes/41">http://happycasts.net/episodes/41</a>
<a href="https://gist.github.com/tsabat/1498393">https://gist.github.com/tsabat/1498393</a>
<a href="http://rhnh.net/2011/08/20/vim-and-tmux-on-osx">http://rhnh.net/2011/08/20/vim-and-tmux-on-osx</a>
<a href="https://geakit.com/lilydjwg/dotvim/commit/eeae45ffcb380b6a8d5643f8a3c052c61bbf694e">https://geakit.com/lilydjwg/dotvim/commit/eeae45ffcb380b6a8d5643f8a3c052c61bbf694e</a>
</pre>
