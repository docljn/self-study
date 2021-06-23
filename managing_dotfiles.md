# Managing Dotfiles

## Bare git repo



## Dotfile managers



## Manual Sync

- copy your dotfiles into a separate folder which you have set up as a git repo
- complexity: low
- effort: high
- risk: high


## Sources

https://www.atlassian.com/git/tutorials/dotfiles
https://dotfiles.github.io
https://www.freecodecamp.org/news/dive-into-dotfiles-part-2-6321b4a73608/
https://github.com/webpro/awesome-dotfiles
https://medium.com/toutsbrasil/how-to-manage-your-dotfiles-with-git-f7aeed8adf8b
https://news.ycombinator.com/item?id=11070797
https://wiki.archlinux.org/index.php/Dotfiles
https://calvin.me/managing-dotfiles/
https://fedoramagazine.org/managing-dotfiles-rcm/
https://medium.com/@webprolific/getting-started-with-dotfiles-43c3602fd789
https://jratzenboeck.com/tools/2018/04/13/dotfiles.html
https://kalis.me/dotfiles-automating-macos-system-configuration/


### To think about

Ripgrep doesn't automatically discover it - you'll need to set the $RIPGREP_CONFIG_PATH env variable for it to pick it up. I've duffed mine in ~/.config/ripgrep/config to follow the XDG convention.

```bash
# Don't let ripgrep vomit really long lines to my terminal, and show a preview.
--max-columns=150
--max-columns-preview

--smart-case
```
