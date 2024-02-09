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
      setopt nonomatch
      export PATH="/usr/local/bin:$PATH"
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

# Applications & Application Configurations

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

_Everything else..._

---
- [TODO] Install from `brew bundle` inventory (this will replace much of what comes below this in this document)
  _Requires `Brewfile`_
  ```
  brew bundle install
  ```
---

- [ ] [1password-cli](https://developer.1password.com/docs/cli) - Command-line interface for 1Password
  ```
  brew install 1password-cli
  ```
  
- [ ] [AppCleaner](https://freemacsoft.net/appcleaner/) - Application uninstaller
  ```
  brew install appcleaner
  ```

- [ ] [Bartender](https://www.macbartender.com/) - Menu bar icon organizer
  ```
  brew install bartender
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

- [ ] [ChronoSync](https://www.econtechnologies.com/) - Synchronization and backup tool (see also: carbon-copy-cloner)
  ```
  brew install chronosync
  ```
  
- [ ] Dropbox, https://www.dropbox.com/install
  ```
  brew install dropbox
  ```
  - [ ] Configure  personal login
  - [ ] Configure Webley login

- [ ] [Google Chrome](https://www.google.com/chrome/) - Web browser
  ```
  brew install google-chrome
  ```

- [ ] [Raycast](https://www.raycast.com/) - Control your tools with a few keystrokes
  ```
  brew install raycast
  ```
  
- [ ] [Rewind](https://www.rewind.ai/) - Record and search your screen and audio
  ```
  brew install rewind	
  ```
  _Disable audio capture_

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

## Apple App Store Applications

- [ ] [Amphetamine](https://apps.apple.com/us/app/amphetamine/id937984704?mt=12) - Powerful keep-awake utility
- [ ] [Amphetamine-Enhancer](https://github.com/x74353/Amphetamine-Enhancer) - Enhances Amphetamine

---

# Misc

bash5
Python3
Ruby
Perl
docker/podman
