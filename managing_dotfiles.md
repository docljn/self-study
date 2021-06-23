# Managing Dotfiles

## Setup

1. Create the bare repo

```bash
git init --bare ~/.dotfiles
alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
config config status.showUntrackedFiles no
echo "alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'" >> $HOME/.bashrc
```

2. Add files to the repo

```bash
config add .vimrc
config commit -m "Added .vimrc"
```

3. Push the repo to your preferred location e.g. github

```bash
config remote add origin git@github.com:<USERNAME>/<REPONAME>.git
config branch -M main
config push -u origin main

```


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


