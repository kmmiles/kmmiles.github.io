---
layout: post
author: Beef Broccoli
title:  "Why you should use tmux"
date:   2022-04-12 12:26:38 -0600
categories: tmux
---

![tmux-example]({{ site.url }}/assets/img/tmux-example.gif)

Ever lost your internet connection while `ssh`ing into a remote host?

If your terminal closed right now, your shells (and any programs running said shells) would be terminated.

But if you were using `tmux`, your shells and programs would still be running.

You also get cool tabs, screen splitting and a stupid bar with a clock on it! 

## Installing tmux

You'll find it in nearly every package manager as `tmux`.

See here for more details: <https://github.com/tmux/tmux/wiki/Installing>

## Install / configure your terminal font

Most `tmux` themes (the good looking ones) use glyphs that require a special font.
If you haven't already, download and install a patched font of your choosing here: <https://github.com/ryanoasis/nerd-fonts#patched-fonts>.
Then configure your terminal to use that font.

The font in the screenshots is `Fira Mono Nerd Font`.

## Installing TPM (tmux plugin manager)

{% highlight bash %}
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
{% endhighlight %}

## Updating tmux config

This is a reasonable default config. Write these contents to `~/.tmux.conf`.

```tmux
# uncomment below to use `zsh` or your favorite shell by default
#set-option -g default-shell /usr/bin/zsh

# enable truecolor mode
set -g default-terminal "xterm-256color"
set-option -ga terminal-overrides ",xterm-256color:Tc"

# change the prefix from C-b to C-a. these are the keys used by `screen`, so it's easier for converts.
make C-a the prefix
unbind-key C-b
set-option -g prefix C-a
bind-key   a send-prefix

# enable mouse. clicking tabs will work, if your terminal supports it.
setw -g mouse on

# bind space to "next window"
bind-key -r Space next-window
bind-key -r "C-Space" next-window

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

## Installing tmux plugins

Install the tmux plugins on the command line:

```bash
~/.tmux/plugins/tpm/bin/install_plugins
```

## Create a `mux` alias

Add this to your `~/.bashrc` or `~/.zshrc`:

`alias mux='tmux new -A -s tmux'`

Replace the session name `tmux` with anything you like - it is the text displayed in the lower left corner.

## Running for the first time

Restart your terminal and run `mux`:

![tmux-first-launch]({{ site.url }}/assets/img/tmux-first-launch.png)

## Navigating `tmux` and shortcut keys

All `tmux` commands work by holding `Ctrl` and pressing `a`, followed by the desired shortcut letter.

- `c`             create a window (tab)
- `n` or `space`  move to next window
- `p`             move to previous window
- `,`             rename current window
- `r`             reload `tmux.conf`
- `I`             install plugins

Try creating a couple of windows, and cycling between them for practice.
It only takes a few moments to figure out.

## Other themes

Don't like `nord`? There's others. Here's some alternatives:

 - Some plain color variations: <https://github.com/jimeh/tmux-themepack>
 - Gruvbox: <https://github.com/egel/tmux-gruvbox>
 - Dracula: <https://github.com/dracula/tmux>

## Other tmux plugins

If a clock isn't good enough for you, you can add more stupid stuff to tmux like battery power, cpu, temps, etc.

See tmux plugins here: <https://github.com/tmux-plugins>

## Windows Terminal and WSL2

Here's a neat trick to always open WSL2 with `tmux` using `Debian` as an example.

![tmux-open-close]({{ site.url }}/assets/img/tmux-open-close.gif)

- Open Settings in Windows Terminal
- Click `Add New Profile`, copy from the existing `Debian` profile, and name it `Debian (tmux)`
- Change the `Command Line` option to: `wsl.exe -d Debian /usr/bin/tmux new -A -s WSL2`
- Optionally make it the new default terminal in `Settings -> Startup`
