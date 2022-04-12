---
layout: post
title:  "Why you should use tmux"
date:   2022-04-12 12:26:38 -0600
categories: tmux linux ssh
---

# Why you should be using `tmux`

Have you ever had to `ssh` into a machine? 

Ever open multiple terminals/tabs/connections to do more then one thing at a time?

Or more importantly: Have you ever had a power/network outage or otherwise had your terminal close,
and subsuqently killed that database dump or `cp -R` halfway through?

This doesn't only apply to `ssh`ing into remote servers. Try closing your terminal right now:
everything currently running will be sent a `SIGHUP` (Hangup Signal) aka it dies.

With `tmux` you get tabs built into your shell, and your programs keeps running even if you
close the terminal or lose your network connection.

## Installing tmux

You'll find it in nearly every package manager as `tmux`.

See here for more details: <https://github.com/tmux/tmux/wiki/Installing>

## Install / configure your terminal font

Many `tmux` themes use glyphs that require a special font.
If you haven't already, download and install a patched font of your choosing here: <https://github.com/ryanoasis/nerd-fonts#patched-fonts>.
Then configure your terminal to use that font.

I personally use `Fira Mono Nerd Font`.

## Running

To start: `tmux`

This is where people get confused. It's extremely bare out of the box, and the shortcuts are a bit confusing.

So, we're going to install a few plugins and a reasonable config.

Go ahead and exit `tmux` now with `exit`.

## Installing TPM (tmux plugin manager)

Run:

```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

...and you're done.

## Updating tmux config

This is my default tmux config. Write these contents to `~/.tmux.conf`.

```tmux
# uncomment below to use `zsh` or your favorite shell by default
#set-option -g default-shell /usr/bin/zsh

# enable truecolor mode
set -g default-terminal "xterm-256color"
set-option -ga terminal-overrides ",xterm-256color:Tc"

# make C-a the prefix
unbind-key C-b
set-option -g prefix C-a
bind-key   a send-prefix

# bind space to "next window"
bind-key -r Space next-window
bind-key -r "C-Space" next-window

# enable mouse
setw -g mouse on

# set clock format
setw -g clock-mode-style 12

# bind 'r' to reload config
bind r source-file ~/.tmux.conf \; display-message "Config reloaded..."

# install plugin stuff
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin "arcticicestudio/nord-tmux"
run '~/.tmux/plugins/tpm/tpm'
```

### NOTES 

- The "prefix" is changed to `control+a` from the default `control+b`. This is to be consistant with `screen`, which `tmux` essentially replaces.
- The mouse is enabled to facilitate copying/pasting, as well as switching tabs with the mouse.
- `nord` is my chosen theme, but there are many alternatives, here's a collection: <https://github.com/jimeh/tmux-themepack>

## Install tmux plugins

Install the `tmux` plugins with: `~/.tmux/plugins/tpm/bin/install_plugins`

Now launch `tmux` again.

## Navigating `tmux` and shortcut keys

All `tmux` commands work by hitting the prefix (`control + a`), releasing the keys, followed by hitting another key.

- `c`       create a new tab
- `n`       move to next tab
- `space`   move to next tab
- `p`       move to previous tab
- `r`       reload the `tmux.conf`
- `I`       install plugins

This is essentially all you really need to know.

## Create a `mux` alias

I like to use an alias, so `mux` is the only thing I need type. It will either attach your main session, or create it if it doesn't exist.

`alias mux='tmux new -A -s km'`

Here i'm naming this session `km` because those are my initials. Feel free to choose a name of your liking.

Now plop your alias at the bottom of `~/.bashrc` or `~/.zshrc` if you're using `zsh`.
