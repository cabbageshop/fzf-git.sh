fzf-git.sh
==========

bash and zsh key bindings for Git objects, powered by [fzf][fzf].

<img width="1680" alt="image" src="https://user-images.githubusercontent.com/700826/185568470-20d70937-eea4-4274-aec5-14dfe7ee2de6.png">

Each binding will allow you to browse through Git objects of a certain type,
and select the objects you want to paste to your command-line.

[fzf]: https://github.com/junegunn/fzf
[fzf-tmux]: https://github.com/junegunn/fzf/blob/master/bin/fzf-tmux

Installation
------------

1. Install the latest version of [fzf][fzf] (including [fzf-tmux][fzf-tmux])
    * (Optional) Install [bat](https://github.com/sharkdp/bat) for
      syntax-highlighted file previews
1. Source [fzf-git.sh](https://raw.githubusercontent.com/junegunn/fzf-git.sh/main/fzf-git.sh) file from your .bashrc or .zshrc

Usage
-----

### List of bindings

* <kbd>ALT-G</kbd><kbd>ALT-F</kbd> for **F**iles
* <kbd>ALT-G</kbd><kbd>ALT-B</kbd> for **B**ranches
* <kbd>ALT-G</kbd><kbd>ALT-T</kbd> for **T**ags
* <kbd>ALT-G</kbd><kbd>ALT-R</kbd> for **R**emotes
* <kbd>ALT-G</kbd><kbd>ALT-H</kbd> for commit **H**ashes
* <kbd>ALT-G</kbd><kbd>ALT-S</kbd> for **S**tashes
* <kbd>ALT-G</kbd><kbd>ALT-L</kbd> for ref**l**ogs
* <kbd>ALT-G</kbd><kbd>ALT-W</kbd> for **W**orktrees
* <kbd>ALT-G</kbd><kbd>ALT-E</kbd> for **E**ach ref (`git for-each-ref`)

> [!WARNING]
> You may have issues with these bindings in the following cases:
>
> * <kbd>ALT-G</kbd><kbd>ALT-B</kbd> will not work if
>   <kbd>ALT-B</kbd> is used as the tmux prefix
> * <kbd>ALT-G</kbd><kbd>ALT-S</kbd> will not work if flow control is enabled,
>   <kbd>ALT-S</kbd> will freeze the terminal instead
>     * (`stty -ixon` will disable it)
>
> To workaround the problems, you can use
> <kbd>ALT-G</kbd><kbd>*{key}*</kbd> instead of
> <kbd>ALT-G</kbd><kbd>ALT-*{KEY}*</kbd>.
>

> [!WARNING]
> If zsh's `KEYTIMEOUT` is too small (e.g. 1), you may not be able
> to hit two keys in time.

### Inside fzf

* <kbd>TAB</kbd> or <kbd>SHIFT-TAB</kbd> to select multiple objects
* <kbd>ALT-/</kbd> to change preview window layout
* <kbd>ALT-O</kbd> to open the object in the web browser (in GitHub URL scheme)

Customization
-------------

```sh
# Redefine this function to change the options
_fzf_git_fzf() {
  fzf-tmux -p80%,60% -- \
    --layout=reverse --multi --height=50% --min-height=20 --border \
    --border-label-pos=2 \
    --color='header:italic:underline,label:blue' \
    --preview-window='right,50%,border-left' \
    --bind='ctrl-/:change-preview-window(down,50%,border-top|hidden|)' "$@"
}
```

Defining shortcut commands
--------------------------

Each binding is backed by `_fzf_git_*` function so you can do something like
this in your shell configuration file.

```sh
gco() {
  _fzf_git_each_ref --no-multi | xargs git checkout
}

gswt() {
  cd "$(_fzf_git_worktrees --no-multi)"
}
```
