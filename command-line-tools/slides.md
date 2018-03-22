# Command-line tools

## whoami

@aanton

## Agenda

* [Zsh](#zsh)
* [Oh my Zsh](#ohmyzsh)
* [Z](#z)
* [fd](#fd)
* [ripgrep](#ripgrep)
* [fzf](#fzf)
* [Clipboards](#clipboards)

---

layout: true

# Zsh

---

name: zsh

Zsh is a UNIX command interpreter (shell) usable as an interactive login shell and as a shell script command processor. Of the standard shells, zsh most closely resembles ksh but includes many enhancements.

Zsh has command line editing, builtin spelling correction, programmable command completion, shell functions (with autoloading), a history mechanism, and a host of other features.

Its configuration is usually managed in the `.zshrc` file.

---

## Installation

* Check if already installed & install if necessary
* Make it your default shell (logout is required)

```bash
cat /etc/shells
sudo apt install zsh

chsh -s $(which zsh)
```

---

## Scripting

The zsh scripting is very similar to bash with only small differences, but the differences are large enough that migrating scripts is not trivially easy. 

Bash is recommended for scripting. Use **shebang** in your scripts!

---

## Context based completion & navigation

* Context based `<TAB>` completion
  * Filenames/Directories: `cat re<TAB>`
  * Directories (only): `cd re<TAB>`
    * Fuzzy matching to extend directories: `cd /p/l/p<TAB>` expands to `cd /projects/laravel/public`
  * Commands arguments: `git c<TAB>`
  * Commands parameters: `git commit -<TAB>`
  * Process completion in `kill` command: `kill php<TAB>`
  * Remote filename completion in ssh: `scp remote:<TAB>`
* Navigation using `<TAB>` & `<UP>/<DOWN>/<LEFT>/<RIGHT>`

---

## Globbing (complex pattern matching mechanism)

* Recursive search: `**/`
* Open HTML files anywhere in the hierarchy: `vim **/index.html`
* Search a string in HTML files anywhere in the hierarchy: `cat **/*.html | grep title`
* List all regular files in the hierarchy,: `ls -l **/*(.)`
* List all symlink files in the hierarchy: `ls -l **/*(@)`
* List all directories: `print -l **/*(/)`
* List all PNG files in the hierarchy: `ls -l **/*.png`
* List all PNG & GIF files in the hierarchy: `ls -l **/*.(png|gif)`
* List all files by pattern in the hierarchy: `ls -l **/[a-e]*.png`
* List 10 recent files: `ls -lt **/*(.om[1,10])`
* List 10 bigger files: `ls -lt **/*(.OL[1,10])`
* List empty files: `ls -l **/*(L0)`

---

## Globing & completion

* Expand the matched files/directories in the command-line: `ls -l **/*.png<TAB>`

---

## Special aliases

* Directory aliases
  * Home directory `~`
  * Last directory `-`
  * Upwards `..`, `...`
* Global aliases: words that can be used anywhere on the command line, thus you can give certain files, filters or pipes a short name
* Suffix aliases: tie a file suffix

```bash
alias -g L="| less"
ls -lR L

alias -g SR="| sort | uniq -c | sort -n | tail -10"
fd --type f | awk -F/ '{print $NF}' SR

alias -s txt=code
alias -s html=google-chrome
```

---

## History commands

* Sharing of command history among all running shells
* Write the first characters of a command & use `<UP>` to navigate through the commands in the history that starts with those characters
* Avoid duplicate commands in the history (remove the older duplicated one): `hist_ignore_all_dups`

```bash
setopt hist_ignore_all_dups
```

---

## Miscellaneous

* Change partially the current path: `cd old new`
* Repeat last command with replacements
  * Repeat last command: `r`
  * With replacement: `r old=new`

---

## And much more features ...

* Spelling correction
* Themeable prompts: left & right
* Loadable modules (eg. zmv: massive rename)
* Frameworks (eg. ohmyzsh)

---

## My aliases

```bash
# Alias Git
alias gd="git diff"
alias gs="git status"
alias ga="git add -u"
alias gc="git commit -v"
alias gl="git log --pretty=format:\"%h %ad | %s%d [%an]\" \
  --graph --date=iso --all -20"
alias gll="git log --pretty=format:\"%h %ad | %s%d [%an]\" \
  --graph --date=iso -20"
alias gf="git fetch -p"
alias gpull="git pull --ff-only"
alias gpush="git push origin master"
alias glast="git show -1"

# Alias Docker
alias ddf="docker system df"
alias dip="docker inspect --format \
  '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'"
alias dips="docker inspect --format \
  '{{$e := . }}{{with .NetworkSettings}} {{$e.Name}} \
    {{range $index, $net := .Networks}}{{$index}} {{.IPAddress}} \
    {{end}}{{end}}' \
  $(docker ps -q)"
```

---

## Resources

* [Use Zsh](http://fendrich.se/blog/2012/09/28/no/)
* [Master Your Z Shell with These Outrageously Useful Tips](http://reasoniamhere.com/2014/01/11/outrageously-useful-tips-to-master-your-z-shell/)
* [Ten Zsh tricks (youtube)](https://www.youtube.com/watch?v=avr_sCFKthw)

---

layout: true

# Oh My Zsh

---

name: ohmyzsh

Oh-My-Zsh is an open source, community-driven framework for managing your Zsh configuration. It comes bundled with a ton of helpful functions, helpers, plugins, themes, and a few things that make you shout...

[Project home](http://ohmyz.sh/)

---

## Installation

* Prerequisites: zsh (>= 4.3.9), curl/wget
* Download & install

```bash
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

---

## Themes

* [Official themes](https://github.com/robbyrussell/oh-my-zsh/wiki/themes)
* Many additional themes
  * [Powerlevel9k](https://github.com/bhilburn/powerlevel9k)
  * [Pure](https://github.com/sindresorhus/pure)

```bash
# Install "Powerline-patched font"
sudo apt-get install fonts-powerline

# Test the font
echo "\ue0b0 \u00b1 \ue0a0 \u27a6 \u2718 \u26a1 \u2699"
```

* My favourite theme: [Agnoster](https://github.com/agnoster/agnoster-zsh-theme)
  * Simple configuration to hide my user name

```bash
ZSH_THEME="agnoster"
DEFAULT_USER="aanton"
```

---

## Plugins

* [Official plugins](https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins)
  * composer
  * docker
  * docker-composer
  * git
  * sudo
  * web-search
  * ... and much more
* Additional plugins can be installed
* Plugins must be enabled in the .zshrc file, adding theirs names in the `plugins` parameter

---

layout: true

# Z

---

name: z

Tracks your most used directories, based on 'frecency'. After a short learning phase, z will take you to the most 'frecent' directory that matches ALL of the regexes given on the command line, in order.

> Frecency is any heuristic that combines the frequency and recency into a single measure.

[Github project](https://github.com/rupa/z)

---

## Installation

```bash
# Download to home directory
curl -fsSL https://raw.githubusercontent.com/rupa/z/master/z.sh -o ~/z.sh

# Add to .zshrc or .bashrc
[ -f ~/z.sh ] && source ~/z.sh
```

---

## Usage

```bash
# Will match the most frecency directory from root (/) that contains foo
z foo

# Will match /foo/bar but not /bar/foo
z foo bar
```

---

## Command options

* List only: `z -l`
* Echo the best match, don't cd: `z -e`
* Restrict matches to subdirectories of the current directory: `z -c`
* Match by rank only: `z -r`
* Match by recent access only: `z -t`

---

layout: true

# fd

---

name: fd

A simple, fast and user-friendly alternative to `find`

[Github project](https://github.com/sharkdp/fd)

---

## Installation

* On Debian/Ubuntu download the adequate `.deb` package from the [release page](https://github.com/sharkdp/fd/releases) and install it using `sudo dpkg -i <package>`
* Also can be installed using `cargo`, the Rust package manager: `cargo install fd-find`

---

## Simple syntax

```bash
fd <pattern>

# find alternative
find -iname "*pattern*"
```

---

## Sensible defaults

While it does not seek to mirror all of `find` powerful functionality, it provides sensible (opinionated) defaults for most cases

* Search by regex
* Search matches the file/directory name (not in the path)
* Search in current directory
* Exclude hidden directories & files
* Exclude patterns defined in `.gitignore`
* Do not follow symlinks directories
* Smart case search: the search is case-insensitive by default, but it switches to case-sensitive if the pattern contains an uppercase character

---

## But defaults can be changed

* Search by literal string instead of a regex: `--fixed-strings`
* Search matches the full path (not only in the file/directory name): `--full-path`
* Search in a different path: `fd <pattern> <path>`
* Include hidden directories & files: `--hidden`
* Include patterns defined in  `.gitignore`: `--no-ignore`
* Follow symlinks directories: `--follow`
* Case sensitive searches: `--case-sensitive`
* Case insensitive searches: `--ignore-case`

```bash
# Search following symlinks, including hidden directories/files & .gitignore patterns
fd --follow --hidden --no-ignore <pattern>
```

---

## Filter search results

* Filter search results by filetype: `--type <file|directory|symlink>`
* Filter search results by extension: `--extension <extension>`

```bash
# Search only PHP files
fd --type file --extension php <pattern>
```

---

## Execute commands

* Execute a command in parallel for each search result: `--exec <command>`
* Available tokens can be used in the command
  * `{}`: places the input in the location of this token
  * `{.}`: like `{}` but without the file extension
  * `{/}`: places the basename of the input
  * `{/.}`: places the basename of the input, without the extension
  * `{//}`: places the parent of the input

```bash
# Convert all JPG files to PNG files
fd --extension jpg --exec convert {} {.}.png
```

---

## Easy to remember options

* All options & flags have an one-character alternative & are easy to remember
  * `--hidden`: `-H`
  * `--no-ignore`: `-I`
  * `--type file`: `-t f`
  * `--exec <command>`: `-x <command>`

---

## And more features ...

* Exclude entries that match the given *glob pattern*: `--exclude`
* In the search results, show the full path from the root: `--absolute-path`
* Using fd with `xargs` or `parallel` requires to separate the results by the `NULL` character (instead of newlines): `--print0`

```bash
# Search including hidden directories & files, but exclude .git
fd --hidden --exclude .git <pattern>

# Show the number of lines of all TXT files (it executes a unique "wc -l" in all results)
fd --extension txt --print0 | xargs -0 wc -l

# Alternative using --exec (the result is different, it executes a "wc -l" in each result)
fd --extension txt --print0 --exec wc -l {}
```

---

layout: true

# ripgrep (rg)

---

name: ripgrep

ripgrep is a line-oriented search tool that recursively searches your current directory for a regex pattern. It is similar to other popular search tools like The Silver Searcher, ack and grep

[Github project](https://github.com/BurntSushi/ripgrep)

---

## Installation

* On Debian/Ubuntu download the adequate `.deb` package from the [release page](https://github.com/BurntSushi/ripgrep/releases) and install it using `sudo dpkg -i <package>`
  * On Ubuntu it can be installed from the `snap` store: `sudo snap install rg`
* Also can be installed using `cargo`, the Rust package manager: `cargo install ripgrep`

---

## Simple syntax

```bash
rg <pattern>
```

---

## Sensible defaults

* Regex search
  * But features like backreferences and arbitrary lookaround are not supported
* Search in current directory
* Respect `.gitignore`
* Exclude hidden directories & files
* Exclude binary files
* Do not follow symlinks directories
* Case sensitive search

---

## But defaults can be changed

* Search by literal string instead of a regex: `--fixed-strings` or `-F`
* Search in a different path: `rg <pattern> <path>`
* Include hidden directories & files: `--hidden`
* Include patterns defined in  `.gitignore`: `--no-ignore`
* Include binary files: `--text` or `-a`
* Follow symlinks directories: `--follow` or `-L`
* Case sensitive searches (default): `--case-sensitive` or `-s`
* Case insensitive searches: `--ignore-case` or `-i`
* Smart case search (the search is case-insensitive if all characters are lowercase, otherwise it is case-sensitive): `--smart-case` or `-S`

```bash
# Search following symlinks, including hidden directories/files & .gitignore patterns
rg --follow --hidden --no-ignore <pattern>
```

---

## Filter search files

* Filter by file type (a type is related to a list of file extensions): `-t <type>`
  * Exclude by file type: `-T <type>`
  * List all available types: `rg --type-list`
* Include or exclude files and directories for searching that match the given glob: `-g <glob>`
  * Precede a glob with a `!` to exclude it

```bash
# Search in TXT files
rg -t txt <pattern> # TXT type is configured to search *.txt
rg -g "*.txt" <pattern>
```

---

## Display options

* Print each file that would be searched without actually performing the search: `--files`
* Print only matched files: `--files-with-matches` or `-l`
* Print matched files with the number of occurrences (lines): `--count` or `-c`
* Show lines before & after each match: `--context <num>` or `-C <num>`
  * Show lines before each match: `-B <num>`
  * Show lines after each match: `-A <num>`

---

## And more features ...

* Invert matching: `-v` or `--invert-match`
* Search in compressed files: `-z` or `--search-zip`

---

layout: true

# fzf

---

name: fzf

A command-line fuzzy finder. An interactive Unix filter for command-line that can be used with any list: files, command history, processes, hostnames, git commits, etc.

It is really fast & portable.

[Github project](https://github.com/junegunn/fzf)

---

## Installation & upgrading

```bash
# Installation
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install

# Upgrading
cd ~/.fzf && git pull && ./install
```

---

## Command-line usage

fzf will launch interactive finder, read the list from STDIN, and write the selected item to STDOUT. Without STDIN pipe, fzf will use find command to fetch the list of files excluding hidden ones (the default command can be modified with `FZF_DEFAULT_COMMAND`)

```bash
find -type f | fzf > selected

# Open selected file in vim
vim $(fzf)
```
---

## Using the finder: Sensible defaults

* Fuzzy search
* fzf starts in "extended-search mode" where you can type in multiple search terms delimited by spaces: `^music nirvna smll .mp3$` (yes! some letters were omitted!)
* Searches are "smart case"
* Latin letters are normalized before matching
* Selects only one result
* Results are sorted by its algorithm

---

## Using the finder: But defaults can be changed

* Enable exact search (without "quote" every word): `fzf -e` or `fzf --exact`
* Disable "extended-search mode": `fzf +x` or `fzf --no-extended`
* Case insensitive searches: `fzf -i`
* Case case sensitive searches: `fzf +i`
* Disable latin letters normalization: `fzf --literal`
* Enable multiple selections: `fzf -m`
* Do not sort the results (keep input order): `fzf +s` or `fzf --no-sort`

---

## Using the finder: Search syntax

* Fuzzy search: `nirvna smll`
* Exact match token (quoted): `'term`
* Start match token: `^term`
* End match token: `term$`
* Do-not include token: `!term`
* The `|` character acts as an OR operator: `jpg$ | png$ | gif$`

---

## Using the finder: Navigation the results

* Move cursor up/down: `<UP>/<DOWN>`, `<CTRL-J>/<CTRL-K>` or `<CTRL-N>/<CTRL-P>`
* Select the item: `<ENTER>`
* Exit: `<ESC>/<CTRL-C>/<CTRL-G>`
* Mouse can be also used: scroll, click, double-click
* On multi-select mode mark multiple items: `<TAB>` & `<SHIFT-TAB>`

---

## Layout & preview

* By default starts in fullscreen mode, but you can make it start below the cursor using `--height` option
* By default the layout is "bottom-up", but you can make it "top-down" layout using `--reverse` option

```bash
find -type f | fzf --height 40% --reverse
```

* Preview window is available with the `preview` option
  * fzf automatically starts external process with the current line as the argument and shows the result in the split window
    * Recommendation for a good performance: use `head -20` for files & `tree -C | head -10` for directories
  * Its position is configured with the `--preview-window` option

---

## Key bindings for command line

The install script will setup the following key bindings for bash, zsh, and fish.

* `<CTRL-T>`: Paste the selected files and directories onto the command line
* `<CTRL-R>`: Paste the selected command from history onto the command line
  * Sorting is disabled by default to respect chronological ordering
  * You can dynamically enable sorting by pressing `<CTRL-R>` again, but if you like it to be enabled by default, add `--sort` to `FZF_CTRL_R_OPTS`
* `<ALT-C>`: cd into the selected directory

---

## Fuzzy completion for bash & zsh

* Files & directories
  * Fuzzy completion for files & directories can be triggered if the word before the cursor ends with the trigger sequence (default `**`)
  * Do not confuse with the "recursive glob" pattern: `**/` used in zsh
* Process IDs for `kill` command
* Host names (names are extracted from `/etc/hosts` & `~/.ssh/config`)

```bash
# Files under current directory (multiple selection)
vim **<TAB>
# Files under ~/github directory that matches pattern (multiple selection)
vim ~/github/pattern**<TAB>

# Directories under current directory (single selection)
cd **<TAB>
# Directories under ~/github directory that matches pattern (single selection)
cd ~/github/pattern**<TAB>

# Search processes (no trigger sequence)
kill -9 <TAB>

# Search hostnames
ssh **<TAB>
```

---

## Customizations

* A few environment variables are available to configure fzf:
  * The default command: `FZF_DEFAULT_COMMAND`
  * The default options: `FZF_DEFAULT_OPTS`
  * The command & options for each command-line key binding (eg. `FZF_CTRL_T_COMMAND`, `FZF_CTRL_T_OPTS`)
  * The completion options & trigger

---

## My fzf customizations

* Multiple selections by default
* Preview files & directories at 40% right, but not in "history navigation"
* When using `<CTRL+T>` to search files, bind `<CTRL+X>` to open selection in vscode
* Integrate with [fd](#fd): a simple, fast and user-friendly alternative to find
  * Exclude patterns defined in `.gitignore` (default), include hidden files & directories, exclude .git & follow symlinked directories
* Add `<CTRL+P>` key binding (similar than SublimeText & vscode) in Zsh
* Custom aliases using fzf & git

---

## My fzf customizations

```bash
export FZF_DEFAULT_OPTS="--multi --reverse --border --inline-info \
  --preview '([ -e {} ] && (head -20 {} || tree -C {} | head -20) || (echo {})) 2> /dev/null' \
  --preview-window=right:40%:wrap"
export FZF_CTRL_R_OPTS="--no-preview"
export FZF_CTRL_T_OPTS="--bind 'ctrl-x:execute(code -r {})'"

# fzf + fd
export FZF_DEFAULT_COMMAND="(fd --hidden --exclude .git --follow || \
  git ls-tree -r --name-only HEAD || \
  (find . -path \"*/\.*\" -prune -o -type f -print -o -type l -print | sed s/^..//)) \
  2> /dev/null"
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"

# fzf bindigs
bindkey '^P' fzf-file-widget

# fzf + git
alias gshow="git show \$(git log --pretty=oneline | fzf +m | awk '{print \$1}')"
alias gbranch="git checkout \$(git branch -vv | fzf +m | awk '{print \$1}')"
alias grebase="git rebase -i \$(git log --pretty=oneline | fzf +m | awk '{print \$1}')^"
```

---

## Resources

* [Tips key bindigns](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings)
* [fzf examples](https://github.com/junegunn/fzf/wiki/examples)

---

layout: true

# Clipboards

---

name: clipboards

## Understanding clipboards

There are three "clipboards" defined in the "Inter-Client Communication Conventions Manual": `PRIMARY`, `SECONDARY` (used inconsistently) & `CLIPBOARD`.

The majority of programs for Xorg (the most popular display server among Linux users) treat the the `PRIMARY` and `CLIPBOARD` selections in the following way:

* The `CLIPBOARD` selection is used for explicit copy/paste commands involving keyboard shortcuts or menu items
  * It behaves like the single-clipboard system on Windows
  * It can also handle multiple data formats
* The `PRIMARY` selection is used for the currently selected text, even if it is not explicitly copied, and for middle-mouse-click pasting
  * Nothing is copied anywhere until it is pasted. If you select some word in a terminal window, close the terminal and then want to paste it somewhere else, it will not work because the terminal is gone and the text has not been copied anywhere
  * In some cases, pasting is also possible with a keyboard shortcut

---

## Terminal emulators

* Terminal emulators already use `<CTRL-C>` & `<CTRL-V>` for theirs own purposes
* Most terminal emulators supports the key combinations `<CTRL-INSERT>` to copy and `<SHIFT-INSERT>` to paste
  * These shorcuts are also supported by other applications
  * But `PRIMARY` or `CLIPBOARD` selection ?
* Gnome Terminal
  * Copy & paste to/from the `CLIPBOARD` selection is done using `<CTRL-SHIFT-C>`/`<CTRL-SHIFT-V>`
  * Paste from the `PRIMARY` selection: `<SHIFT-INSERT>`
  * Paste from the `CLIPBOARD` selection: `<CTRL-SHIFT-INSERT>` !!
  * Copy to the `CLIPBOARD` selection: `<CTRL-INSERT>` !!

---

## Command line copy/paste to/from the clipboards

TODO: xclip & xsel

---

## Clipboard managers

* Enable users to manipulate the clipboard
* Store all the clipboard history and offers an interface to select an ocurrence
* Optional text search (fixed, regex or fuzzy)
* Optional `PRIMARY` and `CLIPBOARD` selections synchronization
* GUI & CLI modes

My clipboard manager is [ClipIt](https://sourceforge.net/projects/gtkclipit/)

---

## Resources

* https://wiki.archlinux.org/index.php/Clipboard
* https://fernandobasso.github.io/en/shell/copy-paste-from-command-line-xclip-xsel-clipboard.html

---

layout: false

# Command-line tools

## Maybe in a near future

* awk & sed
* Bash scripting
* curl & httpie
* Git searches
* jq
* PGP
* SSH

## Thanks ! Feedback is much appreciated !
