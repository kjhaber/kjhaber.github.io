+++
date = '2026-01-03T11:26:30-08:00'
title = 'My Tmux Configs'
+++

I use the CLI terminal daily, especially on my home and work Macs, and I'm a fan of using [tmux](https://github.com/tmux/tmux) whenever I work with terminal sessions.  I won't go into describing tmux here, but if you use the terminal at all and aren't aware of tmux or other terminal multiplexers like it, I recommend checking it out.

One of tmux's strengths is its ability to customize and script its behavior, and I've customized my tmux configuration quite a bit.  There are a lot of customizations that I could share and all of them can be found in my [dotfiles](https://github.com/kjhaber/dotfiles/blob/main/.tmux.conf), but here are some of my favorites:


## tm.zsh (zsh plugin)
To lower the friction of using tmux in the first place, I made up a zsh plugin named [tm.zsh](https://github.com/kjhaber/tm.zsh) to make it easy to list, create, and attach to tmux sessions.
* At the zsh shell prompt, type `tm` by itself to see the list of sessions
* Type `tm blah` to connect to a tmux session named "blah", creating the session if it doesn't already exist.  If you're already in another tmux session, it will also switch to that session.
* It works with zsh plugin managers (I use [zcomet](https://github.com/agkozak/zcomet) nowadays), but you can also just download it and source it in your `~/.zshrc`.  The whole [script](https://github.com/kjhaber/tm.zsh/blob/215c07276ca8225156803b536cb49b851b84215a/tm.zsh) is 41 lines including comments and blank lines.
* It also includes zsh tab autocomplete for tmux session names.  Using a zsh plugin manager should set up autocomplete for you, or you can add the 5-line [autocomplete config](https://github.com/kjhaber/tm.zsh/blob/215c07276ca8225156803b536cb49b851b84215a/_tm) to your zsh config's $fpath manually.


## Using backtick as the action trigger key
By default tmux actions are triggered by the `ctrl-b` keystroke, followed by some action key.  I changed my tmux action key to the backtick key (\`) to reduce the amount of chording I need to do (so my pinkie finger takes a little less abuse), and I've found it minimizes conflict with keybindings in other programs.

The downside is that if you want to type a backtick character while in a tmux session, you have to press the backtick character twice.  I'm very used to that now, but it might not be for everyone.

For the rest of this post I'll refer to `<action>` when talking about tmux keybindings.


## More memorable shortcuts for splits
The default tmux keybindings for splitting panes are `<action> %` for horizontal split and `<action "` for vertical split.  I never memorized these keybindings and don't use them.  Instead I use:
* `<action>|` (bar character) for horizontal split
* `<action>-` (dash character) for vertical split

I think this remapping is pretty common but it's made a big difference for me using tmux.

(Aside: I set up the same leader mapping for splitting buffers in vim: `<leader>|` and `<leader>-`.  Like with tmux it's much easier for me to remember than the defaults, and being consistent between both vim and tmux reduces cognitive load and friction.)


## Vim-aware pane switching
Speaking of friction with tmux vs. vim, I use vim as my main editor, which - like tmux and many other editors - can split windows into multiple panels.  Vim maps h/j/k/l keys to left/down/up/right, respectively, so I'm very used to using h/j/k/l keys to move around.  But when you start mixing vim split windows within tmux split windows, it can get thorny to navigate.

https://github.com/christoomey/vim-tmux-navigator provides a vim plugin that (combined with some tmux config) makes <ctrl-h/j/k/l> keys move focus between tmux and vim splits seamlessly.  This makes moving focus around in shells and editors super fast.  I love this as another way to reduce friction and cognitive load when working in tmux and vim.

I did run into a couple of gotchas with mapping <ctrl-h>, which traditionally sends the same control character as Backspace:
* In iTerm2 I needed to map <ctrl-h> to a CSI code `[104;5u` in order to trigger <ctrl-h> mappings. [notes](https://github.com/kjhaber/dotfiles/blob/27ae686b64e50e607e3f8bc696f91b1e5322275d/.config/zsh/autoload/ctrl-h-widget.zsh#L9-L11)
* I use vi mode in zsh (naturally), and in that mode <ctrl-h> triggers a backspace or moving left 1 character depending on whether the prompt is in command mode.  To get around that I defined a custom [zle widget](https://github.com/kjhaber/dotfiles/blob/27ae686b64e50e607e3f8bc696f91b1e5322275d/.config/zsh/autoload/ctrl-h-widget.zsh) to remap ctrl-h in my zsh configs.


## tmux-editpanecontent (tmux plugin)
Tmux has a copy-paste mode for grabbing terminal output to clipboard, but it's always felt unnatural to me and I never got the hang of using it - even though there is a vim mode to make it easier for vim users like me.  Instead of practicing and learning tmux copy mode, I made up a tmux plugin named [tmux-editpanecontent](https://github.com/kjhaber/tmux-editpanecontent) that captures the content of the currently selected pane and opens a split with the content in an editor.  Now if I want to copy something from my terminal scrollback, it's super easy to pop the whole terminal session into vim and work with it as a normal file.
* Use `<action>e` to open a horizontal split with tmux content in your editor
* Install with tmux plugin manager [tpm](https://github.com/tmux-plugins/tpm)


## Conclusion
There are more things in my tmux configs that might be of interest, but I think the above items are the most interesting and have the most impact on my day-to-day terminal usage.  If you've read this far, I hope you find something here useful.

