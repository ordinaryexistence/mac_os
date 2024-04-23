SEE ALSO: https://github.com/geerlingguy/mac-dev-playbook

_Last performed on (`sw_vers`):_

```bash
sw_vers

ProductName:		macOS
ProductVersion:		14.3
BuildVersion:		23D56
```

# Standards & Conventions

- Use `[shift] + [control]` as the prefix for all custom keyboard shortcuts

# Baseline System Setup & Configuration 

- [ ] OS
  - Prerequisites
    - Wi-Fi network
    - Apple ID for iCloud, the App Store, etc. (not absolutely required at setup, but will need eventually)
  - [ ] Setup Assistant
    - [ ] Language [English]
    - [ ] Country or Region [United States]
    - [ ] Written and Spoken Languages [Accept Defaults]
    - [ ] Accessibility [Not Now]
        - [ ] Vision
        - [ ] Motor
        - [ ] Hearing
        - [ ] Cognitive
    - [ ] Data & Privacy (notice)
    - [ ] Connect to a Wi-Fi network
    - [ ] Migration Assistant [Not Now]
    - [ ] Sign In with Your Apple ID
        - [ ] FileVault
    - [ ] Terms & Conditions [Agree]
    - [ ] Create a Computer Account
    - [ ] Enable Location Services (Yes)
    - [ ] Analytics (uncheck all)
    - [ ] Screen Time [Set Up Later]
    - [ ] Siri (disable)
    - [ ] Choose Your Look [Auto]
  - [ ] Update OS
    ```
    sudo softwareupdate --install --all --restart
    ```
  - [ ] Complete iCloud configuration
    ```
    open /System/Library/PreferencePanes/AppleIDPrefPane.prefPane
    ```
	- [ ] Advanced Data Protection
	- [ ] [TODO] Enumerate additional features, i.e. mail, calendars, messages, etc
  - [ ] Get Serial Number and check warranty registration at https://checkcoverage.apple.com/
    ```
    system_profiler SPHardwareDataType | grep Serial
    ```

---

# Advanced System Setup & Configuration

- [ ] Enable Touch ID for sudo commands
  Modify or create `/etc/pam.d/sudo_local`
  Ensure `sudo_local` contains the following:
  ```
  # sudo_local: local config file which survives system update and is included for sudo
  # uncomment following line to enable Touch ID for sudo
  auth sufficient pam_tid.so
  ```
  Ensure that the `/etc/pam.d/sudo` file contains the following line:
  ```
  auth include sudo_local
  ```
  __tl;dr__
  ```
  sudo cp -p /etc/pam.d/sudo_local.template /etc/pam.d/sudo_local
  sudo cp -p /etc/pam.d/sudo{,.orig}
  ```
  ```
  sudo grep -qxF 'auth       sufficient     pam_tid.so' /etc/pam.d/sudo_local \
  || sudo sed -i '1s/^#//' /etc/pam.d/sudo_local
  ```
  ```
  sudo sed -i '/^auth[[:space:]]*sufficient[[:space:]]*pam_tid.so/!{1 i\auth       sufficient     pam_tid.so}' /etc/pam.d/sudo_local
  ```
  ```
  sudo grep -qxF 'auth include sudo_local' /etc/pam.d/sudo \
  || sudo sed -i '1s/^#//' /etc/pam.d/sudo
  ```
  ```
  sudo sed -i '/^auth[[:space:]]*include[[:space:]]*sudo_local/!{1 i\auth include sudo_local}' /etc/pam.d/sudo
  ```
- [ ] Install Xcode Command Line Tools
  ```
  xcode-select --install
  xcode-select -p
  ```
- [ ] [TODO] Clone (public) setup repository with setup scripts
  ```
  git clone https://github.com/ordinaryexistence/macOS-setup
  ```
	- [ ] Run setup scripts from repo (this will replace much of what comes below this in this document)
    ```
    cd macOS-setup
    TBD...
    ```
- [ ] Configure `defaults`
  ```
  defaults delete com.apple.dock persistent-apps
  defaults delete com.apple.dock persistent-others
  defaults write com.apple.Dock showhidden -bool yes
  killall Dock
  ```
- [ ] Configure terminal
  - [ ] [TODO] Preferrences
	- [ ] Base `zsh` configuration
		- [ ] Add `/usr/local/bin` to `$PATH` in `.zprofile`
      ```
      setopt HIST_IGNORE_SPACE
      setopt NO_CLOBBER
      setopt nonomatch
      export PATH="/usr/local/bin:$PATH"
      ```
  TODO:
  ```
  export CVSROOT=":pserver:ifoutch@cvs:2401/usr/local/cvsroot"

  eval $(ssh-agent -s)
  ssh-add --apple-use-keychain

  alias history='history 0'
  ```
      
- [ ] Set power saver options, systemsetup & pmset
  Check capabilities
  ```
  pmset -g cap
  ```
  See settings
  ```
  pmset -g
  ```
  Set display sleep
  ```
  sudo pmset -b displaysleep 5
  sudo pmset -c displaysleep 15
  ```

---

# Applications

## Homebrew Applications

_Prerequisites to everything else_

- [ ] Homebrew, https://brew.sh/
  ```
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
  brew doctor
  ```

- [ ] 1Password 8    https://1password.com/
  ```
  brew install 1password
  ```
	- [ ] Configure  personal login
	- [ ] Configure Webley login

- [ ] [Sophos Endpoint](https://cloud.sophos.com/) - Full malware protection and more
  - Get installer
    - Download from Sophos Central
    - Download from Google Drive
  - Decompress ZIP archive
  - Run installer
     
- [ ] [Cisco AnyConnect Secure Mobility Client](https://drive.google.com/drive/folders/1b--4dSCTPD6BaD-0J3jz7H7nnVhNsffF?usp=drive_link) - VPN Client
  - Download latest installer (4.10.0690 at time of writing)
  - Run installer

_Everything else..._

---
- [TODO] Install from `brew bundle` inventory (this will replace much of what comes below this in this document)
  _Requires `Brewfile`_
  ```
  brew bundle install
  ```
---

- [ ] *$* [1password-cli](https://developer.1password.com/docs/cli) - Command-line interface for 1Password
  ```
  brew install 1password-cli
  ```

- [ ] [Adium](https://www.adium.im/) - Instant messaging application
  ```
  brew install adium
  ```

- [ ] [Anki](https://apps.ankiweb.net/) - Memory training application
  ```
  brew install anki
  ```

- [ ] [AppCleaner](https://freemacsoft.net/appcleaner/) - Application uninstaller
  ```
  brew install appcleaner
  ```

- [ ] [as-tree](https://github.com/jez/as-tree) - Print a list of paths as a tree of paths
  ```
  brew install as-tree
  ```

- [ ] [ascinema](https://asciinema.org) - Record and share terminal sessions
  ```
  brew install asciinema
  ```

- [ ] [asroute](https://github.com/stevenpack/asroute) - CLI to interpret traceroute -a output to show AS names traversed
  ```
  brew install asroute
  ```
- [ ] [Audacity](https://www.audacityteam.org/) - Multi-track audio editor and recorder
  ```
  brew install audacity
  ```

- [ ] AWS * - AWS related tooling, see whats new!
  ```
  brew search aws
  ```
  - Utilities I depend on
    - aws-shell
    - awscli
    - awsume
    - cli53 (not aws*, but related)


- [ ] *$* [Bartender](https://www.macbartender.com/) - Menu bar icon organizer
  ```
  brew install bartender
  ```

- [ ] [bash5](https://www.gnu.org/software/bash/) - Bourne-Again SHell, a UNIX command interpreter
  ```
  brew install bash
  ```

- [ ] [bash-completion@2](https://github.com/scop/bash-completion) - Programmable completion for Bash 4.2+
  ```
  brew install bash-completion@2
  ```

- [ ] [bash-git-prompt](https://github.com/magicmonty/bash-git-prompt) - Informative, fancy bash prompt for Git users
  ```
  brew install bash-git-prompt
  ```

- [ ] [bashdb](https://bashdb.sourceforge.net/) - Bash shell debugger
  ```
  brew install bashdb
  ```

- [ ] [BetterDisplay](https://betterdisplay.pro/) - Display management tool
  ```
  brew install betterdisplay
  ```

- [ ] [Brave](https://brave.com/) - Web browser focusing on privacy
  ```
  brew install brave-browser
  ```

- [ ] [Carbon Copy Cloner](https://bombich.com/) - Hard disk backup and cloning utility (see also: chronosync)
  ```
  brew install carbon-copy-cloner
  ```

- [ ] [cheatsheet](https://www.mediaatelier.com/CheatSheet/) - Tool to list all active shortcuts of the current application
  ```
  brew install cheatsheet
  ```

- [ ] [ChronoSync](https://www.econtechnologies.com/) - Synchronization and backup tool (see also: carbon-copy-cloner)
  ```
  brew install chronosync

- [ ] [coreutils](https://www.gnu.org/software/coreutils) - GNU File, Shell, and Text utilities
  ```
  brew install coreutils
  ```

- [ ] _$_ [CoScreen](https://www.coscreen.co/) - Collaboration tool with multi-user screen sharing
  SEE ALSO: Tuple
  ```
  brew install coscreen
  ```

- [ ] [csshx](https://github.com/brockgr/csshx) - Cluster ssh tool for Terminal.app
  ```
  brew install csshx
  ```

- [ ] [cvs](https://www.nongnu.org/cvs/) - Version control system
  ```
  brew install cvs
  ```

- [ ] [cvs-fast-export](http://www.catb.org/~esr/cvs-fast-export/) - Export an RCS or CVS history as a fast-import stream
  ```
  brew install cvs-fast-export
  ```

- [ ] [cvsutils](https://www.red-bean.com/cvsutils/) - CVS utilities for use in working directories
  ```
  brew install cvsutils
  ```

- [ ] [cvsync](https://www.cvsync.org/) - Portable CVS repository synchronization utility
  ```
  brew install cvsync
  ```

- [ ] [Cyberduck](https://cyberduck.io/) - Server and cloud storage browser (see also: forklift)
  ```
  brew install cyberduck
  ```

- [ ] [direnv](https://direnv.net/) - Load/unload environment variables based on $PWD
  ```
  brew install direnv
  ```

- [ ] [Discord](https://discord.com/) - Voice and text chat software
  ```
  brew install discord
  ```

- [ ] docker* - Docker related tooling...
  ```
  brew search docker
  ```
  - Utilities I depend on
    - docker
    - docker-completion
    - docker-compose
    - docker-ls

- [ ] [Dropbox Uploader](https://www.andreafabrizi.it/2016/01/01/Dropbox-Uploader/) - Bash script for interacting with Dropbox
  ```
  brew install dropbox-uploader
  ```

- [ ] _$_ [Evernote](https://evernote.com/) - App for note taking, organizing, task lists, and archiving
  SEE ALSO: Logseq, Notion, Obsidian
  ```
  brew install evernote
  ```

- [ ] *$* [Fantastical](https://flexibits.com/fantastical) - Calendar software
  ```
  brew install fantastical
  ```

- [ ] [fd](https://github.com/sharkdp/fd) - Simple, fast and user-friendly alternative to find
  ```
  brew install fd
  ```

- [ ] [ffmpeg](https://ffmpeg.org/) - Play, record, convert, and stream audio and video
  ```
  brew install ffmpeg
  ```

- [ ] *$* [Forklift](https://binarynights.com/) - Finder replacement and FTP, SFTP, WebDAV and Amazon s3 client (see also: cyberduck)
  ```
  brew install forklift
  ```

- [ ] [freetds](https://www.freetds.org/) - Libraries to talk to Microsoft SQL Server and Sybase databases
  ```
  brew install freetds
  ```

- [ ] [Gitify](https://github.com/gitify-app/gitify) - App that shows GitHub notifications on the desktop
  ```
  brew install gitify
  ```

- [ ] [gnutls](https://gnutls.org/) - GNU Transport Layer Security (TLS) Library
  ```
  brew install gnutls
  ```

- [ ] [Google Chrome](https://www.google.com/chrome/) - Web browser
  ```
  brew install google-chrome
  ```

- [ ] [Handbrake](https://handbrake.fr/) - Open-source video transcoder
  ```
  brew install handbrake
  ```

- [ ] [hazeover](https://hazeover.com/) - Windows manager and desktop organiser
  ```
  brew install hazeover
  ```

- [ ] [htop](https://htop.dev/) - Improved top (interactive process viewer)
  ```
  brew install htop
  ```

- [ ] [iam-policy-json-to-terraform](https://github.com/flosell/iam-policy-json-to-terraform) - Convert a JSON IAM Policy into terraform
  ```
  brew install iam-policy-json-to-terraform
  ```

- [ ] [iterm2](https://www.iterm2.com/) - Terminal emulator as alternative to Apple's Terminal app
  ```
  brew install iterm2
  ```

- [ ] [jq](https://jqlang.github.io/jq/) - Lightweight and flexible command-line JSON processor
  ```
  brew install jq
  ```

- [ ] [jql](https://github.com/yamafaktory/jql) - JSON query language CLI tool
  ```
  brew install jql
  ```

- [ ] [KeyCue](https://www.ergonis.com/products/keycue) - Finds, learns and remembers keyboard shortcuts
  ```
  brew install keycue
  ```

- [ ] [Keybase](https://keybase.io/) - End-to-end encryption software
  ```
  brew install keybase
  ```

- [ ] [KeyboardCleanTool}(https://folivora.ai/keyboardcleantool) - Blocks all Keyboard and TouchBar input
  ```
  brew install keyboardcleantool
  ```

- [ ] [Little Snitch](https://www.obdev.at/products/littlesnitch/index.html) - Host-based application firewall
  SEE ALSO: radio-silence, lulu
  ```
  brew install little-snitch
  ```

- [ ] [Logseq](https://github.com/logseq/logseq) - Privacy-first, open-source platform for knowledge sharing and management
  SEE ALSO: Evernote, Notion, Obsidian
  ```
  brew install logseq
  ```

- [ ] _$_ [Loom](https://www.loom.com/) - Screen and video recording software
  ```
  brew install loom
  ```

- [ ] [Lulu](https://objective-see.com/products/lulu.html) - Open-source firewall to block unknown outgoing connections
  SEE ALSO: little-snitch, radio-silence
  ```
  brew install lulu
  ```

- [ ] [mas](https://github.com/mas-cli/mas) - Mac App Store command-line interface
  __NOTE: Recent updates to macOS continue to break functionality of this utility, making it difficult to depend on.__
  ```
  brew install mas
  ```

- [ ] [masscan](https://github.com/robertdavidgraham/masscan/) - TCP port scanner, scans entire Internet in under 5 minutes
  ```
  brew install masscan
  ```

- [ ] [Microsoft Remote Desktop](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac) - Remote desktop client
  ```
  brew install microsoft-remote-desktop
  ```

- [ ] [Microsoft Visual Studio Code](https://code.visualstudio.com/) - Open-source code editor
  ```
  brew install visual-studio-code
  ```

- [ ] [miro](https://miro.com/) - Online collaborative whiteboard platform
  ```
  brew install miro
  ```

- [ ] [mtr](https://www.bitwizard.nl/mtr/) - 'traceroute' and 'ping' in a single tool
  ```
  brew install mtr
  ```

- [ ] [Mozilla Firefox](https://www.mozilla.org/firefox/) - Web browser
  ```
  brew install firefox
  ```

- [ ] [MySQL Workbench](https://www.mysql.com/products/workbench/) - Visual tool to design, develop and administer MySQL servers
  SEE ALSO: navicat-*, razorsql
  ```
  brew install mysqlworkbench
  ```

- [ ] [mysql-client](https://dev.mysql.com/doc/refman/8.3/en/) - Open source relational database management system
  ```
  brew install mysql-client
  ```
  
- [ ] [nmap](https://nmap.org/) - Port scanning utility for large networks
  ```
  brew install nmap
  ```

- [ ] [notion](https://www.notion.so/) - App to write, plan, collaborate, and get organized
  SEE ALSO: Evernote, Logseq, Obsidian
  ```
  brew install notion
  ```

- [ ] [Obsidian](https://obsidian.md/) - Knowledge base that works on top of a local folder of plain text Markdown files
  SEE ALSO: Evernote, Logseq, Notion
  ```
  brew install obsidian
  ```

- [ ] _$_ [Pandora](https://www.pandora.com/desktop) - Desktop client for the Pandora web radio service
  ```
  brew install pandora
  ```

- [ ] [pcre](https://www.pcre.org/) - Perl compatible regular expressions library
  ```
  brew install pcre
  ```

- [ ] [pcre2](https://www.pcre.org/) - Perl compatible regular expressions library with a new API
  ```
  brew install pcre2
  ```

- [ ] [pipenv](https://github.com/pypa/pipenv) - Python dependency management tool
  SEE ALSO: pyenv
  ```
  brew install pipenv
  ```

- [ ] _$_ [Postman](https://www.postman.com/) - Collaboration platform for API development
  ```
  brew install postman
  ```

- [ ] [powerlevel10k](https://github.com/romkatv/powerlevel10k) - Theme for zsh
  SEE ALSO: starship
  ```
  brew install powerlevel10k
  ```

- [ ] [pstree](https://github.com/FredHucht/pstree) - Show ps output as a tree
  ```
  brew install pstree
  ```

- [ ] [pyenv](https://github.com/pyenv/pyenv) - Python version management
  SEE ALSO: pipenv
  ```
  brew install pyenv
  ```

- [ ] [readline](https://tiswww.case.edu/php/chet/readline/rltop.html) - Library for command-line editing
  ```
  brew install readline
  ```

- [ ] [Radio Silence](https://radiosilenceapp.com/) - Network monitor and firewall
  SEE ALSO: little-snitch, lulu
  ```
  brew install radio-silence
  ```

- [ ] [Raycast](https://www.raycast.com/) - Control your tools with a few keystrokes
  ```
  brew install raycast
  ```

- [ ] *$* [RazorSQL](https://razorsql.com/) - SQL query tool and SQL editor
  SEE ALSO: mysqlworkbench, navicat-*
  ```
  brew install razorsql
  ```

- [ ] [Rewind](https://www.rewind.ai/) - Record and search your screen and audio
  ```
  brew install rewind
  ```
  _Disable audio capture_

- [ ] [Signal](https://signal.org/) - Instant messaging application focusing on security
  ```
  brew install signal
  ```

- [ ] _$_ [Slack](https://slack.com/) - Team communication and collaboration software
  ```
  brew install slack
  ```

- [ ] *$* [Soulver](https://soulver.app/) - Notepad with a built-in calculator
  ```
  brew install soulver
  ```

- [ ] _$_ [Spotify](https://www.spotify.com/) - Music streaming service
  ```
  brew install spotify
  ```

- [ ] [starship](https://starship.rs) - Cross-shell prompt for astronauts
  SEE ALSO: powerlevel10k
  ```
  brew install starship
  ```

- [ ] [swaks](https://www.jetmore.org/john/code/swaks/) - SMTP command-line test tool
  ```
  brew install swaks
  ```

- [ ] [tastytrade](https://tastytrade.com/technology/) - Desktop trading platform
  ```
  brew install tastytrade
  ```

- [ ] [telnet](https://opensource.apple.com/) - User interface to the TELNET protocol
  ```
  brew install telnet
  ```

- [ ] [terraform*](https://www.terraform.io/) - Tool to build, change, and version infrastructure
  ```
  brew search terraform
  ```
  - Utilities I depend on
    = terraform
    - terraform-docs
    - terraformer
    - terraforming

  _In addition to the dedicated toosl below for managing terraform versions. Remember that you can use `brew` itself to switch between versions._
  _E.G._
  ```
  brew switch terraform 0.11.14
  terraform --version
  ```

- [ ] [Terraform Switcher](https://github.com/warrensbox/terraform-switcher) - Manage multiple terraform versions
  SEE ALSO: tfenv
  ```
  brew install warrensbox/tap/tfswitch
  ```

- [ ] [tfenv](https://github.com/tfutils/tfenv) - Terraform version manager inspired by rbenv
  ```
  brew install tfenv
  ```

- [ ] [Thinkorswim](https://mediaserver.thinkorswim.com/installer/install.html#macosx) - Desktop client for TD Ameritrade trading platform
  ```
  brew install thinkorswim
  ```

- [ ] [tmux](https://tmux.github.io/ - Terminal multiplexer
  ```
  brew install tmux
  ```

- [ ] [tree](https://oldmanprogrammer.net/source.php?dir=projects/tree) - Display directories as trees (with optional color/HTML output)
  ```
  brew install tree
  ```

- [ ] _$_ [Tuple](https://tuple.app/) - Remote pair programming app
  SEE ALSO: CoScreen
  ```
  brew install tuple
  ```

- [ ] [unixodbc](https://www.unixodbc.org/) - ODBC 3 connectivity for UNIX
  ```
  brew install unixodbc
  ```

- [ ] [UTM](https://mac.getutm.app/) - Virtual machines UI using QEMU
  ```
  brew install utm
  ```

- [ ] [VirtualBuddy](https://github.com/insidegui/VirtualBuddy) - Virtualization tool
  ```
  brew install virtualbuddy
  ```

- [ ] VLC, https://www.videolan.org/vlc/
  ```
  brew install vlc
  ```

- [ ] [wget](https://www.gnu.org/software/wget/) - Internet file retriever
  ```
  brew install wget
  ```

- [ ] [Whalebrew](https://github.com/whalebrew/whalebrew) - Homebrew, but with Docker images
  ```
  brew install whalebrew
  ```

- [ ] [WhatsApp](https://www.whatsapp.com/) - Native desktop client for WhatsApp
  ```
  brew install whatsapp
  ```

- [ ] [Wireshark](https://www.wireshark.org) - Graphical network analyzer and capture tool
  ```
  brew install --cask wireshark
  brew install wireshark
  ```

- [ ] [zenmap](https://nmap.org/zenmap/) - Multi-platform graphical interface for official Nmap Security Scanner
  ```
  brew install zenmap
  ```

- [ ] _$_ [Zoom](https://www.zoom.us/) - Video communication and virtual meeting platform
  ```
  brew install zoom
  ```

- [ ] [zsh*] - Additional utilities for zsh
  ```
  brew search zsh
  ```
  - Utilities I depend on
    - zsh-autocomplete
    - zsh-completions
    - zsh-git-prompt

## Apple App Store Applications

- [ ] [Amphetamine](https://apps.apple.com/us/app/amphetamine/id937984704?mt=12) - Powerful keep-awake utility
- [ ] [Amphetamine-Enhancer](https://github.com/x74353/Amphetamine-Enhancer) - Enhances Amphetamine

## Manual Downloads (Web, GitHub, Etc.)

- [ ] [Hocus Focus](https://hocusfoc.us/) - Menu bar utility that hides your inactive windows (see also: hazeover)
- [ ] 
---

# Misc

bash5
Python3
Ruby
Perl
docker/podman

# Application Configurations

## git

### git-config
### aliases
### https
### ssh
### gh
### API
### gitignore
### git-worktree

## Zsh

### zsh Configuration Files

| all users     | user      | login shell | interactive shell | scripts | Terminal.app |
|---------------|-----------|:-----------:|:-----------------:|:-------:|:------------:|
| /etc/zshenv   | .zshenv   |      √      |         √         |    √    |       √      |
| /etc/zprofile | .zprofile |      √      |         x         |    x    |       √      |
| /etc/zshrc    | .zshrc    |      √      |         √         |    x    |       √      |
| /etc/zlogin   | .zlogin   |      √      |         x         |    x    |       √      |
| /etc/zlogout  | .zlogout  |      √      |         x         |    x    |       √      |

By default, zsh will look in the root of the home directory for the user `.z*` files, but this behavior can be changed by setting the `ZDOTDIR` environment variable to another directory.

`zsh` will start with `/etc/zshenv`, then the user’s `.zshenv`. The zshenv files are always used when they exist, even for scripts with the `#!/bin/zsh` shebang.

Next, when the shell is a <b>login</b> shell, `zsh` will run `/etc/zprofile` and `.zprofile`. Then for <b>interactive</b> shells (and login shells) `/etc/zshrc` and `.zshrc`. Then, again for <b>login</b> shells `/etc/zlogin` and `.zlogin`.

Finally, there are zlogout files that can be used for cleanup, when a <b>login</b> shell exits. In this case, the user level `.zlogout` is read first, then the central `/etc/zlogout`. If the shell is terminated by an external process, these files might not be run.

### Apple Provided Configuration Files

`/etc/zprofile` uses `/usr/libexec/path_helper` to set the default `PATH`. 

> The path_helper utility reads the contents of the files `/etc/paths` then in the directories `/etc/paths.d`, read in alpha-numerical order of the filename, and appends their contents to the `PATH` environment variable. Files in these directories should contain one path element per line.

Some tools will edit `/etc/profile` and others will look for the various profile files in a user’s home directory and edit those.

__Re-ordering the `PATH`__

`path_helper` may reorder your `PATH`. Suppose you pre-pended `~/bin` to your `PATH` with `export PATH=~/bin:$PATH`

Then some process launches a subshell which can call `path_helper` again. `path_helper` will ‘find’ your additions to the defined `PATH`, but it will append to the list of default paths from `/etc/paths` and `/etc/paths.d`, changing your order and thus which tools will be used.

A better way to override built-in commands which is not affected by `path_helper` would be to use aliases or functions in your profile.

__What about `MANPATH`?__
This is usually not used on macOS since the the default settings for the man tool are quite flexible. However, if a `MANPATH` environment variable is set when path_helper runs, it will also assemble the command to set the `MANPATH` built in a similar way to the `PATH` from the files `/etc/manpaths` and the directory `/etc/manpaths.d`.

Then `/etc/zshrc` enables UTF–8 with `setopt combiningchars`.

# Known Issues

## Git

```
git
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

Fix (check for system updates that includes xcode, or run the following):
```
xcode-select --install
xcode-select: note: install requested for command line developer tools
```

