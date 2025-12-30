+++
date = '2025-12-29T23:39:29-08:00'
title = 'Using a Bare Repository to Manage Dotfiles'
+++


When I first started managing my [dotfiles](https://github.com/kjhaber/dotfiles) in git, I followed the example from a [blog I found](https://blog.smalleycreative.com/using-git-and-github-to-manage-your-dotfiles) that proposed managing dotfiles in a `~/dotfiles` directory/Git repo, then using a `makesymlinks.sh` shell script to create symlinks to the "real" file locations expected by zsh, vim, tmux, etc. such as `~/.zshrc`, `~/.vimrc`, and so on. My version of `makesymlinks.sh` was rather clunky for new machine installs, and not especially well-maintained. I installed rarely enough that hand-managing the symlinks was tedious but manageable. But I used that approach to git-managing my dotfiles successfully for many years. (From 2016 to 2023 if `git log` is to be believed.) I occasionally looked at other dotfile symlink management solutions like [GNU Stow](https://www.gnu.org/software/stow) but they never quite clicked for me.

At some point in 2023 I stumbled across https://www.atlassian.com/git/tutorials/dotfiles which described managing dotfiles using a Git feature I hadn't heard of called bare repositories. The original article does a good job explaining, and credits a Hacker News post with the initial idea.

The short version is this: Git repositories normally store their revision and branch metadata in a directory named `.git` located at the repository's root directory. Bare repositories let you give the repo metadata directory whatever name and location you want. So the plan for dotfiles is use a bare repository to make my $HOME directory a git repository, with the Git metadata stored somewhere other than `~/.git`.

In my zsh configs I have an [alias](https://github.com/kjhaber/dotfiles/blob/aeb36f9da60ddc9e9ac0c74309d7157086a4a2ee/.config/zsh/aliases.zsh#L8) called `gitdotfile` - it is defined as `alias gitdotfile='git --git-dir=$HOME/.config/meta/repo/ --work-tree=$HOME'`. To do Git operations on my dotfiles, I use that alias instead: `gitdotfile status`, `gitdotfile add .zshrc`, `gitdotfile commit`, `gitdotfile push`, `gitdotfile pull`. And I don't use symlinks for dotfiles at all anymore: I work directly on `~/.zshrc`, `~/.tmux.conf`, etc.

For that to work, I can't use a plain `git clone` to pull my repo onto a new machine. The command I use ends up being `git clone --bare https://github.com/kjhaber/dotfiles "$HOME/.config/meta/repo"` (In reality I use an install shell script that runs this command, mentioned in the repo [README](https://github.com/kjhaber/dotfiles/blob/main/.github/README.md). That's another useful trick I suppose: instead of keeping the dotfile repo README.md in the root of my $HOME directory, I keep it under the .github directory which still makes it visible on the Github repo main page.)

The other important Git feature needed is to disable showing untracked files, which is activated with the command `gitdotfile config --local status.showUntrackedFiles no`. This keeps `gitdotfile` commands from offering to manage every single file in my home directory, only the files I've explicitly added. (Setting this is also handled by the install script.)

A minor downside of this approach is losing vim's ability to work with git commits on the dotfiles. Vim normally autodetects when code is Git-managed, but doesn't do so with my dotfiles' bare repos. There might be a way to get it to work, but since I'm used to Git's CLI it hasn't bothered me enough to look into it. If you use Git UIs instead of the regular Git CLI, using them with this bare repo approach might not work well.

On the other hand, not having a `.git` directory in my $HOME directory means vim and other tools don't get confused when my normal Git-managed code projects are nested within $HOME's git repository. So in practice I've found this approach works well.
